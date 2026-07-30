# Chatbots, RAG, LLM integration and data warehousing: draft site copy

Status: **draft, nothing published.** Written to match the existing voice on
/ai-automation/ (second person, concrete, two sentence card bodies, three `›`
bullets per card). Review, mark up, and I will place it.

Everything below is written so that **no claim depends on a number we cannot
stand behind.** There are no project counts, no accuracy percentages and no
client names in this copy, deliberately. See the "Claims discipline" section at
the end.

---

## 1. Recommended placement

| What | Where it goes | Why there |
| --- | --- | --- |
| AI Chatbot Development | /ai-automation/ capability grid | It is an automation outcome, and the grid already has a thin "AI Assistants" card to grow from |
| RAG / document grounded assistants | /ai-automation/ capability grid + new detail section | Buyers search for it by name and it is the strongest differentiator of the four |
| LLM Integration | /ai-automation/ capability grid | Sits with the API Connections story already on that page |
| Data Warehouse Development | /custom-software-development/ | It is data engineering, not AI. It is also the honest sales order: warehouse first, model on top |
| All four | /services/ hub, service 01 and 02 bullet lists | So the hub reflects what the detail pages now sell |

This keeps the six service lines intact, so the mega menu, the footer and the
`6 CORE SERVICE LINES` chip need no changes.

The capability grid on /ai-automation/ goes from six cards to nine, so its intro
line changes from "Six capabilities that work together" to "Nine capabilities
that work together". Three by three still grids cleanly at every breakpoint.

---

## 2. New capability cards for /ai-automation/

Drop in after the existing "AI Assistants" card. Same card markup, same three
bullet pattern.

### AI Chatbot Development

> Custom chatbots built on OpenAI, Claude or Gemini and connected to the systems
> that hold your answers. Not a scripted decision tree, and not a generic widget
> bolted onto your site.
>
> - › Website and customer support bots
> - › WhatsApp and internal helpdesk bots
> - › Escalation to a human, with full history

### Document Grounded Assistants (RAG)

> Assistants that answer from your own documents rather than from whatever the
> model learned on the public internet. Every answer points back to the file and
> the page it came from, so your team can check it.
>
> - › Contracts, policies and SOPs
> - › Product and technical documentation
> - › Source citations on every answer

### LLM Integration

> The language model wired into the software you already run, so it acts on real
> records instead of sitting in a separate chat window. Drafting, summarising,
> classifying and extracting, inside your own workflow.
>
> - › Model integrated into your app or ERP
> - › Prompt, cost and rate limit handling
> - › Fallbacks when a model is slow or down

---

## 3. New detail section for /ai-automation/

Place below the capability grid, above "How It Works". Follows the existing
two column pattern used elsewhere on the site.

**Eyebrow:** `// CHATBOTS, RAG AND LLM SYSTEMS`

**Heading:** Answers From Your Own Data, Not From the Open Internet

**Intro:** A general purpose chatbot will confidently invent an answer about
your refund policy. One built on your documents will quote the policy and tell
you which page it came from. That difference is the whole reason to build rather
than subscribe.

### Column A: What we build chatbots for

- **Customer support.** Handles the questions your team answers twenty times a
  day, and hands over to a person the moment it is out of its depth.
- **Internal knowledge base.** Staff ask in plain language instead of hunting
  through a shared drive for the current version of a document.
- **Website assistant.** Answers pre sales questions, qualifies the enquiry and
  passes a complete lead to your CRM.
- **Inside your product.** An assistant embedded in your own SaaS or portal,
  scoped to what each user is allowed to see.

### Column B: What we ground them on

- **PDFs and scanned files.** Manuals, reports and anything already sitting in a
  folder rather than a database.
- **Company documentation.** Policies, SOPs and process guides, kept in step as
  they are revised.
- **Contracts and legal documents.** Clause lookup and comparison, with the
  source paragraph shown every time.
- **HR material.** Leave, payroll and onboarding questions, answered without
  routing every one to HR.
- **Product documentation.** Specs, release notes and support articles, for
  customers and for your own team.

### Closing line for the section

Access is scoped before anything is indexed. An assistant only ever reads the
documents you point it at, permissions follow the ones already set on those
files, and every question and answer is logged so you can audit what it told
people.

---

## 4. New capability card for /custom-software-development/

### Data Warehouse Development

> One place where your ERP, POS, CRM, finance tools and spreadsheets finally
> agree with each other. Modelled properly, refreshed on a schedule, and built
> to be reported on rather than exported from.
>
> - › Pipelines from your existing systems
> - › Clean, modelled tables for reporting
> - › Dashboards on live data, not exports

### Optional supporting paragraph

