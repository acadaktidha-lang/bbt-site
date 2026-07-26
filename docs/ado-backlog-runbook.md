# Runbook: reconstruct any local repo as an Azure DevOps board

A repeatable procedure for taking a local git repository and producing, in Azure
DevOps, a complete delivery backlog that maps every commit to a work item, plus
optionally moving the code itself into Azure DevOps Repos.

This is written to be **process agnostic**. Do not assume a project uses Agile,
Scrum or Basic. Phase 2 discovers what is actually there, and every later phase
reads from that discovery. Skipping Phase 2 is the single most common way this
goes wrong.

Placeholders used throughout:

| Placeholder | Meaning | Example |
| --- | --- | --- |
| `<ORG>` | Azure DevOps organization | `contoso` |
| `<PROJECT>` | Project name or id | `contoso.com` |
| `<PROJ_ID>` | Project GUID | `c12395a0-...` |
| `<REPO_ID>` | Repository GUID | `e34c5655-...` |
| `<EMAIL>` | Your Azure DevOps account | `you@example.com` |
| `<PAT>` | Personal Access Token | never commit this |

---

## Phase 0: decide whether you need this

Worth doing when the repository has real history that nobody can see, when a
client or manager wants the work visible on a board, or when you are handing the
project over. Not worth doing for a repo with under about ten commits, where a
board adds ceremony without adding traceability.

Budget roughly one to two hours for a fifty commit repository, most of it in
Phase 3 where you read the history and decide how to group it.

---

## Phase 1: authentication

### 1.1 Pick an auth mode before anything else

The Azure DevOps MCP server (`@azure-devops/mcp`) supports four modes via the
`--authentication` flag:

| Mode | When to use |
| --- | --- |
| `interactive` (default) | Work or school (Entra ID) account, on a desktop with a browser |
| `azcli` | Azure CLI already installed and `az login` already done |
| `envvar` | CI and headless, token in `ADO_MCP_AUTH_TOKEN` |
| `pat` | Personal Microsoft accounts, headless, or anything that must not open a browser |

**The trap.** If your Azure DevOps account is a *personal* Microsoft account
(MSA), for example an outlook.com or gmail backed login, the default
`interactive` mode **cannot work**. Entra rejects it with "You can't sign in here
with a personal account. Use your work or school account instead." Worse, the MCP
tool call does not fail fast: it sits waiting for a sign in that will never
complete and only aborts at the idle timeout, roughly thirty minutes later.
`azcli` hits the same wall for the same reason.

If you are not certain which kind of account you have, assume PAT. It works in
both cases.

### 1.2 Mint a PAT

Go to `https://dev.azure.com/<ORG>/_usersSettings/tokens`, then New Token.

- **Organization:** scope it to the single org, not "all accessible organizations".
- **Expiration:** the shortest that covers the job. 30 days is generous.
- **Scopes:** choose Custom defined, never Full access. Then:

| Scope | Level | Needed for |
| --- | --- | --- |
| Work Items | Read, write, & manage | Creating and updating items. "Read & write" alone is enough to create, but "manage" additionally lets you *delete* a malformed item instead of only editing it. |
| Project and Team | Read | Listing projects, reading process metadata |
| Code | Read & write | **Only if** you also plan Phase 7, pushing the repo |

Copy the token immediately. It is displayed once and never again.

### 1.3 Never paste the token into a chat, ticket, or commit

Anything you paste into an assistant conversation is stored in that transcript.
If it happens anyway, treat the token as burned: finish the task if you must,
then revoke and reissue. Base64 encoding is **not** encryption, it is just the
HTTP Basic transport format, so an encoded token in a config file is as exposed
as a plain one.

Prefer a local script that prompts with `read -s` so the secret never enters
shell history or a transcript:

```bash
read -rs -p "PAT: " ADO_PAT; echo
export ADO_PAT
```

Note the `export`. A bare `VAR=value` line on its own sets a *shell* variable
that child processes never see, which produces a confusing `KeyError` or empty
credential later. Either `export` it, or put the assignment and the command on
one line: `ADO_PAT='...' python3 script.py`.

### 1.4 Configure the MCP server

```bash
claude mcp remove azure-devops -s local 2>/dev/null || true
claude mcp add azure-devops -s local \
  -e "PERSONAL_ACCESS_TOKEN=$(printf ':%s' "$ADO_PAT" | base64 | tr -d '\n')" \
  -- npx -y @azure-devops/mcp <ORG> \
     -d core work work-items repositories wiki \
     --authentication pat
```

