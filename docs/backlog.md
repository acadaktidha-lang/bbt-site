# BigBinaryTech Website Redesign: Delivery Backlog

Source of truth for the Azure DevOps board in the **bigbinarytech.com** project
(organization: **bigbinarytech**). This file reconstructs the full delivery
history of this repository as Epics, User Stories, and Tasks so the work is
documented, logged, and traceable from the start.

## Conventions

- **Work item hierarchy:** Epic contains User Stories. A User Story contains
  Tasks only where a task adds real detail beyond the story. The Release 1 items
  were originally created as Issues under the Basic process and were migrated to
  User Story on 9 August 2026 (see the process note).
- **Traceability:** every item cites the exact commit hashes it maps to, so the
  board and the git history stay in lock-step. This is the audit trail.
- **Assignment:** every item is assigned to **You** and, once closed, its
  Completed By (Closed By) is **You**. "You" resolves to the signed-in Azure
  DevOps account when the items are created.
- **State:** the Agile process has four states, **New**, **Active**, **Resolved**
  and **Closed**, plus a hidden **Removed**. Finished history is **Closed**. Work
  in progress is **Active** or **Resolved**. Future work is **New**. The old
  To Do, Doing and Done triad belonged to the Basic process and no longer exists
  anywhere on this project, including on the board columns.
- **Writing standard:** no em dashes and no en dashes anywhere, in titles,
  descriptions, comments, or acceptance criteria. Plain sentences only. This
  standard is already part of the project (see US-3.4).
- **Process note, superseded on 9 August 2026:** when items 1 to 32 were created
  the bigbinarytech.com project ran the **Basic** process, whose types are Epic,
  Issue and Task, so the middle tier was created as **Issue** in states To Do,
  Doing and Done. The project has since been switched to **Agile**. Its backlog
  levels are now Epics, Features, Stories and Tasks, the middle tier type is
  **User Story**, and the states are New, Active, Resolved and Closed. Agile does
  have an Acceptance Criteria field, so Release 2 items carry their acceptance
  line there rather than inside the description. **Items 1 to 32 were migrated on
  9 August 2026**: the 24 Issues became User Stories and every item moved from
  the stale state string "Done" to "Closed". Changing System.WorkItemType over
  the REST API preserves the parent link, the description and the history. The
  project property string still reads "Scrum", which is wrong in both processes
  and should be ignored. The one constant is that Azure DevOps refuses to create
  a work item directly in a closed state, so every item is created in the initial
  state and then transitioned. Two further API notes: System.Parent is returned
  as a field but is **not settable** as one, so a parent must be added as a
  System.LinkTypes.Hierarchy-Reverse relation, and the wit $batch endpoint is not
  available on this project, so bulk edits have to be issued one request each.
- **Board configuration repair, 9 August 2026:** switching the process migrated
  the work item types but left the team's Kanban boards behind. The Stories board
  still carried the Basic columns To Do, Doing and Done with **no state mapping
  at all**, and the Epics board mapped its columns to the three dead Basic
  states, so both boards refused to render and showed only "The column
  configurations are not valid. The board cannot be displayed." Only the Features
  board was correct, because Azure DevOps generated it after the switch. All
  three boards now use New, Active, Resolved and Closed, matching the process,
  and each reports isValid. The team also had the Epics backlog level switched
  off, which hid all 16 Epics from the UI, so that was turned back on. This is
  team configuration rather than work item data: it lives in Azure DevOps only
  and is recorded here for the audit trail, not stored in this repository.

## Total scope

Release 1: 8 Epics and 24 User Stories, mapping the first 49 commits.
Release 2: 8 Epics and 20 User Stories, mapping the remaining 24 commits.
60 items in total, all Closed.

Together these cover every non-merge commit on master. The three merge commits,
34e15b2, 89c5cae and e33972a, carry no changes of their own and get no item.

## Board status: created

All 60 items exist on the bigbinarytech.com board, assigned to Ghazanfar Sheikh,
every one Closed with Closed By set. Verified 9 August 2026: 16 Epics, 44 User
Stories, no orphaned story, nothing left in a stale state.

Release 2 work item ids:

| Item | Id | Item | Id | Item | Id |
| --- | --- | --- | --- | --- | --- |
| EPIC-9 | 78 | EPIC-13 | 82 | US-13.1 | 102 |
| EPIC-10 | 79 | EPIC-14 | 83 | US-13.2 | 103 |
| EPIC-11 | 80 | EPIC-15 | 84 | US-13.3 | 104 |
| EPIC-12 | 81 | EPIC-16 | 85 | US-14.1 | 105 |
| US-9.1 | 86 | US-10.3 | 97 | US-14.2 | 106 |
| US-9.2 | 87 | US-10.4 | 98 | US-15.1 | 113 |
| US-9.3 | 88 | US-11.1 | 99 | US-15.2 | 115 |
| US-10.1 | 95 | US-11.2 | 100 | US-15.3 | 117 |
| US-10.2 | 96 | US-12.1 | 101 | US-15.4 | 119 |
| US-16.1 | 121 | | | | |

Every Release 2 User Story carries its commits as real Azure DevOps commit
links, not just hashes in prose, so each item opens straight into its diffs.
Acceptance lines live in the Acceptance Criteria field, and each description
follows As, I want, So that, then What, How and Why.

All 32 items exist on the bigbinarytech.com board, assigned to Ghazanfar Sheikh,
with Closed By set on every Done item.

- Work items 1 to 8 are EPIC-1 through EPIC-8, in that order. EPIC-1 reuses the
  pre-existing work item 1, which was retitled from "BigBinaryTech Website
  Layout".
- Work items 9 to 32 are the 24 User Stories, in seed file order from US-1.1 to
  US-8.2, each parented to its Epic. They were Issues until the 9 August 2026
  migration.
- Every item is Done. US-7.4 (work item 30) was the last one open, and closed
  when its work landed as commit 83aebc1.

The code now lives in Azure DevOps Repos as well. The repository
`bigbinarytech.com` in this same project holds all 54 commits on master plus the
feat/site-review-fixes and fix/hero-headline-crossfade branches, and is the
`origin` remote. The former GitHub remote is kept as `github`.

---

## EPIC-1: Website redesign build (foundation)

Stand up the initial multi-page website and its baseline content.

### US-1.1: Establish the initial site and page set

- Type: User Story
- State: Closed (Done)
- Commits: 416d9b5, 1203fbb, cc1a15c, ec63e6a
- What: created the first version of the site and its page structure, including
  the solution pages.
- How: added the initial HTML page set and asset folders to the repository.
- Why: to give the redesign a working baseline to iterate on.
- Acceptance: the site renders locally with its core pages in place.

### US-1.2: Populate core page content

- Type: User Story
- State: Closed (Done)
- Commits: 76213f9, a4ea331, 1dd59db
- What: filled in the page copy and updated the About page content.
- How: edited the page HTML to carry real content instead of placeholders.
- Why: to move the pages from scaffolding toward review-ready content.
- Acceptance: core pages carry their intended copy.

---

## EPIC-2: Responsive design and mobile experience

Make the pages work on phones and tablets and add mobile navigation.

### US-2.1: Add responsive layouts and mobile menu to About, Blogs, Industries

