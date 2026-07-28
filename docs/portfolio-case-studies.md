# Portfolio and case study content

Source material for the Selected Work section and the case study pages. Written
from repositories that actually exist, with clients anonymised.

## Ground rules

1. **Every claim here traces to something real.** Where a number is not
   measurable from the repository or a client has not confirmed it, the number
   is absent rather than estimated. An absent number is recoverable. A wrong one
   published on a live site is not.
2. **Anonymised means the client is unnamed, not that the project is invented.**
   Sector plus region plus problem is enough to be credible.
3. **No em dashes and no en dashes**, per the project standard set in US-3.4.
4. Items marked **NEEDS CONFIRMATION** must be checked with the client or
   measured before they go live.

## What these replace

The live home page currently shows three Selected Work items and a revenue
figure that no repository supports:

| Currently live | Problem |
| --- | --- |
| Retail ERP, "40+ outlets" | No repository evidence of a 40 outlet deployment |
| Logistics, "cut manual dispatch work by 60%" | No logistics or dispatch repository exists |
| SaaS, "Stripe billing" portal | No billing or Stripe repository exists |
| "PKR 24.6M, up 31%" monthly recurring revenue | Company revenue figure presented as a product metric |

Replace all four. The material below is the honest version.

---

## CASE STUDY 1: Learning operations platform

**Sector:** Education technology
**Built with:** TypeScript, modern web stack
**Source:** internal product, BBT LearnOS

**The problem.** Training providers run enrolment, course delivery, learner
progress and outcomes across separate tools that do not talk to each other.
Administrators reconcile by hand and learners lose track of where they are.

**What we built.** A career operating system that holds the learner journey in
one platform: enrolment, structured course delivery, progress tracking and
outcome reporting, with role separation between learners, instructors and
administrators.

**Why it matters.** This is the largest codebase we have built to date, and it
is our own product rather than a one off engagement, so it is maintained
continuously rather than handed over and abandoned.

**Verifiable:** the platform exists and is under active development.
**NEEDS CONFIRMATION:** learner numbers, institutions using it, retention.

---

## CASE STUDY 2: Point of sale and inventory system

**Sector:** Retail
**Built with:** PHP
**Source:** internal product, POS

**The problem.** Small and mid sized retailers run the till on one system and
stock on a spreadsheet. The two never agree, so month end reconciliation is
manual and stock decisions are guesswork.

**What we built.** A point of sale system that keeps checkout and inventory on
the same data, so a sale updates stock as it happens rather than at end of day.

**Honest framing.** This is an in house build, currently at working system
stage. It demonstrates the capability behind the Retail and POS solutions page.

**Do not claim:** the "80+ POS systems deployed" figure on the solutions page.
Nothing supports it.
**NEEDS CONFIRMATION:** any live retail deployment, transaction volume.

---

## CASE STUDY 3: Campus management mobile application

**Sector:** Education
**Built with:** Flutter, cross platform mobile
**Source:** internal product, CampusPlus

**The problem.** Institutions communicate with students through noticeboards,
email and messaging groups. Students miss schedule changes, results and fee
deadlines because nothing reaches them where they actually are.

**What we built.** A cross platform mobile application putting schedules,
results, announcements and fee status in the student's pocket, from one codebase
serving Android and iOS.

**Why it matters.** Demonstrates real mobile delivery, not just web. The single
codebase approach halves the maintenance surface compared with two native apps.

**NEEDS CONFIRMATION:** deployment status, active users.

---

## CASE STUDY 4: Corporate web platform

**Sector:** Technology services
**Built with:** HTML, CSS, JavaScript, responsive build
**Source:** DataSap Labs website

**The problem.** Technical companies get judged on their own website before
anyone reads a proposal. A slow or dated site undercuts the credibility of the
work being sold.

**What we built.** A responsive marketing platform for an AI and data
engineering consultancy: service architecture, capability presentation and lead
capture, built to load fast on any device.

**Disclosure:** DataSap Labs is a related company under the same ownership.
Present it as a sister brand build, not as an arms length client engagement.

---

## CASE STUDY 5: Education sector web platform

**Sector:** Education
**Built with:** HTML, CSS, JavaScript
**Source:** BBT EDU

**The problem.** Education providers need to present programmes, handle
enquiries and convert interest into enrolment, usually without a technical team
to maintain the result.

**What we built.** A programme focused web platform with structured course
presentation and enquiry capture, built so non technical staff can maintain it.

---

## CASE STUDY 6: Learning delivery platform

**Sector:** Education technology
**Built with:** JavaScript
**Source:** internal product, Khoj

**The problem.** Course content, learner interaction and progress usually live
in three disconnected places, so nobody can see whether learning is working.