The `PERSONAL_ACCESS_TOKEN` value is base64 of `email:pat`. An empty username,
that is `:pat`, works too because Azure DevOps ignores the Basic auth username.

Verify with `claude mcp get azure-devops`. Status should read Connected.

### 1.5 Prove auth works before planning anything

Make one cheap call and confirm it returns. Tools being *loaded* is not the same
as tools being *authenticated*, and conflating the two is how you discover an
auth problem thirty minutes into a hang rather than in two seconds.

```bash
curl -sS -u ":$ADO_PAT" \
  "https://dev.azure.com/<ORG>/_apis/projects?api-version=7.1" \
  -w '\n[status=%{http_code}]\n'
```

Expect 200 and a project list. Record the GUID of your target project as
`<PROJ_ID>`.

---

## Phase 2: discover the process (do not skip)

### 2.1 Get the work item types that actually exist

```bash
curl -sS -u ":$ADO_PAT" \
  "https://dev.azure.com/<ORG>/<PROJ_ID>/_apis/wit/workitemtypes?api-version=7.1" \
 | python3 -c "import json,sys; [print(' -',w['name']) for w in json.load(sys.stdin)['value']]"
```

Read the result against this table to identify the process:

| Types you see | Process | Epic tier | Middle tier | Leaf |
| --- | --- | --- | --- | --- |
| Epic, Issue, Task | **Basic** | Epic | **Issue** | Task |
| Epic, Feature, User Story, Task, Bug, Issue | **Agile** | Epic | **User Story** | Task |
| Epic, Feature, Product Backlog Item, Task, Bug, Impediment | **Scrum** | Epic | **Product Backlog Item** | Task |
| Epic, Feature, Requirement, Task, Bug | **CMMI** | Epic | **Requirement** | Task |

**Critical warning.** Do **not** trust the project property string. Reading
project properties can return `System.Process Template: Scrum` on a project whose
actual types are Epic, Issue and Task, which is Basic. That property is
vestigial and actively misleading. The type list is the truth.

Two consequences worth knowing in advance:

- Basic has **no** `User Story` type at all. If a stakeholder asks for User
  Stories on a Basic project, that is a process migration, not a naming choice.
  See Phase 8.
- Agile *also* has a type called `Issue`, but in Agile it is an impediment that
  does not sit on the backlog. The same word means different things in different
  processes.

### 2.2 Get the states and fields for each tier

```bash
for T in Epic Issue Task; do    # substitute your actual type names
  echo "=== $T ==="
  curl -sS -u ":$ADO_PAT" \
    "https://dev.azure.com/<ORG>/<PROJ_ID>/_apis/wit/workitemtypes/$T?api-version=7.1" \
  | python3 -c "
import json,sys
d=json.load(sys.stdin)
print('  states:', ', '.join(f\"{s['name']}[{s['category']}]\" for s in d.get('states',[])))
f=[x['referenceName'] for x in d.get('fields',[])]
for want in ['System.AssignedTo','Microsoft.VSTS.Common.ClosedBy',
             'Microsoft.VSTS.Common.ClosedDate','System.Description',
             'Microsoft.VSTS.Common.AcceptanceCriteria']:
    print(f'  {want}:', 'YES' if want in f else 'no')
"
done
```

Record three things:

1. **The state names.** Basic uses To Do / Doing / Done. Agile uses New / Active
   / Resolved / Closed. Scrum uses New / Approved / Committed / Done / Removed.
   Never hardcode a triad you did not read from the API.
2. **The initial state**, which is the one in the `Proposed` category. You will
   need it in Phase 5.
3. **Whether `Microsoft.VSTS.Common.AcceptanceCriteria` exists.** Basic has no
   such field, so acceptance text has to live inside `System.Description`.
   Discovering this after you have authored 24 descriptions means rewriting them.

### 2.3 Resolve your own identity

```bash
curl -sS -u ":$ADO_PAT" \
  "https://dev.azure.com/<ORG>/_apis/connectionData?api-version=7.1-preview" \
 | python3 -c "import json,sys; u=json.load(sys.stdin)['authenticatedUser']; print(u.get('providerDisplayName'), '|', u.get('properties',{}).get('Account',{}).get('\$value'))"
```

Use the returned unique name as the `System.AssignedTo` and
`Microsoft.VSTS.Common.ClosedBy` value.

### 2.4 Check what is already on the board

```bash
curl -sS -u ":$ADO_PAT" -H "Content-Type: application/json" \
  -X POST "https://dev.azure.com/<ORG>/<PROJ_ID>/_apis/wit/wiql?api-version=7.1" \
  -d '{"query":"SELECT [System.Id] FROM WorkItems WHERE [System.TeamProject] = @project"}' \
 | python3 -c "import json,sys; w=json.load(sys.stdin).get('workItems',[]); print('count:',len(w)); print([x['id'] for x in w][:50])"
```