Most reporting problems are not reporting problems. They are five systems each
holding a slightly different version of the same number, reconciled by hand once
a month in a spreadsheet. A warehouse ends that argument: data lands once, gets
modelled once, and every dashboard and every AI assistant reads the same figures.

**Confirm before publishing:** the tools named here have to be ones you actually
run. Tell me which apply and I will name only those. Candidates based on what
the careers page already advertises: PostgreSQL, Azure, AWS, Python. Commonly
paired with these: BigQuery, Snowflake, dbt, Power BI, Metabase.

---

## 5. Updates to the /services/ hub

Service **01 · AI & AUTOMATION**, replace the four bullets:

| Now | Proposed |
| --- | --- |
| › Workflow Automation | › Workflow Automation |
| › AI Chatbots & Assistants | › AI Chatbots & Assistants |
| › Process Integration | › Document Grounded Assistants (RAG) |
| › Custom API Connections | › LLM Integration & API Connections |

Process Integration drops off the hub summary because it survives on the detail
page and the four slots are better spent on what nobody else in the market
names explicitly. Say the word if you would rather keep it and run five bullets.

Service **02 · CUSTOM SOFTWARE**, replace the fourth bullet:

| Now | Proposed |
| --- | --- |
| › Cloud-Based Solutions | › Data Warehousing & Analytics |

---

## 6. FAQ additions for /ai-automation/

Same accordion, appended to the existing five.

**Which AI model do you use?**
Whichever fits the job and your constraints. OpenAI, Claude and Gemini are all
strong, and they differ on cost, speed, context length and where the data is
processed. If your data has to stay in a particular region, or inside your own
cloud tenancy, that decides it before anything else does. We tell you what we
recommend and why, and the system is built so the model can be swapped later
without a rebuild.

**Will our documents be used to train someone else's model?**
No. We use business tier APIs where the provider does not train on submitted
data, and your documents stay in storage you control. Where data cannot leave
your environment at all, we say so early, because that rules some options out
and changes what the build costs.

**How do we know the answers are right?**
Every answer cites the document and section it came from, so an answer is
checkable in one click rather than taken on trust. Before launch we test the
assistant against a set of real questions with known correct answers, and where
it should not guess, we configure it to say it does not know and route the
question to a person.

**Do we need a data warehouse before we can use AI?**
Not for a document based assistant, which reads files as they are. You do need
one the moment you want answers about numbers, because a model cannot reconcile
five systems that disagree. If that is where you are heading we usually build
the warehouse first, since it pays for itself on reporting alone.

---

## 7. Pricing page: the re-tiering this forces

/pricing/ currently puts **"Standard AI chatbot setup"** in the $3,500 Starter
tier and **"Chatbot only"** in the Starter column of the comparison table.

Publish RAG as an offering and that line becomes a problem: a document grounded
assistant is a materially bigger build than a scripted bot, and a buyer who
reads the AI page and then the pricing page will anchor on $3,500 for it.

Proposed comparison table row, replacing the current AI Automation row:

| Tier | Now | Proposed |
| --- | --- | --- |
| Starter | Chatbot only | Scripted chatbot, single channel |
| Growth | 2 to 3 workflows | Document grounded assistant, 2 to 3 workflows |
| Enterprise | Full AI/ML integration | Full LLM integration, warehouse and custom models |

And the Starter bullet changes from "Standard AI chatbot setup" to "Scripted
chatbot setup, one channel".

This is the only change in this document that touches a page you have already
signed off, so it is flagged separately rather than bundled in.

---

## 8. Claims discipline

What this copy deliberately does **not** say, and should not be edited to say
without evidence behind it:

- Any count of chatbots, assistants or warehouses delivered
- Any accuracy or deflection percentage
- Any named client for an AI build
- Any claim of a partnership or certification with OpenAI, Anthropic or Google

The existing site already publishes three different project totals, which is
tracked in [stats-verification.md](stats-verification.md). Adding a fourth set
of unverifiable numbers to a brand new service line is the fastest way to make
that worse. The offering is persuasive on specifics alone: the use cases, the
grounding sources, the citation behaviour and the permission model are all
things competitors do not spell out.

One thing worth noting in your favour: [careers.html](../bigbinarytech-website-redesign/project/build/careers.html)
already advertises hiring for LLM powered chatbots with OpenAI API and LangChain,
and the DataSap work on the home page carries an Azure OpenAI tech chip. So the
capability is already claimed publicly. This copy makes it sellable rather than
introducing it from nothing.

---

## 9. What I need from you

1. Approve or mark up the copy above
2. Confirm the data warehouse tool names in section 4
3. Decide on the pricing re-tier in section 7, since it changes a signed off page
4. Confirm whether Process Integration keeps its bullet on the services hub