**What we built.** A learning platform bringing content delivery, learner
interaction and progress signals together in one interface.

---

## CAPABILITY PIECE: Search and AI enrichment at corpus scale

**This is not a client case study. Publish it as a capability article with no
client named, no system named and no metrics attached.**

**Why it is written this way.** The underlying engagement is covered by the
client's confidentiality. Their internal service names, ticket references, event
topic names and data models must not appear on a public site. What follows
describes the shape of the problem and the engineering approach only. Nothing
here identifies the client, and it should stay that way.

**The problem class.** When a content corpus reaches millions of documents,
naive search stops working. Users ask in natural language, filters must be exact,
and the relationships between concepts are richer than keyword matching can
express.

**The architecture we work in.**

- **Event driven indexing.** The content system emits a change event when a
  document is published or materially updated. Independent consumers subscribe:
  one rebuilds the search index, another generates metadata. The content system
  never calls them directly, so new consumers are added without touching it.
- **Denormalisation at index time.** Relationship traversal is expensive. Doing
  it while a user waits is the wrong trade. The indexer resolves relationships
  once at write time and flattens the result into the search index, so query
  time is simple filtering.
- **Asymmetric include and exclude logic.** Searching *for* an ingredient should
  be narrow: asking for one thing should not return every relative of it.
  Excluding an ingredient must be broad: excluding a grain has to also exclude
  everything derived from it. These need separate index fields, because
  collapsing them into one gets one of the two cases wrong.
- **AI as enhancement, never as dependency.** A language model can turn a
  sentence into structured filters. It can also fail, time out or return
  nonsense. Model output is validated against an allow list before it reaches
  the query builder, and if validation fails the request falls through to the
  deterministic path. Model failure must never raise the baseline search failure
  rate.
- **Durable workflow orchestration.** Multi step AI enrichment spans unreliable
  network calls. A durable workflow engine holds state so a failed step retries
  on its own rather than restarting the whole job, and survives restarts and
  deployments.

**Capabilities this demonstrates:** event driven architecture, search
engineering, taxonomy and ontology modelling, LLM integration with safe
fallback, durable workflow orchestration, offline analytics pipelines.

**Suggested title:** Building search that understands meaning without betting
availability on a language model.

---

## Home page Selected Work: what is live

Superseded on 28 July 2026. The client directed that the education and retail
products should not headline bigbinarytech.com, because the site sells AI and
automation and that work sits with DataSap Inc, a sister company under the same
ownership.

The three cards now live are DataSap platforms, shown with the product artwork
from datasapinc.com and attributed in a line under the grid: "Delivered by our
team with our sister company DataSap Inc, which leads our AI and data
engineering practice."

1. **DataNexus** autonomous data onboarding platform. Azure OpenAI, TypeScript,
   Python, Next.js.
2. **Carrier Revenue Operations** billing intelligence. Azure, Apache Airflow,
   dbt, Power BI.
3. **FlowStack** zero touch validation and load engine. Snowflake, dbt,
   PostgreSQL, Python.

A fourth DataSap platform, **Meridian Hospitality Data Hub** (Azure, Snowflake,
dbt, Airflow, Power BI), is available and unused. It suits a case studies page.

**Do not import DataSap's testimonials.** That site's five client quotes use
randomuser.me stock portraits, so the people pictured are not real. The quotes
themselves are unverified.

Case studies 1, 2, 3, 5 and 6 below remain accurate descriptions of work that
exists. They are candidates for a case studies page, not for the home page.

## Original home page recommendation (superseded)

Pick the three with the strongest verifiable substance:

1. Education technology, learning operations platform (Case study 1)
2. Retail, point of sale and inventory system (Case study 2)
3. Education, campus mobile application (Case study 3)

Use sector and capability as the headline. Add a measured outcome only once
someone has measured it.

## Claims to remove before launch

| Claim | Where | Why |
| --- | --- | --- |
| "PKR 24.6M, up 31%" MRR | Home hero area | Company revenue shown as a product metric, unverifiable |
| "40+ outlets", "60% dispatch reduction" | Home Selected Work | No supporting project exists |
| "200+ Industry Specific Projects" | Industries page | No evidence at this scale |
| "80+ POS Systems Deployed" | Retail and POS page | One in house POS build exists |
| "100+ Websites Built" | Web Development page | No evidence at this scale |
| "60+ Transformation Projects" | Digital Transformation page | No evidence at this scale |
| "50+ Odoo projects delivered" | Odoo ERP page | No Odoo repository exists |
| "15+ industries served" | Multiple pages | No evidence at this scale |
| Named client testimonials | Multiple pages | The specs themselves say not to publish placeholder attributed quotes |

Replace with what is true and still persuasive: the technologies, the problem
classes solved, the products built, and the regions served.