If items already exist, inspect them and decide explicitly: leave them alone,
reuse one as your first Epic, or delete. Do not create blindly alongside unknown
items, and do not delete anything without asking the owner first.

---

## Phase 3: build the seed backlog from git history

This is the part that takes judgment. Do it in a file first, not directly against
the API, so it can be reviewed and corrected cheaply.

### 3.1 Extract the history

```bash
git log --reverse --pretty=format:'%h%x09%ad%x09%an%x09%s' --date=short > /tmp/history.tsv
git log --reverse --merges --pretty=format:'%h %s' > /tmp/merges.txt
git status --short                      # uncommitted work is real work, capture it
git branch -a --format='%(refname:short)'
```

### 3.2 Group commits into Epics

Read the subjects in order and cluster them by **theme**, not by date. A good
Epic is a coherent stream of work a stakeholder would recognise as one thing, for
example "Responsive design and mobile experience" or "Performance optimization".
Five to ten Epics suits a fifty commit repo.

Then split each Epic into middle tier items, each one to five commits. Aim for
twenty to thirty total. Fewer and the board says nothing useful, more and it
becomes noise.

### 3.3 Write each item to a fixed shape

Use the same five headings on every item so the board reads consistently:

```markdown
### US-3.4: Professionalize typography and page metadata

- Type: <middle tier type from Phase 2.1>
- State: <target state from Phase 2.2>
- Commits: 5550580, 4c5807e
- What: what changed, in plain language.
- How: the technique or approach used.
- Why: the reason it was needed.
- Acceptance: the observable condition that proves it is finished.
```

The `Commits` line is the entire point. It is what makes the board and the git
history verifiable against each other later.

### 3.4 Handle the awkward history

| Situation | How to record it |
| --- | --- |
| Merge commits | Cite the feature commit and the merge, `12e16eb (merged via PR #8, e4cb449)` |
| Reverted then reapplied | One item covering the whole churn, listing every hash in order, explaining what survived |
| Work with no commit yet | An item in the in-progress state, `Commits: none yet (uncommitted working tree)`, listing the dirty paths |
| Work only evidenced by a file | Cite the artifact, `Commits: see HANDOFF.md` |
| Squashed or rewritten history | Cite the post-rewrite hashes only, and note the rewrite once at the top of the file |

### 3.5 Adopt a writing standard and state it

Pick the conventions the items must follow and write them into the file's header,
for example "no em dashes and no en dashes anywhere". Then enforce it
mechanically in Phase 4. A standard nobody checks is not a standard.

Save the result as `docs/backlog.md`. Commit it. It is the source of truth that
the board is generated from, and it stays useful after the board exists.

---

## Phase 4: validate before writing anything

Write a script that builds the full payload in memory and can run in `--dry-run`
mode. Before the first API write, assert:

1. **Counts match the plan.** N Epics, M children, and every child has a parent.
2. **Every type name exists** in the Phase 2.1 list.
3. **Every state name exists** in the Phase 2.2 list for that type.
4. **The writing standard holds.** Hard fail, do not warn:

   ```python
   # Escapes, not literal characters, so this checker file passes its own check.
   BAD = {'\u2014': 'em dash',     '\u2013': 'en dash',
          '\u2012': 'figure dash', '\u2015': 'horizontal bar'}
   problems = [f"{name} in: {text[:70]}"
               for text in all_titles_and_descriptions
               for ch, name in BAD.items() if ch in text]
   if problems:
       for p in problems: print(' -', p)
       raise SystemExit('ABORT: writing standard violated')
   ```

5. **No field is used that Phase 2.2 said does not exist**, acceptance criteria
   being the usual offender.

Print the whole tree and read it before proceeding.

---

## Phase 5: create the items

### 5.1 The ordering rule that matters most

**Azure DevOps refuses to create a work item directly in a non-initial state.**
Attempting it returns:

```text
The field 'State' contains the value 'Done' that is not in the list of supported values
```

So every item must be **created in the initial state, then transitioned**. This
is the single most common failure in this procedure. Note the asymmetry: an
*update* to any state works fine, it is only *creation* that is restricted.

The working sequence is therefore:

1. Create Epics with title, description and assignee, **no state field**.
2. Create children under each Epic, which also establishes the parent link.
3. Batch update states, assignee and closed-by across everything.

### 5.2 Create the Epics