- Type: User Story
- State: Closed (Done)
- Commits: 12e16eb (merged via PR #8, e4cb449), ef6ffaa
- What: added responsive breakpoints and a working mobile menu to the About,
  Blogs, and Industries pages.
- How: introduced responsive CSS and a mobile navigation pattern on those pages.
- Why: the pages were not usable on small screens without it.
- Acceptance: the three pages reflow cleanly and the mobile menu opens and
  closes on a phone viewport.

### US-2.2: Integrate responsive-page contributions and reconcile revert churn

- Type: User Story
- State: Closed (Done)
- Commits: PR #1 (2f302df), revert PR #2 (ea8fc43, c1e5264), PR #3 (080b230),
  revert PR #4 (8558622, fabdc35), PR #7 (ef6ffaa, cf00db2), uploads 0538795
- What: integrated several responsive-page contributions, some of which were
  reverted and then re-applied cleanly.
- How: merged the contribution branches, reverted the uploads that regressed
  the layout, and kept the versions that matched the specs.
- Why: to land the responsive work without carrying the broken uploads forward.
- Acceptance: the responsive pages that survived reconciliation are the ones on
  master, with no orphaned upload commits active.

---

## EPIC-3: Content and layout-spec reconciliation

Bring every page in line with the layout and content specifications.

### US-3.1: Reconcile all pages to layout specs and add three new pages

- Type: User Story
- State: Closed (Done)
- Commits: 60b059f
- What: reconciled all existing pages to their layout specs, added responsive
  behavior, and added three new pages.
- How: reworked the page markup and styles to match the spec layouts.
- Why: to close the gap between the built pages and the agreed designs.
- Acceptance: pages match their layout specs and the three new pages exist.

### US-3.2: Reconcile solution and service copy to content specs

- Type: User Story
- State: Closed (Done)
- Commits: 5d8edba, 79bf2e4
- What: aligned the copy on the solution and service pages, and across seven
  pages, with the content specs.
- How: replaced drifted copy with the spec-approved wording.
- Why: the live copy had drifted from the approved content.
- Acceptance: the reconciled pages carry spec-approved copy.

### US-3.3: Preserve reconciled About, Blogs, Industries through merge

- Type: User Story
- State: Closed (Done)
- Commits: c997144 (merged via PR #13, bb100be)
- What: kept the reconciled About, Blogs, and Industries pages when merging
  origin/master.
- How: resolved the merge in favor of the reconciled versions.
- Why: to avoid regressing the reconciled pages during the merge.
- Acceptance: the merged tree carries the reconciled versions of those pages.

### US-3.4: Professionalize typography and page metadata

- Type: User Story
- State: Closed (Done)
- Commits: 5550580, 4c5807e
- What: rewrote em and en dashes into plain sentences across all pages, added a
  B-monogram favicon, and cleaned up the page titles.
- How: edited the copy to remove em and en dashes and updated the head metadata.
- Why: to make the site read professionally and to set the no-dashes standard
  the project follows.
- Acceptance: no em or en dashes remain in page copy, the favicon shows, and
  titles are clean.
- Note: this story is the origin of the no-dashes rule applied to this backlog.

---

## EPIC-4: Service pages build-out

Build the individual service pages to the sections their specs call for.

### US-4.1: Add the first four built service pages and fix footer full-bleed

- Type: User Story
- State: Closed (Done)
- Commits: a6c0fbc
- What: added four built-out service pages and fixed the footer so it spans the
  full width.
- How: authored the service page markup and corrected the footer container.
- Why: to expand the service coverage and fix the boxed footer.
- Acceptance: the four service pages render and the footer is full-bleed.

### US-4.2: Add the POS Systems service page and link it from the menu

- Type: User Story
- State: Closed (Done)
- Commits: 39c9c62
- What: added the POS Systems service page and linked it from the Services menu.
- How: authored the page and added its menu entry.
- Why: to complete the service menu coverage.
- Acceptance: the POS Systems page exists and is reachable from the menu.

### US-4.3: Build the AI Automation service sections

- Type: User Story
- State: Closed (Done)
- Commits: f36483f
- What: built the six service sections the AI Automation spec calls for.
- How: authored the six sections to match the content spec.
- Why: the page was missing the sections its spec required.
- Acceptance: all six AI Automation sections are present and match the spec.

### US-4.4: Build the Web Development service sections

- Type: User Story
- State: Closed (Done)
- Commits: 16a2c1c
- What: built the seven service sections the Web Development spec calls for.
- How: authored the seven sections to match the content spec.
- Why: the page was missing the sections its spec required.
- Acceptance: all seven Web Development sections are present and match the spec.

### US-4.5: Restore the Social Media Marketing copy from spec

- Type: User Story
- State: Closed (Done)
- Commits: 7751e3b
- What: restored the Social Media Marketing page copy from its content spec.
- How: replaced the drifted copy with the spec-approved wording.
- Why: the page copy had drifted from the approved content.
- Acceptance: the page carries the spec-approved copy.

---

## EPIC-5: Bug-fix hardening (site review)

Fix the defects surfaced by the site review across navigation, layout, and
interactive components.

### US-5.1: Fix mobile navigation defects

- Type: User Story
- State: Closed (Done)
- Commits: 40a1b45, b67b070
- What: fixed the mobile nav scroll-lock, dead mobile-nav links caused by an
  overlay painting over the drawer, dead CSS variables, and a blank page when a
  CDN failed to load.
- How: corrected the scroll-lock handling, layered the drawer above the overlay,
  removed the dead variables, and added a fallback path for CDN failure.
- Why: the mobile navigation was partly unusable and could blank the page.
- Acceptance: the mobile menu links work, scrolling locks correctly, and a CDN
  failure no longer blanks the page.

### US-5.2: Fix layout clipping and off-screen elements

- Type: User Story
- State: Closed (Done)
- Commits: 1da6010, 790447e, 3aef15d, 4633c43
- What: let the home hero heading box grow instead of clipping, stopped the
  process heading flying off-screen, stopped the process section clipping its
  last card row on short laptops, and removed a stray closing div that
  unbalanced the Industries markup.
- How: adjusted the affected layout rules and fixed the broken markup.
- Why: content was being clipped or pushed off-screen and the markup was
  unbalanced.
- Acceptance: the hero heading, process heading, and process cards display fully
  and the Industries markup is balanced.

### US-5.3: Fix interactive components

- Type: User Story
- State: Closed (Done)
- Commits: 929b1a8, afdcb04, 23a230b, ca52add, 5a92c3f
- What: centered the active trust-slider card instead of assuming a fixed 300px
  width, removed the hero headline flicker with a valley-free one-at-a-time
  crossfade, stopped the Enterprise Odoo section orphaning its fourth module
  card, and fixed the scroll-progress bar while removing stale root duplicates.
- How: reworked the slider centering math, the headline rotation, the Odoo card
  grid, and the scroll-progress calculation.
- Why: these components behaved incorrectly and looked unpolished.
- Acceptance: the slider centers correctly, the headline rotates without
  flicker, the Odoo cards align, and the scroll-progress bar reports correctly.

### US-5.4: Apply Home, About, Contact review and contact-spec fixes

- Type: User Story
- State: Closed (Done)
- Commits: fb2ab04
- What: applied the review-report fixes and the contact-spec fixes to the Home,
  About, and Contact pages.
- How: edited the three pages per the review report and the contact spec.
- Why: to clear the outstanding review items on those pages.
- Acceptance: the review-report and contact-spec items for those pages are
  resolved.

---

## EPIC-6: Performance optimization

Reduce load cost and runtime jank on the homepage.

### US-6.1: Remove the homepage relayout loop and eager background video

- Type: User Story
- State: Closed (Done)
- Commits: 3d04854
- What: stopped a homepage relayout loop and removed 8 MB of eager background
  video that loaded on page open.
- How: broke the layout-thrash loop and stopped auto-loading the heavy video.
- Why: the loop and the eager video hurt load time and caused jank.
- Acceptance: the homepage no longer runs the relayout loop and does not eagerly
  load the 8 MB video.

---

## EPIC-7: WordPress deployment readiness

Prepare the pages to run inside WordPress and Elementor and finalize the build.

### US-7.1: Prepare WP-ready assets and clean the Resources mega-menu

- Type: User Story
- State: Closed (Done)
- Commits: f59e9ce
- What: prepared WordPress-ready assets and cleaned up the Resources mega-menu
  across the site.
- How: adjusted the assets and menu markup for the WordPress context.
- Why: to make the assets and menu behave correctly once deployed to WordPress.
- Acceptance: the assets are WordPress-ready and the Resources mega-menu is
  clean sitewide.

### US-7.2: Cancel the Elementor boxed container for full-bleed layout

- Type: User Story
- State: Closed (Done)
- Commits: 43ab4ff
- What: added a fix that cancels the Elementor boxed 1140px container.
- How: added a full-bleed override so the layout is not constrained by the
  theme container.
- Why: the theme container boxed the layout that was designed to be full-bleed.
- Acceptance: the layout spans full width inside Elementor.

### US-7.3: Nav cleanup and real footer contact data across all pages

- Type: User Story
- State: Closed (Done)
- Commits: 8d82e96
- What: cleaned up the navigation and put real footer contact data on all pages.
- How: edited the shared nav and footer markup across the pages.
- Why: the footer carried placeholder contact data and the nav needed cleanup.
- Acceptance: every page shows the real footer contact data and a clean nav.

### US-7.4: Finalize the WordPress document-container builds and logo

- Type: Issue
- State: Done
- Commits: 83aebc1
- What: finalized the WordPress document-container (.dc) builds, swapped the
  light logo, and produced the build/ and build14/ output.
- How: edited the .dc.html builds, replaced the light logo with the higher
  resolution asset, and regenerated the build output.
- Why: to produce the final WordPress-ready bundle for deployment.
- Acceptance: the .dc builds and logo are final and the build output is
  generated. Met.
- Note: the same commit swapped the services list across the nav and footer to
  the client's final six, so POS Systems is out and Social Media Marketing is
  in. Build output is build/ with 7 pages and build14/ with 13 pages.
- Since superseded: build14/ was merged into build/ and the six files carrying
  attachment collision suffixes were renamed to match their live slugs. There is
  now one build file per published page, listed in
  [page-manifest.md](page-manifest.md).

---

## EPIC-8: Repository structure and handoff

Document the frontend handoff and reduce the repo to the redesign bundle.

### US-8.1: Add the frontend brand handoff documentation

- Type: User Story
- State: Closed (Done)
- Commits: see BBT-FRONTEND-BRAND-HANDOFF.md
- What: added the frontend brand handoff document.
- How: authored the handoff markdown describing the brand and frontend.
- Why: to give the client and any future developer a clear handoff reference.
- Acceptance: the handoff document exists and describes the brand and frontend.

### US-8.2: Clean the repo down to the redesign bundle

- Type: User Story
- State: Closed (Done)
- Commits: e662ccc, 5a92c3f, 8dc64ea
- What: removed the old project tree and stale root duplicates, ignored the
  root-level audit screenshots, and reduced the repo to the redesign bundle.
- How: deleted the superseded files, updated .gitignore, and kept only the
  redesign bundle.
- Why: to leave a clean repository focused on the redesign.
- Acceptance: the repo contains only the redesign bundle and ignores the audit
  screenshots.

---

---

## Release 2: work delivered from 16 July to 7 August 2026

Everything below covers the commits that landed after the original board was
seeded. EPIC-1 to EPIC-8 above map the first 49 commits. These eight Epics and
twenty User Stories map the remaining 24, so every non-merge commit on master is
now accounted for on the board.

One commit from the earlier era, a4d7b2d, was missed when the first backlog was
written. It is picked up here as US-10.1 rather than being back-filled into
EPIC-3, so the numbering above stays stable.

## EPIC-9: Board and delivery traceability

Reconstruct the repository history as a board, and make the procedure reusable.

### US-9.1: Reconstruct the repository history as a delivery backlog

- Type: User Story
- State: Closed (Done)
- Commits: b7412e2, 46fa1a2
- What: mapped every commit in the repository to an Epic and a User Story, then
  recorded US-7.4 as done and the move of the code into Azure DevOps Repos.
- How: authored docs/backlog.md citing the exact commit hashes behind each item,
  and confirmed the project's process details rather than assuming them.
- Why: so the board and the git history stay in lock-step and the delivery has
  an audit trail rather than an undocumented pile of commits.
- Acceptance: every commit is cited by exactly one item, and the board matches
  the document.

### US-9.2: Generalize the board reconstruction into a reusable runbook

- Type: User Story
- State: Closed (Done)
- Commits: 07beef5
- What: turned the procedure used here into a process agnostic runbook covering
  discovery, seeding, creation, verification, and moving code into Azure Repos.
- How: authored docs/ado-backlog-runbook.md, including the failure modes that
  cost real time on this project: personal Microsoft accounts hanging interactive
  auth, the refusal to create a work item directly in a closed state, and a
  project property string that reports the wrong process.
- Why: so the next repository can be reconstructed as a board without paying the
  same discovery cost again.
- Acceptance: the runbook works against Basic, Agile, Scrum and CMMI without
  assuming a type or state name.

### US-9.3: Retire the superseded frontend brand handoff document

- Type: User Story
- State: Closed (Done)
- Commits: 35b7243
- What: removed BBT-FRONTEND-BRAND-HANDOFF.md, the 313 line handoff added under
  US-8.1.
- How: deleted the file once the redesign bundle and the layout specs carried the
  same information in a form that was being kept current.
- Why: a handoff document that is no longer maintained misleads the next reader
  more than having no document at all.
- Acceptance: the file is gone and nothing in the repository references it.

---

## EPIC-10: Homepage credibility and trust signals

Make every claim on the home page traceable to something that actually exists.

### US-10.1: Separate awards from regulatory registrations in the trust strip

- Type: User Story
- State: Closed (Done)
- Commits: a4d7b2d
- What: split one mixed trust row into two labelled rows, "Certifications, Awards
  and Tech Partnerships" and "Registered and Compliant With", so a buyer can tell
  an award from proof the company is legally registered.
- How: rebuilt the strip as a semantic list with every logo capped to the same
  48px bounding box, replaced the 300px testimonial style cards the spec did not
  ask for, made the marquee per track so desktop never renders duplicate logos,
  and added Organization JSON-LD carrying memberOf, hasCredential and award.
- Why: the previous single row let PSEB read as an award, so the site never
  actually stated its registration status.
- Acceptance: two labelled rows render, one centred line each on desktop, each
  marqueeing independently below 768px, with descriptive alt text on every logo.
- Note: superseded in part by US-16.1, which removed the registrations row when
  the company repositioned to the United States.

### US-10.2: Replace unverifiable Selected Work claims with evidenced products

- Type: User Story
- State: Closed (Done)
- Commits: 04e3bbc, 06e95a8
- What: removed three case studies that no repository in the organisation
  supports, a 40 outlet retail ERP, a UK logistics platform credited with cutting
  dispatch work by 60 percent, and a US SaaS billing portal, and replaced them
  with products this team designed and maintains.
- How: catalogued six case studies traceable to real repositories in
  docs/portfolio-case-studies.md with clients anonymised, then swapped the home
  page cards to LearnOS, BBT POS and CampusPlus. The hero console kept its layout
  but is now labelled an illustration in both its caption and its aria-label, and
  the invented "PKR 24.6M, up 31%" figure became a neutral axis label.
- Why: publishing outcomes nobody measured is a liability in front of a prospect
  who asks for a reference.
- Acceptance: every card describes what a product does rather than quoting an
  unmeasured outcome, and the layout renders exactly as before.

### US-10.3: Lead Selected Work with the AI and data platforms the site sells

- Type: User Story
- State: Closed (Done)
- Commits: 62eb996
- What: replaced the three cards again, this time with DataNexus, Carrier Revenue
  Operations and FlowStack, the DataSap Inc platforms that match the site's AI and
  automation positioning.
- How: inlined the product artwork from datasapinc.com as SVG rather than
  uploading it, since WordPress rejects SVG uploads by default, set each to fill
  its tile by cropping evenly, and added a scrim to FlowStack's light themed
  mockup so it sits in the dark card row. Attribution under the grid states the
  work was delivered with sister company DataSap Inc.
- Why: LearnOS, BBT POS and CampusPlus are real, but they are not what this site
  sells, so they undersold the AI and data capability the visitor came for.
- Acceptance: verified at 1440px and 390px with no horizontal overflow and all
  three mockups rendering.

### US-10.4: Catalogue every published statistic for verification

- Type: User Story
- State: Closed (Done)
- Commits: cb2b233
- What: listed every numeric claim across seventeen pages so each can be
  confirmed, corrected or removed individually.
- How: swept the pages into docs/stats-verification.md, flagging the claims a
  prospect is most likely to check: the 4.8 out of 5 rating, 2,500 plus newsletter
  subscribers, 25k monthly readers, 12M plus global users, and the before and
  after outcome table on the case studies page.
- Why: the audit surfaced that the site publishes three different project totals,
  200 plus, 60 plus and 50 plus, and two different satisfaction figures, 98
  percent and 92 percent. A visitor comparing two service pages sees the
  contradiction, which costs more credibility than a smaller accurate number.
- Acceptance: every numeric claim on the site appears in the checklist with a
  status.

---

## EPIC-11: Lead capture and contact reliability

Stop losing enquiries, then make the fix durable.

### US-11.1: Stop the enquiry form discarding every submission

- Type: User Story
- State: Closed (Done)
- Commits: 37640ea
- What: the contact form shipped with action="#" and no server side handler, so
  submitting it reloaded the page and threw the enquiry away with no error shown.
  Every lead sent through the contact page since launch was lost.
- How: as an immediate stopgap the form now composes the completed enquiry and
  hands it to the visitor's own mail client, addressed to
  `business@bigbinarytech.com`, and shows that address on screen so a visitor with
  no configured client can still reach us. Name and business email are validated
  before submit, and the status line is aria-live so screen readers announce the
  result.
- Why: no form plugin was installed, so there was nothing to post to, and leaving
  the form silently broken for another release was not acceptable.
- Acceptance: no submission path ends in silence. Superseded by US-11.2.

### US-11.2: Deliver enquiries through Contact Form 7 and record them

- Type: User Story
- State: Closed (Done)
- Commits: 451f684
- What: replaced the mailto stopgap with a real submission path that no longer
  depends on the visitor having a mail client.
- How: installed Contact Form 7 and Flamingo and pointed the form at CF7's public
  REST endpoint for form 704. CF7's REST write route would not persist a custom
  form definition on this install, so the six extra fields the page collects,
  phone, company, region, service and project details, are folded into the message
  body with the selected service carried in the subject line. The mail client path
  is kept as a fallback whenever the response is anything other than mail_sent.
- Why: enquiries need to be delivered and stored, not handed to a client that may
  not exist.
- Acceptance: verified end to end against the live page. Submission returns
  mail_sent, the success message renders, and the form resets.
- Open follow up: CF7 form 704 still mails to the WordPress administrator address
  rather than `business@bigbinarytech.com`. That recipient must be changed in
  wp-admin, because the REST route will not save it. Flamingo stores every
  submission either way, so nothing is being lost.

---

## EPIC-12: Legal pages and compliance

Publish the policies the footer already promised.

### US-12.1: Publish Privacy Policy and Terms of Service

- Type: User Story
- State: Closed (Done)
- Commits: 9a6c97c
- What: both legal links in the footer were href="#" on every page, and neither
  page existed. WordPress still held its default Privacy Policy stub in draft,
  full of "Suggested text:" boilerplate, so /privacy-policy/ returned 404 and
  /terms-of-service/ did not exist at all.
- How: wrote both pages against how the site actually behaves rather than from a
  template. The enquiry form composes mail client to client and stores nothing,
  Google Analytics loads through Site Kit, and WordPress and the host set the
  remaining cookies. The terms separate website use from client engagements, which
  are governed by the agreement signed for that work. Both are built on the shell
  used by the rest of the site, and the footer links across all 24 build files now
  point at them.
- Why: publishing a privacy link that 404s is worse than publishing none, and the
  site was collecting enquiry data with no stated policy.
- Acceptance: verified at 1440px and 390px with no horizontal overflow, and both
  footer links resolve on every page.

---

## EPIC-13: Quality assurance sweep and sign-off

Sweep every public page, fix what the sweep finds, and sign the release off.

### US-13.1: Fix the defects found sweeping all 22 pages

- Type: User Story
- State: Closed (Done)
- Commits: b53c982
- What: fixed three blocking defects and eight layout and content defects found
  by a full sweep of HTTP status, internal link integrity, placeholder copy,
  heading structure, image alt text, contact detail consistency, and layout at
  1440, 1024, 768 and 390 pixels.
- How: the blockers were /about-us/ rendering on the default theme template
  instead of elementor_canvas, so it showed two navigation bars, two footers, two
  H1s and a stale contact block carrying details that are not current;
  /e-books-guides/ publishing a designer's brief as customer facing copy under a
  heading promising a preview; and /pos-systems/ live saying "This page is being
  prepared", now a draft with a 301 to /retail-pos/. The layout defects included
  the "MOST POPULAR" badge sitting on the wrong tier on five pages, Enterprise
  Odoo's module labels each one card out of step, prices reading backwards, and a
  swipe hint showing at desktop widths where nothing scrolls. Footer social icons
  were href="#" on all 22 pages and now point at the profiles that exist, with the
  X icon replaced by Instagram since no X account exists.
- Why: these are the defects a visitor hits first, and several of them published
  contact details that would send a prospect to the wrong place.
- Acceptance: one email address and one phone number sitewide, no dead links, no
  placeholder copy, no horizontal overflow at any of the four widths, no broken
  images, and no console errors.

### US-13.2: Move every page to a clean URL and remove the visual placeholders

- Type: User Story
- State: Closed (Done)
- Commits: ad9847e
- What: six pages were published on suffixed URLs, /about-us-2/, /solutions-3/,
  /resources-2/, /retail-pos-2/, /digital-transformation-3/ and /ai-automation-2/.
  All six now sit on their clean slugs, and the two remaining visual placeholders
  are gone.
- How: the cause was not the old design drafts. Media attachments held the clean
  slugs, because WordPress puts attachments and pages in the same URL namespace,
  so an image named about-us took /about-us/ and the page fell back. Renaming the
  conflicting attachments freed the slugs. Slug changes made over the REST API do
  not record an old slug, so WordPress created no redirect and the previous URLs
  began returning 404. Installed Redirection and added a 301 from each old URL,
  then rewrote 331 internal links across 22 build files and pushed the same
  rewrite to all 22 live pages so navigation does not depend on the redirects. The
  Selected Work cards, which had shipped with a diagonal stripe fill reading
  "[ real project screenshot ]", each got a small CSS illustration of their
  product, and the testimonial cards' empty grey avatar circles now carry a
  quotation mark, since the quotes are deliberately unattributed and a photograph
  would contradict them.
- Why: suffixed URLs read as a mistake to anyone who looks at the address bar, and
  the placeholders were visible to every visitor.
- Acceptance: all six old URLs verified returning 301 to the right target, card
  dimensions, grid and spacing unchanged.

### US-13.3: Issue the QA sign-off report and record the open metadata tasks

- Type: User Story
- State: Closed (Done)
- Commits: 81b85a4
- What: produced the client facing sign-off report covering all 22 public pages,
  three blocking defects, eight layout and content defects and four items resolved
  earlier in the release, with the coverage matrix and the sign-off checklist.
- How: wrote docs/qa-report-2026-07-28.html for the client rather than for
  engineers, so each finding says what a visitor would have seen and what was done
  about it, and put the paste ready values for the findings that cannot be fixed
  from site code into docs/seo-metadata-todo.md.
- Why: Yoast keeps page titles and meta descriptions in per post options, and its
  REST route accepts a write, returns 200, then discards it, so nine shortened
  titles and four descriptions have to be entered by hand.
- Acceptance: every replacement value is inside Google's length limits, checked on
  write.

---

## EPIC-14: Navigation and service catalogue

Make every published page reachable from the menu, and publish the AI services.

### US-14.1: Add Resources to the navigation and restore Pricing and Careers

- Type: User Story
- State: Closed (Done)
- Commits: ddc6853, ff6f18e
- What: the Resources section had no entry in the navigation, so its three
  subpages were only reachable by typing the address. Added a Resources dropdown
  between Industries and About, and put Pricing and Careers back in the footer
  under Company.
- How: followed the Services and Solutions pattern already in use, a link to the
  hub page plus the subpages, with the mobile menu carrying a single flat link to
  the hub as those two do. A follow up pass reordered the dropdown to Blogs,
  E-Books and Guides, Case Studies, relabelled "Blog and Insights" to "Blogs" so
  the menu uses the same names as the pages, and dropped the "All Resources" entry
  as a duplicate route to the hub the dropdown header already links to. Applied to
  all 22 pages, since the navigation and footer are inlined into each one.
- Why: a published page with no route into it will not be found.
- Acceptance: verified live. The dropdown opens at 324 by 222 pixels, stays inside
  the viewport, the nav still fits one line at 1440px, and all six menu targets
  return 200.

### US-14.2: Publish the chatbot, RAG, LLM and data warehouse services

- Type: User Story
- State: Closed (Done)
- Commits: 3873b43, c99bfa5
- What: added four AI and data services to the catalogue and closed the remaining
  Social Media Marketing gaps against its content spec.
- How: drafted the copy in docs/ai-data-services-copy.md first, then published it
  across the services hub, ai-automation, custom-software-development, pricing and
  social-media-marketing.
- Why: these are the services the business actually sells, and the site did not
  list them.
- Acceptance: each service appears in the catalogue with its own copy, and
  Social Media Marketing matches its spec.

---

## EPIC-15: Layout-spec architecture and build consolidation

Apply the client's layout specs as structure, and make the source self-sufficient.

### US-15.1: Build the section architecture the 39 layout specs define

- Type: User Story
- State: Closed (Done)
- Commits: d3127cb
- What: applied the section and layout structure from the client's 39 layout and
  content documents across the site, taking eleven pages from 81 sections to 154,
  and added two new pages, pos-systems and faqs.
- How: the specs were largely present as content but not as layout. Every list in
  them, deliverables, platforms, market notes, testimonials and stats, had been
  flattened into stacked full-width paragraphs, most visibly on
  social-media-marketing where one section ran to fourteen of them. Those became
  two-column blocks with a what-you-get panel, a chip row for platforms, and a
  stat grid legible on the dark ground. Palette, typography and voice are
  deliberately unchanged: the specs propose a different colour system and it was
  not adopted. FAQs carries 32 answers across 8 categories with search, category
  filters and role shortcuts. Every class this build introduces is namespaced
  under bb-, because the generic names it started with are the same shape as the
  theme collision that has caught this project before.
- Why: the client's specs describe layout, and delivering their content as
  undifferentiated paragraphs loses most of what the specs were for.
- Acceptance: 24 pages verified at 1440, 900 and 390px with no horizontal
  overflow, 655 internal links and every anchor resolving, one h1 per page, no
  empty hrefs, and alt text on every image.
- Defects fixed in passing: the home hero rotating word hanging 6px below its
  line, which is the alignment issue the client reported; six Explore actions on
  the services hub pointing at /contact-us/ instead of the service pages; seven
  pages with a .g3 grid collapse rule but no .g4; six pages with a skip-to-content
  link whose target had no id; and stat grids writing their column count as an
  inline style, which outranks the responsive media queries and broke seven pages
  at 390px.

### US-15.2: Consolidate the build output to one file per page

- Type: User Story
- State: Closed (Done)
- Commits: a0c84b0
- What: merged the split build/ and build14/ trees into one file per published
  page and recorded the result in a verified manifest.
- How: removed the duplicate build14 files and wrote docs/page-manifest.md
  mapping each build file to its live page.
- Why: two build trees for one site meant every sitewide change had to be applied
  twice, which is how pages drift apart.
- Acceptance: one build file per published page, listed in the manifest.

### US-15.3: Carry the Elementor full-bleed rule in the page source

- Type: User Story
- State: Closed (Done)
- Commits: bf2b437
- What: embedded the rule that cancels Elementor's boxed container into every
  page's own stylesheet.
- How: added the selector to each page rather than relying on the copy held in the
  WordPress install. The selector matches nothing outside WordPress, so it is
  inert locally.
- Why: these pages are pasted as full-bleed HTML into an Elementor HTML widget,
  and Elementor's default boxed container confines them to a centred 1140px box.
  The rule lived only in the WordPress install, so any page redeployed from source
  lost it and came back cramped. That drift had recurred on this site.
- Acceptance: a page redeployed from source renders full-bleed with no manual step.

### US-15.4: Match FAQ search to the category names

- Type: User Story
- State: Closed (Done)
- Commits: 4b0a9e1
- What: searching the name of a category, pricing or compliance or outsourcing,
  returned nothing, because the term appears in no question and no answer, so the
  filter fell through to the empty state on a page that has four pricing questions.
- How: the filter now also matches data-cat.
- Why: a visitor searching the word the page itself uses as a heading should not
  see an empty state.
- Acceptance: searching a category name returns that category's questions.
- Also recorded: the ids WordPress assigned to the two new pages, 715 for
  pos-systems and 716 for faqs, and that the /pos-systems/ redirect is disabled
  rather than deleted, since the Redirection REST API has no single-redirect
  DELETE and silently ignores the enabled flag on update.

---

## EPIC-16: United States market repositioning

Move the company's stated home from Pakistan to the Wyoming head office.

### US-16.1: Move the positioning from Pakistan to the Wyoming head office

- Type: User Story
- State: Closed (Done)
- Commits: 94eee71
- What: removed every mention of Pakistan, Lahore, PSEB, P@SHA, NAVTTC, PSDA,
  LCCI, FBR, Karachi, Islamabad, Punjab, PKR and PKT from all 26 build pages. The
  head office is now 30 N Gould St # 48715, Sheridan, WY 82801, and
  +1 307-443-4365 is the primary number everywhere.
- How: structural changes rather than find and replace. The registrations badge
  block came off Home and About with no US equivalents invented in its place. The
  NAVTTC FAQ came out of both the visible accordion and the FAQPage JSON-LD. The
  two testimonial cards located in Pakistan were removed from About and Contact
  rather than relocated, which would have falsified a reference. Case study and
  client rows dropped city and country and kept the sector. Careers salary bands
  became "Competitive, discussed at offer stage", because the PKR figures were not
  converted and publishing a guessed number in a live job ad is worse than
  publishing none. The +92 line is kept as a secondary contact, relabelled from
  sales to technical support in the schema.org contactPoint with PK dropped from
  areaServed, and schema.org PostalAddress on all 26 pages now carries
  streetAddress, addressRegion WY, postalCode 82801 and addressCountry US.
- Why: the company sells into the United States, and a Pakistan address at the top
  of every page sets a different expectation before a prospect reads a word.
- Acceptance: published to all 26 live pages and verified. HTTP 200, one header
  and one footer per page, valid JSON-LD, Wyoming address present, zero Pakistan
  terms.
- Open follow ups: a Wyoming registration badge should be added once the filing
  number is known, and the Terms of Service change moving governing law to Wyoming
  and venue to Sheridan County needs a lawyer's eye before it is relied on.
- Also picked up: the previously uncommitted 2026-08-05 spec compliance work,
  which was already live but had never been committed, plus the two pages that
  shipped with it, outsourcing (720) and videos (721).

---

## EPIC-17: Page content alignment to the client content specs

Walk every published page against its client content document and reconcile the
copy. Layout, section structure and design are out of scope: this epic changes
words only. All of it lands on one branch, `content/page-content-updates`, and
goes up as a single pull request.

**Standing decision, taken 11 August 2026.** The content specs were written
before the Pakistan to Wyoming repositioning in US-16.1. Where a spec conflicts
with that repositioning, **the repositioning wins** and the conflict is recorded
on the story rather than applied. This keeps Pakistan, PSEB, NAVTTC and
government affiliation language off the site while the rest of each spec is
adopted in full.

### US-17.1: Reconcile the home page to its content document

- Type: User Story
- State: Closed (Done)
- Commits: 09a8adf
- What: the Trusted By section is retitled from "Recognized by industry leaders.
  Registered and compliant in the United States." to the spec's "Recognized by
  Industry Leaders", its eyebrow drops "& REGISTRATIONS", and its body becomes
  the spec line "We've earned the trust of leading technology and education
  bodies. Here's who stands behind us:".
- How: text nodes only. Every style attribute, class and element in the section
  is byte for byte unchanged, so the heading, the paragraph and the badge row
  occupy the same layout as before. Verified after the edit: one h1, 11 h2, 12
  sections, both JSON-LD blocks parse, 440 style attributes, zero Pakistan terms.
- Why: the rest of the page already carried the spec verbatim, applied as
  structure in d3127cb. Hero, the seven step process, the partnership section,
  all six capability cards, all eight industry cards, all four solution cards,
  the four reasons and all eight FAQs matched on a line by line diff. This
  heading was the only copy left that the spec disagreed with on wording alone.
- Why the spec's punctuation is not reproduced: the document separates the two
  halves of the "What We Build" and "More Than a Vendor" headings with an em
  dash. The build renders both with a colon, which is US-3.4 and stays.
- Spec lines deliberately not applied, per the standing decision above: the FAQ
  answer "What makes Big Binary Tech different" credits "PSEB registration" and
  "government affiliations" in the spec, and the page keeps "US registration"
  and "university affiliations". The two registration and compliance FAQs added
  during US-16.1 are kept, though the spec lists eight questions rather than ten.
- Acceptance: every section of the content document is present on the page in the
  spec's order and wording, except the conflicts recorded above.
- Open follow up, not actioned: the badge row still lists FAST, UMT, LIST and
  WAC. FAST and UMT read as Pakistani university affiliations that the US-16.1
  term sweep did not catch, because it searched for place names and regulators
  rather than institutions. Left in place pending confirmation of what each body
  is. The same heading is also live on about-us.html, so that page needs the same
  change when its turn comes or the two will drift.

### US-17.2: Reconcile the About page to its content document

- Type: User Story
- State: Closed (Done)
- Commits: 942fe07
- What: nine text edits bringing about-us.html back to the wording of its
  content document, plus one Pakistan reference the US-16.1 sweep missed.
- How: text nodes only. Every style attribute and every tag on the changed lines
  is identical before and after, verified by extracting them from the patch and
  comparing. Structure after the edit is unchanged: one h1, 9 h2, 10 sections,
  7 details, both JSON-LD blocks parse.
- The drift that was corrected: the two story paragraphs had lost the spec's
  serial commas; core value 02 had dropped the spec's quotation marks around
  "good enough" and read "better on every single project" for "better, every
  single project"; core value 03 had been flattened from the spec's question,
  "Before anything else, we ask: is this actually helping our client?", into a
  statement; Global Reach read "more than 50 countries", "you are" and "custom
  software" where the spec has "50+ countries", "you're" and "custom software
  solutions"; and the testimonials line read "Here is what businesses" for
  "Here's what businesses".
- The missed Pakistan reference: the hero stat strip read "4 / Markets: PK, GCC,
  US, UK". US-16.1 swept for place names, regulators and currency codes, so a
  bare ISO country code sitting inside a stat label passed through it. Now
  "3 / Markets: GCC, US, UK". No other build file contains that string.
- Carried from US-17.1: the Trusted By section is retitled "Recognized by
  Industry Leaders" with the spec's body line. This page's content document does
  not describe that section, but home and About carried the same heading, so
  changing one and not the other would have drifted them.
- Why: the page was structurally complete against its spec. What had gone was
  the exact wording, in small edits that individually look like tidying and
  together move the copy away from what the client signed off.
- Acceptance: all nine spec sections present in the spec's order and wording,
  except the conflicts recorded below.
- Spec lines deliberately not applied, per the EPIC-17 standing decision: FAQ 6
  answers "We're based in Pakistan and serve clients across 50+ countries", and
  the final CTA gives "Or call us at 0326 8880101" as the only number. The page
  keeps Sheridan, Wyoming and leads with +1 307-443-4365, with the +92 line
  retained as the secondary contact set up in US-16.1.
- Open follow up, not actioned: the spec's testimonials section calls for four
  cards and the page carries three, because US-16.1 removed the two located in
  Pakistan rather than relocate them. The spec supplies no copy for any of the
  four, so there is nothing to restore without new client material. The FAST,
  UMT, LIST and WAC badges noted in US-17.1 are on this page too.

### US-17.3: Reconcile the AI Automation page to its SEO content report

- Type: User Story
- State: Closed (Done)
- Commits: 0bd0ceb
- What: nine text edits plus three link changes, bringing ai-automation.html to
  the wording of its content report. The hero and the five step process carried
  none of the spec's copy; everything between them already matched.
- How: text nodes only apart from the three links. All 193 style attributes and
  all 237 tags on the changed lines are identical before and after, verified by
  extracting them from the patch. Structure after the edit is unchanged: one h1,
  17 h2, 18 sections, 16 details.
- Hero: the H1 becomes "AI Automation & Integration Services That Save Hours
  Every Day"; the subheadline becomes the spec's "Stop wasting time on tasks a
  system can handle for you"; the primary CTA becomes "Get a Free Automation
  Audit" and gains offer=audit, which the GA4 hook added in US-15.1 already
  reads; the secondary CTA becomes "See How It Works" and points at #process.
- The #process anchor was already on the how-we-work section and nothing linked
  to it, so the spec's scroll target had been built and left unreachable.
- Process section: the heading becomes "How We Build Your Automation: Start to
  Finish", the five steps become Free Discovery Call, Audit & Roadmap, Build &
  Test, Launch & Train and Ongoing Support with the spec's descriptions, and its
  CTA becomes "Start With a Free Discovery Call".
- Bottom CTA becomes "Ready to Stop Doing It Manually?" with the spec's subline
  and button label, and gains offer=audit.
- Two FAQs are restored to the spec's wording in the visible accordion and the
  FAQPage JSON-LD together, since editing one and not the other is how the two
  drift. Verified in sync afterwards, 15 questions each matching one for one.
- Why: this page sells the service the business leads with, and its hero was the
  one part a visitor reads first and the one part that matched nothing in the
  spec.
- Acceptance: every section of the content report present in its order and
  wording, except the item recorded below.
- Not applied: the spec's hero trust bar reads "50+ automations built, GCC, USA,
  UK, PSEB Registered, WhatsApp API Partner". PSEB is barred by the EPIC-17
  standing decision, and the remainder is a text bar where the build carries a
  four stat grid, so adopting it would change layout rather than copy.
- Note on the meta description: it now carries the spec's text, but that tag is
  not published. Only the body reaches _elementor_data and the live description
  is a Yoast field, so this change is for source accuracy only.
- Open follow ups, not actioned: the spec's bottom CTA asks for a second button,
  "Download Our AI Guide", pointing at /resources/ai-automation-guide/, which
  does not exist and would 404, and the lead magnet PDF behind it has not been
  written. The spec also writes the six problem card headlines in title case
  where the build uses sentence case, which is a typography convention set in
  US-3.4 rather than a copy difference, so it was left alone.

### US-17.4: Reconcile the Blogs page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: f516b2c
- What: ten text edits restoring copy that had been shortened or mislabelled
  against the content report.
- How: text nodes only. All 232 style attributes, 244 tags and 9 hrefs on the
  changed lines are identical before and after, verified from the patch.
  Structure unchanged: one h1, 11 h2, 21 h3, 12 sections.
- The drift corrected: the hero body had been cut to two short sentences, losing
  the Dubai, London and New York line and the claim that the engineers write
  about what they have actually built; the featured post tagline read "Featured
  This Week" for "Editor's Pick This Week", its body had lost two clauses and
  its button read "Read Full Article"; post 4 had lost its subtitle "A Complete
  2026 Guide" and the closing sentence of its summary; the most read eyebrow
  read "MOST READ"; the newsletter line had lost "One email. Zero spam. Real
  insights you can use."; the stats bar abbreviated "Average Rating".
- The testimonials heading was carrying the spec's tagline, "What Our Readers
  Say", as its H2. It now carries the spec's H2, "Real Feedback From Real
  Readers".
- Why: the shortened copy reads as house style rather than error, which is
  exactly why it survived. Restored, the page says what the client wrote.
- Acceptance: every section of the content report present in its order and
  wording, except the items recorded below.
- Not applied, per the EPIC-17 standing decision: the spec titles post 5 "Why
  GCC Businesses Are Moving to Pakistan for Tech Outsourcing" and credits "PSEB
  registration" and "Pakistan-based partners". The page keeps the US-16.1
  wording, "Offshore Tech Outsourcing", "verified vendor credentials" and
  "offshore delivery partners".
- Deferred at the time and delivered in US-17.5 below: the section taglines.
- Not applied. A fourth trust badge, "GCC, USA, UK Coverage", has no badge row
  section of its own to sit in, and the video section's body paragraph has no
  paragraph element. Section 11 asks for an H3 "Keep Exploring" where the page
  has an eyebrow reading KEEP EXPLORING above an H2 "Related Pages", so the
  words are already on screen.
- Not applied, and worth a decision before it is. The spec attributes its three
  testimonials to Rashid Al-Mutairi, Priya Sharma and Abdullah Al-Saud. The page
  carries the same three quotes with role and company only. Naming them means
  publishing three identified people as references, and these three are not
  among the names confirmed as usable in the 2026-08-05 spec audit. See
  docs/portfolio-case-studies.md, which records that placeholder attributed
  quotes are not to be published.

### US-17.5: Add the Blogs section taglines and repair the stats section

- Type: User Story
- State: Closed (Done)
- Commits: 6b56f86
- What: the six section taglines held back from US-17.4, plus a repair to the
  stats section found while placing them.
- How: no new component. Each eyebrow reuses the one already on the page, byte
  for byte, in the variant matching its ground, #9d6507 on light and #f5a623 on
  dark. Verified from the patch: six div elements added, nothing removed, no
  href changed, and exactly two distinct new style strings, which are those two
  existing variants. Heading counts unchanged at one h1, 11 h2 and 21 h3.
- Taglines added above the hero, Latest Posts, the newsletter, the video
  section, the testimonials and the final CTA. The page carried this element on
  only two of its twelve sections beforehand.
- The stats repair: the section heading read "Weekly" and was followed by two
  paragraphs reading "Published" and "Coverage". Those are the labels of the
  spec's fifth and sixth stats, "Weekly, Published" and "GCC, USA, UK,
  Coverage", flattened out of the stat grid and into prose, which is why the
  grid showed four of the spec's six. The heading now reads "Numbers That
  Speak", the name the spec gives the section, and the two paragraphs carry the
  information as sentences. Moving them back into the grid was not done because
  the grid is a fixed four column layout and six cells would reflow it.
- Why: the taglines are the client's own section labels, and the site already
  uses this exact element everywhere else, so the page was inconsistent with
  both the spec and its own siblings.
- Acceptance: verified rendered in order. Every section from the hero to Related
  Pages now carries its tagline above its heading.
- Not added: section 3's "Editor's Pick This Week", already on the page as the
  featured card's own heading, where an eyebrow would duplicate it; and section
  2's "Trusted by Readers Across Three Continents", which sits above a badge row
  that is part of the hero rather than a section of its own.

### US-17.6: Reconcile the Custom Software page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: 90e55ea
- What: eleven text edits plus three link changes. The hero, the five step
  process and the bottom CTA carried none of the spec's copy; sections 02
  through 07 already matched it verbatim, including all six problem cards, the
  GCC spotlight and all three sub-service blocks with their GCC, USA and UK
  market notes and deliverables.
- How: text nodes only apart from the three links. All 147 style attributes on
  the changed lines are identical before and after, and the diff adds and
  removes no elements. Structure unchanged: one h1, 13 h2, 14 sections, 10
  details.
- Hero: the H1 highlight word becomes "Business" from "Workflows"; the
  subheadline becomes the spec's sentence about web apps, dashboards and APIs;
  the primary CTA becomes "Get a Free Software Audit" with offer=audit; the
  secondary becomes "See How It Works" pointing at #process, which existed on
  the process section with nothing linking to it, the same defect found on
  ai-automation in US-17.3.
- Process: the heading becomes "How We Build Your Custom Software, Start to
  Finish" and the five steps become Discovery & Planning, Design, Build & Test,
  Launch and Support & Scale with the spec's descriptions.
- Bottom CTA becomes "Ready for Software That Actually Fits Your Business?" with
  the spec's subline and the label "Book Your Free Software Audit". This spec
  sets a rule that no button on the page may read "Contact Us" and every button
  must say what happens next, and all nine of its named CTAs now match.
- Four FAQs rewritten to the spec's wording, in the accordion and the JSON-LD
  together, verified in sync at nine questions each.
- Why: the parts a visitor reads first and last, the hero and the closing CTA,
  were the parts that matched the spec least.
- Acceptance: every section of the content report present in its order and
  wording, except the item recorded below.
- Commitments this introduces, all the client's own words and worth a second
  look before publication: prices of $8,000, $20,000 to $60,000 and $50,000
  upward where the page previously gave no figures; timelines of 6 to 10, 12 to
  20 and 20 to 40 weeks where it said weeks to months; "Every project comes with
  a 30-day warranty", a contractual term that appears nowhere else on the site;
  and code ownership now conditioned on final payment, where the page had said
  the code is yours without that condition.
- Not applied: the spec's hero trust bar reads "75+ Custom Builds Delivered,
  PSEB Registered, Full-Stack Team, GCC, USA & UK". PSEB is barred by the
  EPIC-17 standing decision, and the remainder is a text bar where the build
  carries a four stat grid.
- Not applied, and already correct on the page: the spec marks its three
  testimonials as placeholders to be replaced with real client quotes before
  launch. The page carries the same three quotes attributed by role and market
  only, with no invented names, which is the safer of the two.
- Checked and left alone: three eyebrows on this page write the ampersand as
  "&AMP;" rather than "&amp;", an artifact of uppercasing after escaping. That
  is a valid HTML5 legacy named reference and renders as "&", so it is a source
  inconsistency rather than a defect. The same pattern is in faqs, industries,
  odoo-erp, pos-systems and social-media-marketing.

### US-17.7: Reconcile the Digital Transformation page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: db83fd8
- What: two text edits. This page was the closest to its spec of any reconciled
  so far.
- How: text nodes only. No element added or removed, every style attribute and
  every href identical, verified from the patch. Structure unchanged: one h1,
  15 h2, 16 sections, 9 details, 8 schema questions.
- The H1 becomes "Digital Transformation Solutions", the spec's exact heading,
  from "Digital Transformation Solutions for GCC, USA & UK". The amber accent
  span is kept and moved onto the word "Solutions" so the hero keeps the
  highlight pattern every other page uses, rather than losing it along with the
  geography. Dropping the geography also matches the SEO instruction in the
  Custom Software report, which says to keep the H1 broad and name the markets
  in the subheadline and section copy instead.
- The final CTA button becomes "Get a Free Assessment" from "Book Your Free
  Transformation Assessment".
- Already matching, and left alone: all fifteen sections in the spec's order,
  the six pain point cards, the three state labels, the GCC spotlight with all
  three badges, all three sub-service blocks with their full deliverables lists
  and tech stacks, the six AI cards, the five process steps, the six industry
  cards, all three pricing tiers with prices, features and the MOST POPULAR
  badge, the four stat figures and all eight FAQs.
- Already handled by US-16.1 and kept, per the EPIC-17 standing decision: the
  hero trust bar reads "US Registered" where the spec says "PSEB Registered",
  and testimonial three is attributed to "Managing Director, Professional
  Services Firm" where the spec adds "Karachi, Pakistan".
- Not changed, a style call rather than a content one: the spec expands
  contractions throughout, writing "It is about rethinking" and "AI Is Not the
  Future", where the build contracts them. The information is identical and the
  contracted form is the site's established voice, so it was left. Say the word
  and it can be matched to the spec exactly.
- Defect found, not fixed, needs a decision. The pricing note, "All packages
  include discovery, documentation, and knowledge transfer...", is a section
  level note in the spec that applies to all three tiers. In the build it sits
  inside the Enterprise Transformation card, between its "Custom Quote" line and
  its feature list, so it reads as if it applied to that tier alone. Fixing it
  means moving the paragraph out of the card and below the grid, which is a
  structural move rather than a copy change, so it was left for a decision.

### US-17.8: Reconcile the Odoo ERP Services page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: 29310a8
- What: eleven text edits plus three link changes on odoo-erp.html.
- Which page: the report was handed over against enterprise-odoo.html and does
  not belong to it. It is headed "Odoo ERP Services Page", its breadcrumb is
  Home > Services > Odoo ERP Services, and its sections match odoo-erp.html one
  for one, including the six problem cards, "Odoo ERP, Set Up to Fit Your
  Business", "Odoo ERP Compliance for UAE and Saudi Arabia" and the four
  sub-service blocks. enterprise-odoo.html is the solution page and carries none
  of them: it has The Four Pillars of Enterprise Automation, Flexible Engagement
  Models and industry fit. Applying the report there would have overwritten a
  distinct page with service copy and left the page it was written for
  untouched. Confirmed with the client before any edit.
- How: text nodes only apart from the three links. No element added or removed,
  all 90 style attributes on the changed lines identical, verified from the
  patch. Structure unchanged: one h1, 14 h2, 15 sections, 11 details, FAQ schema
  and accordion in sync at 10 questions.
- Hero: the H1 becomes "Odoo ERP Services Built Around How You Work" from "Odoo
  Implementation, Customisation & Support", with the amber accent span moved to
  "How You Work"; the subheadline becomes the spec's one-liner; button one
  becomes "Get a Free ERP Audit" with offer=audit; button two becomes "See How
  We Work" pointing at #process.
- The #process anchor existed with nothing linking to it. That is the third page
  in this sweep with the same defect, after US-17.3 and US-17.6.
- How We Work: the heading becomes "How We Deliver Your Odoo Project", the five
  steps become Free Discovery Call, Plan & Proposal, Build & Configure, Migrate
  & Go Live and Train & Support, and the CTA becomes "Start With a Free
  Discovery Call".
- Bottom CTA becomes "Ready to Stop Running Your Business on Spreadsheets?" with
  the spec's subline and the label "Book Your Free ERP Audit".
- Already matching: all six problem cards, the What We Offer intro and its four
  pills, the compliance spotlight with all three stat boxes, and all four
  sub-service blocks with bodies, deliverables and GCC, USA and UK region notes.
- Already correct and worth noting: this spec instructs that placeholder
  testimonials must not be published, because a fake-looking quote does more
  damage than an empty section, and that the stats bar should reuse the figures
  already published elsewhere on the site. The page states that client quotes
  are being collected and carries 50+, 15+, 98% and 4 to 8 weeks. Both
  instructions were already satisfied.
- Not applied: the spec's hero trust bar includes "PSEB Registered", barred by
  the EPIC-17 standing decision, and the rest is a text bar where the build
  carries a four stat grid.
- Not applied, and a deliberate choice rather than an oversight. The spec lists
  eight FAQs and the page carries ten. Five match verbatim, including the cost
  and duration answers with their dollar and week ranges. Three are phrased
  differently: the spec asks "Will our existing data migrate safely?", "What
  happens if we need changes after go-live?" and "Do you provide training for
  our team?", where the page asks how migration is done and how training and
  post go-live support work. Every topic the spec raises is covered, and the
  page's versions are more specific, so replacing them would have removed
  content to gain nothing. Flagged for a decision rather than forced.
- Open follow up: the spec's bottom CTA asks for a second button, "Download Our
  Odoo Guide", pointing at /resources/odoo-erp-guide/. The spec itself notes
  that the PDF must exist first because an outlined button linking to a 404 is
  worse than no button. The page does not exist, so the button was not added.

### US-17.9: Reconcile the Enterprise Odoo page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: 82db461
- What: thirty-four text edits plus five section taglines on enterprise-odoo.html.
  This is the report that belongs to this page, unlike the Odoo ERP Services one
  handled in US-17.8. Its sections match one for one.
- How: the diff adds five div elements, removes nothing, changes no href, and
  introduces no new style string, because the taglines reuse the two eyebrow
  variants already on the page. Structure otherwise unchanged: one h1, 12 h2,
  13 sections, 8 details, JSON-LD parsing.
- Hero: the tagline becomes "Stop Running Your Business on Spreadsheets" from
  "Odoo Implementation & Customization", and the body becomes the spec's "Tired
  of jumping between five different tools" paragraph.
- The Why Odoo intro becomes the spec's plain-language version about no vendor
  lock-in, replacing "open-source advantages with a modular architecture that
  ensures maximum scalability".
- All four module card bodies. Each had been cut to a single feature summary
  sentence where the spec opens with the problem, so HR now begins "Stop chasing
  timesheets and fixing payroll errors" and CRM "Stop losing leads in email
  threads".
- Nineteen of the twenty-four feature list labels had been shortened, for
  example "Employee Self-Service" for "Employee Self-Service Portal" and "Batch
  Tracking" for "Batch & Serial Tracking". All restored.
- All five implementation journey steps were three-word fragments: "Deep process
  audit.", "System architecture.", "Tailored configuration.", "QA & migration."
  and "Rollout & support.". They now carry the spec's sentences.
- Defect fixed: the Related Solutions section carried a stray link reading
  "Solution" with no card behind it. It now reads "Explore All Solutions" and
  points at /solutions/ as before.
- Already correct and kept: the spec's Partners & Certifications list names
  PSEB, FAST SOFTEC, Cisco Networking Academy, NAVTTC and Odoo Partner. The page
  carries three of them, PSEB and NAVTTC having been removed by US-16.1, which
  the EPIC-17 standing decision preserves.
- Why: this page had the widest gap between spec and build of any page in the
  sweep. The module bodies and journey steps in particular had been compressed
  to the point where the page described features without ever naming the problem
  they solve, which is the whole structure the spec is built on.
- Acceptance: every section of the report present in its order, with its copy,
  except the items recorded below.
- Not applied, all of which need new elements rather than new words: the hero
  subheading line "One Platform. One Dashboard. Every Department Working
  Together."; the four stat context lines under Why Odoo, such as "Trusted
  worldwide"; the implementation journey intro; the eight integration category
  body lines, such as "Accept payments your customers already trust"; and the
  Why Choose intro.
- Not applied, FAQ. The spec lists ten questions and the page carries seven.
  The seven cover the spec's Q1 to Q7 with the same facts in slightly shorter
  wording. Missing entirely are Q8 on whether Odoo suits small businesses, Q9 on
  implementation cost, and Q10 on why to choose Big Binary Tech. Adding them
  means new accordion entries and matching FAQPage schema, so it was left for a
  decision.
- Spec defect worth noting: the report lists Retail & E-Commerce twice, as
  Industry 1 and again as Industry 3 with different body copy, and its
  integration categories skip number 3. The page fills the third industry slot
  with Trading & Distribution, which is not in the report but is the sensible
  reading, and adds a Communications integration category. Both were left.

### US-17.10: Reconcile the Industries page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: 4fa7760
- What: thirty-six text edits across the hero, the ten vertical sections, the
  compliance table and the FAQ.
- How: text nodes only. No element added or removed, every style attribute and
  every href identical, verified from the patch. Structure unchanged: one h1,
  16 h2, 17 sections, 7 details, FAQ schema and accordion in sync at six.
- The main finding: the ten vertical headings were duplicating their own
  eyebrow. Each section opened with an eyebrow reading, for example, "// RETAIL
  & E-COMMERCE" followed by an h2 reading "Retail & E-commerce", so the sector
  name appeared twice and the heading said nothing. The spec gives each vertical
  a descriptive heading and all ten are now in place, from Unified Omnichannel
  Retail Technology through to Secure, Compliant Digital Government Platforms.
  The eyebrows are untouched, so each section now names the sector and then says
  what gets built. The 2026-08-05 spec audit had recorded this as a naming
  difference and left it; it is closed now.
- All ten vertical summary cards regain the spec's fuller wording, the three Why
  Industry-Specific features and the hero body return to the spec's phrasing, and
  the first delivery step regains "competitors".
- The compliance table regains the regulators the build had dropped: DHA and
  MOHAP for healthcare, Central Bank for financial services, FTA for retail,
  NESA and SEDD for government, HACCP for manufacturing, KHDA and ADEK for
  education. Its retail approach line also returns to "VAT-ready invoicing,
  Arabic receipts" from "Tax-ready invoicing, localized receipts".
- Two FAQ questions restored to the spec's wording in both the accordion and the
  JSON-LD.
- Not applied, a claims decision rather than a copy one. The spec's hero stats
  bar reads "200+ Industry-Specific Projects" and the page says 60+. Raising it
  is the opposite of what docs/portfolio-case-studies.md asks, which lists that
  exact claim on this page as one to remove for lack of evidence, and
  docs/stats-verification.md records it as unverified. The lower figure stays
  until someone decides which is true. The spec's "PSEB Registered" badge is
  already "USA Registered" from US-16.1 and the EPIC-17 decision keeps it.
- Not applied, whole sections the page does not have. The spec defines four that
  are absent: section 14 The BBT Edge, "We Speak Your Industry's Language";
  section 15, "Regulatory Rigor and Data Residency"; section 18, Global Success
  Stories with four case snippets; and section 19, "Proven Results Across
  Verticals". The 2026-08-05 audit already recorded the last two as missing.
  Building them is new sections rather than new words.
- Not applied, FAQ count. The spec lists eight questions and the page carries
  six. Missing are "Do you offer Arabic support for GCC industries?", which the
  2026-08-05 audit also flagged, and "How do you handle data migration from our
  old industry system?". Adding them needs new accordion entries and matching
  schema.
- Also not applied: the spec's hero stats bar has five items, two of which the
  page does not carry as stats, "GCC, USA, UK" and "Domain Expertise".

### US-17.11: Build the four Industries sections the spec defines but the page lacked

- Type: User Story
- State: Closed (Done)
- Commits: 057e2a8
- What: the four sections and two FAQs held back from US-17.10, plus the two the
  2026-08-05 spec audit had recorded as missing since then.
- How: pure addition. The diff removes no line and no tag from the existing
  markup, 56 lines added and 0 removed, with section and div tags balancing at
  21 and 280. Nothing new was designed. Every section is assembled from
  components already on this page: the eyebrow, h2 and lead paragraph pattern
  from the Why Industry-Specific section, bb-card for the cards, and the page's
  own bb-statg with bb-statg-5 and bb-on-dark for the stat strip. Only the two
  existing typefaces appear, every colour is from the site palette, and every
  font size matches a value already in the file.
- Sections added, in the spec's positions: The BBT Edge and Regulatory Rigor and
  Data Residency after the last vertical and before Compliance and Standards;
  Global Success Stories and Proven Results Across Verticals after the delivery
  process and before the FAQ. Backgrounds continue the page's alternation.
- Responsive: the card rows use g3 and g4 and the stats use bb-statg-5, all of
  which already carry collapse rules at 980 and 620 pixels. US-15.1 records that
  writing the column count as an inline style outranks those media queries and
  broke seven pages at 390px, so it is written as classes here.
- Two FAQs added, on Arabic support for GCC industries and on data migration
  from an old system, in the accordion and the JSON-LD together and in the
  spec's order. Verified afterwards at 8 questions each, matching one for one.
- Content decision on the stat: the spec's Proven Results bar reads "200+
  Industry Projects" and this section carries 60+, matching the hero on the same
  page. Publishing 60+ at the top and 200+ lower down is the exact contradiction
  docs/stats-verification.md warns about, and docs/portfolio-case-studies.md
  lists the 200+ figure for this page as one to remove for lack of evidence.
- Content decision on the success stories: the four are written with the sector,
  the region and the outcome but no client named. The spec supplies no names, and
  the project standard is not to publish attributed quotes nobody will stand
  behind.
- Acceptance: verified rendered in order on a local server, 21 sections each
  with its eyebrow, tags balanced, JSON-LD parsing, no horizontal overflow risk
  since every grid uses the page's existing responsive classes.

### US-17.12: Close the last gaps on the POS Systems page

- Type: User Story
- State: Closed (Done)
- Commits: 337258f
- What: three fixes, none of them visible copy. This page was built from this
  handoff in US-15.1 and is the closest match of any page in the sweep.
- Already correct, and left alone: the hero and its trust bar, all six problem
  cards, the What We Offer intro with three pills and the six-node flow, the
  Gulf spotlight with all three stat boxes, all three sub-service blocks with
  outcome lines, bodies and Gulf, USA and UK market notes, all five process
  steps, the stats bar, and all eight FAQs, which are verbatim down to the
  dollar and week ranges. The meta description also already matched exactly.
- The meta title read "POS Systems, Big Binary Tech" and is now the handoff's
  "Custom POS System Development Company | Big Binary Tech".
- The two audit CTAs, in the hero and the bottom banner, lacked the offer
  parameter the handoff specifies. Both now carry offer=audit, which the GA4
  hook added in US-15.1 reads.
- Schema: the handoff asks for three types and the page had one. Service and
  BreadcrumbList now sit alongside the existing FAQPage, all four blocks parse,
  and the Home, Services, POS Systems trail is in structured data. The Service
  block references the existing organization node by id rather than repeating
  it. This is the first page in the sweep to carry Service schema, and the same
  gap exists on the other service pages.
- How: no element removed, every style attribute unchanged, and the only markup
  added is the two schema scripts. Structure unchanged at one h1, 10 h2, 11
  sections, 9 details, FAQ schema and accordion in sync at eight.
- Not changed: the bottom banner's secondary button reads "Talk to an Engineer"
  where the handoff wants "Download Our POS Buying Guide" pointing at
  /resources/pos-buying-guide/. That page does not exist and the handoff itself
  says the PDF must be created first, so a working button was kept over a 404.
  This is the third page whose spec asks for a lead magnet that has not been
  written, after ai-automation and custom-software-development.
- Not changed: the testimonials section still states that client quotes are
  being collected. The handoff is explicit that a quote must carry a real name
  and that no placeholder name may be published.
- Also verified: the handoff's menu instruction, replacing Digital
  Transformation with POS Systems as the last item in the Services mega menu, is
  already applied sitewide.

### US-17.13: Reconcile the Resources page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: 1b8ab84
- What: six text edits plus one badge.
- How: the diff removes no tag, leaves every style attribute and every href
  identical, and adds a single span. Structure unchanged: one h1, 14 h2, 14
  sections, 13 details, FAQ schema and accordion in sync at 12.
- The hero trust bar carried four badges where the spec lists five. "60+
  PROJECTS" becomes "60+ TRANSFORMATION PROJECTS", "AI-FIRST" becomes "AI-FIRST
  APPROACH", and the missing "GCC, USA, UK" badge is added.
- The hero paragraph reads "actionable insights", the spec's word. The first
  e-book card regains its full title, "Odoo Implementation Playbook for GCC".
  Three topic labels return to the spec's fuller versions: "Legacy System
  Modernization", "ZATCA & GCC Compliance" for "Tax & VAT Compliance", and
  "Cybersecurity & Risk Management".
- A correction made in passing, recorded because it nearly shipped: "Legacy
  Modernization" appears three times on this page, as a topic tag on the
  featured case study, as a blog card heading, and as a topic grid item. Only
  the grid item takes the spec's longer form. The first pass replaced all three
  and the other two were put back before commit. A global replace on a short
  label is not safe on a page that reuses category names.
- Already handled by US-16.1 and kept: the spec's trust bar reads "PSEB
  Registered" where the page says "US REGISTERED", and the spec labels both case
  study card two and testimonial two "Professional Services, Karachi" where the
  page carries no location.
- Not changed, and stated here because it reads as a difference. The spec writes
  the markets as "the GCC, USA, and UK"; the page writes "the USA, the GCC, the
  UK and MENA" in the hero and the case studies intro. Leading with the USA is
  the point of US-16.1, so reordering would work against it.
- Section 09 of the spec says of the FAQ "keep this section as it is", so it was
  left untouched.
- Not applied, would duplicate content: the spec's section 3 is an E-Book
  Spotlight for the Odoo playbook, with the label "THE ODOO PLAYBOOK" and the
  heading "Comprehensive Guide to ERP Implementation in the GCC Region". That
  same asset is already the first card in the E-Books and Guides section, so
  building the spotlight would put one download on the page twice.
- Not applied, needs a new cell: the spec's featured case study results table has
  five rows and the page's stat grid shows four, missing "Downtime, Zero". The
  grid is a fixed four column layout, so a fifth cell reflows it.

### US-17.14: Reconcile the Retail and POS page to its content report

- Type: User Story
- State: Closed (Done)
- Commits: 2c216ba
- What: three changes. One div added, nothing removed, no href changed, no new
  style string. Structure unchanged: one h1, 15 h2, 16 sections, 9 details, FAQ
  schema and accordion in sync at eight.
- The hero had no tagline and now carries the spec's "Retail Technology" above
  the H1, matching the eight eyebrows already on the page.
- Defect fixed: the Related Solutions section carried two truncated links
  reading "Solution" and "Service", pointing at /solutions/ and /services/. They
  now read "Explore All Solutions" and "Explore All Services". This is the same
  defect fixed on enterprise-odoo in US-17.9, so it is a pattern in the related
  solutions block rather than a one-off, and the remaining pages with that block
  should be checked.
- Already matching and left alone: all six pain point cards, the What We Offer
  body and its four visual labels, the GCC spotlight with all three badges, all
  four sub-service blocks with subheadlines, bodies, feature lists and tech
  stacks, all four integration categories, all five process steps, all three
  pricing tiers with prices and the pricing note, the four client result stats,
  all eight FAQs verbatim including the dollar and week ranges, and the final
  CTA.
- Already handled by US-16.1 and kept: the hero stats bar reads "US Registered"
  where the spec says "PSEB Registered", and testimonial three is attributed to
  "Owner, Electronics Store" where the spec adds "Lahore, Pakistan".
- Left as it is: the bottom CTA's secondary button, "Download Our POS Buyer's
  Guide", points at /e-books-guides/, which exists. The spec gives this section
  one CTA only, so the second is additive rather than wrong, and unlike the guide
  buttons on ai-automation, custom-software-development and pos-systems it does
  not 404.
- Spec contradiction worth recording: this report says "80+ POS Systems
  Deployed" where the POS Systems handoff behind US-17.12 says 60+, and the two
  pages currently publish both. docs/stats-verification.md lists the 80+ figure
  as unverified and docs/portfolio-case-studies.md asks for its removal on the
  grounds that one in-house POS build exists. Neither number was changed here,
  pending a decision on the real figure.
- Spec self-contradiction, also unresolved: section 01 gives the H1 as "Retail
  and POS Solutions" while the SEO section at the end gives it as "Retail & POS
  Solutions for GCC, USA & UK". The page carries the second, so it was left.

---

## Going forward

New code changes are tracked the same way: a User Story per change, assigned to
You, with what changed, how, and why, and the commit hash cited on completion.
No em dashes and no en dashes in any item.