Use `wit_create_work_item` with type = your Epic tier. Set `System.Title`,
`System.Description` (with `format: "Html"`), `System.AssignedTo`. Omit
`System.State`. Record the returned id for each.

### 5.3 Create children, which gets you parent links free

Use `wit_add_child_work_items` with `parentId` set to the Epic id. It accepts an
array, so one call per Epic covers all its children and each child comes back
already parented. This is much less error prone than creating items separately
and then linking them.

Children are created unassigned and in the initial state. Phase 5.4 fixes both.

### 5.4 Batch the states, assignee and closed-by

Use `wit_update_work_items_batch`. Per item, in this order:

```json
[
  {"id": 9, "op": "Add",     "path": "/fields/System.AssignedTo", "value": "<EMAIL>"},
  {"id": 9, "op": "Replace", "path": "/fields/System.State",      "value": "<completed state>"},
  {"id": 9, "op": "Add",     "path": "/fields/Microsoft.VSTS.Common.ClosedBy", "value": "<EMAIL>"}
]
```

`Microsoft.VSTS.Common.ClosedBy` **is** writable through the API, in the same
request as the state change. `ClosedDate` populates automatically. If a given
process does reject it as read only, drop that one operation and retry, since the
state change author is recorded regardless.

Keep batches to roughly ten to fifteen items. Larger batches succeed but return
responses too big to read, which costs you your confirmation.

### 5.5 Roll parent state up by hand

Nothing does this for you. An Epic must not be marked complete while any child is
still open. Set each Epic's state from its children: completed only if every
child is completed, otherwise in progress.

---

## Phase 6: verify independently

Do not trust the write responses, especially any that were truncated. Re read
everything and assert the invariants.

```bash
IDS=$(python3 -c "print(','.join(map(str,range(1,33))))")   # see note below
curl -sS -u ":$ADO_PAT" \
 "https://dev.azure.com/<ORG>/<PROJ_ID>/_apis/wit/workitems?ids=$IDS&fields=System.WorkItemType,System.Title,System.State,System.AssignedTo,Microsoft.VSTS.Common.ClosedBy,System.Parent&api-version=7.1" \
 | python3 -c "
import json,sys
d=json.load(sys.stdin)['value']; bad=[]
for w in sorted(d,key=lambda x:x['id']):
    f=w['fields']
    a=f.get('System.AssignedTo',{}).get('displayName','MISSING')
    c='yes' if f.get('Microsoft.VSTS.Common.ClosedBy') else '-'
    p=f.get('System.Parent','-'); st=f['System.State']; t=f['System.WorkItemType']
    print(f\"{w['id']:>3} {t:12} {st:8} par={p} closedby={c} {f['System.Title'][:50]}\")
    if a=='MISSING': bad.append(f\"#{w['id']} unassigned\")
    if st=='<completed state>' and c=='-': bad.append(f\"#{w['id']} completed without ClosedBy\")
    if st=='<initial state>': bad.append(f\"#{w['id']} left in initial state\")
    if t!='<epic type>' and p=='-': bad.append(f\"#{w['id']} orphan\")
print('PROBLEMS:', '; '.join(bad) if bad else 'NONE')
"
```

Assert every one of: total count is what you planned, no orphans, nothing left in
the initial state, no unassigned items, every completed item has ClosedBy.

**Build the id list in Python, not with `seq -s,`.** On macOS, `seq -s, 1 32`
emits a *trailing* separator, producing an empty final id and a 400 response
reading "The following Ids are not valid: ."

---

## Phase 7: move the code into Azure DevOps Repos (optional)

### 7.1 Survey both sides first

```bash
git remote -v && git status -sb && git rev-list --count HEAD && du -sh .git
curl -sS -u ":$ADO_PAT" \
  "https://dev.azure.com/<ORG>/_apis/git/repositories?api-version=7.1" \
 | python3 -c "import json,sys; [print(r['name'], 'size', r['size']) for r in json.load(sys.stdin)['value']]"
```

Note whether the target repo is empty, and whether your local branch is ahead of
its current remote. Commit or stash anything dirty first, and decide whether the
old remote should stay as a mirror or be dropped.

### 7.2 Push without persisting the token

Add the remote with a **clean** URL, then authenticate for the single push using
a header. Embedding `https://:<PAT>@dev.azure.com/...` in the remote writes your
token into `.git/config` in plain text, where it will outlive the task.

```bash
AUTH=$(printf ':%s' "$ADO_PAT" | base64 | tr -d '\n')
git remote add azure https://dev.azure.com/<ORG>/<PROJECT>/_git/<REPO>
git -c http.extraHeader="Authorization: Basic $AUTH" push azure --all
git -c http.extraHeader="Authorization: Basic $AUTH" push azure --tags
```

### 7.3 Verify the push landed

```bash
curl -sS -u ":$ADO_PAT" \
  "https://dev.azure.com/<ORG>/<PROJ_ID>/_apis/git/repositories/<REPO_ID>/refs?api-version=7.1" \
 | python3 -c "import json,sys; [print(' ',r['name'],r['objectId'][:8]) for r in json.load(sys.stdin)['value']]"
```

Confirm the branch tips match your local hashes. Ignore the repository `size`
field, it is a lagging cached statistic and can still read 0 immediately after a
successful push.

### 7.4 Rewire the remotes, if you are fully migrating

```bash
git remote rename origin github          # demote, or `git remote remove origin` to drop
git remote rename azure origin
git -c http.extraHeader="Authorization: Basic $AUTH" fetch origin
git branch -u origin/master master
git remote set-head origin master
git config --get-regexp 'remote\..*\.url'   # confirm no secret was persisted
```

If you keep the old remote, remember it is now behind, and say so out loud to
anyone else who pushes to it.

---

## Phase 8: if someone asks for a work item type the process lacks

Most often: "why are these not User Stories?" on a Basic project.

The type genuinely does not exist in Basic. Getting it requires changing the
project process, which is a migration with manual follow up, not a rename:

1. An account with **Project Collection Administrator** rights goes to
   Organization Settings, Process, the current process, Projects tab, More
   actions, Change process, target process, Save. This is a portal action.
2. Microsoft's documentation is explicit that existing work items are **not**
   converted. You must "update existing work items using the work item types set
   by the target process" yourself.
3. Watch the type collision. Basic `Issue` and Agile `Issue` share a name but
   not a role: in Agile, `Issue` is an impediment and is **not** on the backlog.
   Items left as `Issue` after a Basic to Agile change fall out of the hierarchy.
4. Every state must be remapped, for example Done to Closed and Doing to Active.
5. Re verify the parent links afterwards with Phase 6.

Before committing to this, confirm how the target process renders your intended
hierarchy. Agile expects Epic, then Feature, then User Story, so a User Story
parented straight to an Epic with no Feature between them is worth checking on
the actual backlog view rather than assuming.

Weigh it honestly. If the middle tier items already carry `US-N.N` titles and
occupy the requirement tier, the difference is the label rather than the
structure.

---

## Phase 9: keep it true

Going forward, every change gets one work item, created in the initial state,
moved through states as it progresses, and closed with its commit hash written
onto the item. Update `docs/backlog.md` in the same commit. A backlog that is
accurate for one day is a report, a backlog that stays accurate is an audit
trail.

---

## Gotcha reference

| Symptom | Cause | Fix |
| --- | --- | --- |
| MCP call hangs about 30 min then aborts | Personal MSA against interactive Entra auth | `--authentication pat` |
| "You can't sign in here with a personal account" | Same | Same |
| `State ... is not in the list of supported values` on create | Cannot create in a non-initial state | Create in initial state, then update |
| `KeyError` or empty credential in a script | `VAR=value` on its own line is not exported | `export VAR=...`, or same line as the command |
| Token sent as `<abc>` or `[abc]` | Placeholder delimiters kept when pasting | Paste the raw value only |
| `The following Ids are not valid: .` | macOS `seq -s,` adds a trailing comma | Build the list in Python |
| Process looks like Scrum but types are Epic/Issue/Task | Project property string is vestigial | Trust the type list, not the property |
| Acceptance criteria will not save | Field absent in Basic | Fold acceptance into the description |
| Batch response unreadable | Response exceeded the display limit | Smaller batches, and verify separately in Phase 6 |
| Shell bulk writes to the board are refused | Permission classifier blocks bulk external writes via shell | Use the MCP work item tools |
| Repo `size` still 0 after a push | Lagging cached statistic | Check refs and commit count instead |

## Security checklist

- [ ] PAT scoped to one organization, custom scopes, never Full access
- [ ] Shortest workable expiry
- [ ] Token never pasted into a chat, ticket, or commit message
- [ ] Token never written into `.git/config`
- [ ] Any helper file holding the token is `chmod 600` and deleted afterwards
- [ ] Token revoked at `https://dev.azure.com/<ORG>/_usersSettings/tokens` when finished
- [ ] If the token was ever exposed, verify its **real** scope before assuming impact. A token minted more broadly than intended can push code even if you believed it was work items only. Test rather than assume.
