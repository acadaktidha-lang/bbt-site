# Spec audit — every page vs its layout/content doc

Date: 2026-08-05. Source of truth: 33 unique spec PDFs in `~/Downloads/ajjeeeebbbt/`
(41 files, but the `(1)`/`(2)` copies are byte-identical re-downloads — verified by md5).
Compared against `bigbinarytech-website-redesign/project/build/*.html`, which is what
gets pushed to WordPress.

Two build files have no spec and are excluded: `privacy-policy.html`, `terms-of-service.html`.
That leaves **22 pages audited**.

Decisions this audit was run under (confirmed 2026-08-05):

- Keep the built palette (navy `#0d1b2a` / amber `#f5a623`). Spec colours
  (`#1B3A6B`, teal, coral, `#F8F9FA`, `#D4EFE4`) are **not** being applied; colour
  differences are not listed as findings below.
- Build the missing pages the specs link to.
- Implement everything the specs ask for behaviourally (schema, sticky search,
  query-param CTAs, GA4 hooks, Fuse.js, helpfulness widgets, forms).
- **Spec figures are authoritative** where they conflict with the live site.

---

## Headline: structure is good, behaviour is missing

Section coverage is far better than expected. Fourteen pages carry every section
their spec defines, and most carry more. The service pages in particular
(`web-development`, `ai-automation`, `pos-systems`, `odoo-erp`,
`custom-software-development`, `social-media-marketing`) map 1:1 onto their specs and
then add sections beyond them.

What is missing is almost entirely **the behavioural layer** — the things the specs
ask for in their "Dev note" lines. Those are absent nearly site-wide.

### Site-wide gaps

| Gap | Coverage | Detail |
|---|---|---|
| **FAQPage schema** | **1 / 22 pages** | Only `about-us` has JSON-LD, added 2026-08-05. Sixteen pages carry FAQ accordions — roughly **150 Q&As total** — with zero rich-result eligibility. Multiple specs require this explicitly. Single biggest miss on the site. |
| **Query-param CTAs** | **0 / 22 pages** | Every spec'd link (`?service=pricing&offer=quote`, `?service=starter-package`, `?service=faqs`) collapses to a bare `/contact-us/`. No campaign or package attribution is possible. |
| **Forms** | **1 / 22 pages** | Only `contact-us` has one. Specs also call for: FAQ ask-a-question (7 fields), blog newsletter, e-book newsletter + download gates, careers application. |
| **GA4 event tracking** | **0 / 22 pages** | No accordion-open, filter-click, package-click, persona-select or form-submit events anywhere. |
| **BreadcrumbList schema** | **0 / 22 pages** | Breadcrumbs render as plain markup on every page that has them. |

### Missing pages the specs link to

`/outsourcing/` · `/resources/videos/` · `/resources/ebooks-guides/` — all return **404**
today. `/resources/case-studies/` and `/resources/blog/` **301** to flat URLs, so the
specs' nested `/resources/*` IA does not exist. Approved for building.

---

## Per-page findings

Pages are ordered worst-first by amount of work outstanding.

### home.html — weakest against spec

The spec (`BBT Home page - Content`) is a content doc, not a section layout, and the
build diverges from it more than any other page.

- **H1 differs.** Spec: *"Big Binary Tech - Custom Software Development Company"*.
  Build: *"Engineering software for businesses in Pakistan across…"*.
- **Missing H2: "Our Proven Software Development Process"** — the spec defines a
  **7-step** process (Discovery, Strategy, Design, Development, Testing, Launch, Growth).
  The build has a **3-step** section instead ("Discovery & architecture / Engineering &
  delivery / Go live & scale").
- **Missing H2: "More Than a Vendor — Your Technology Partner"** — the whole strategic
  partnership section is absent.
- **Missing H2: "We Understand Your Industry"** — absent.
- **Capability cards incomplete.** Spec names six: AI Automation & Integration, Odoo ERP
  Solutions, Custom Software Development, Website Development, POS Solutions, Digital
  Transformations. Build has four, and **Website Development and POS are missing** as cards.
- **Missing the four solution cards**: Enterprise & Odoo, Retail & POS, Digital
  Transformation, Business Process Solutions.
- 3 FAQs, no schema.

### resources.html

- **No newsletter section** (spec S12) — absent entirely.
- **FAQ section has no accordion.** The heading *"Looking for a straight answer?"* exists
  but contains **0 `<details>`** — the spec's S10 FAQ section is a heading with no questions.
- Video resources present as H3s but there is no `/resources/videos/` destination.
- 13 of 15 spec sections present otherwise.

### services.html

- **6 FAQs vs the spec's 10.** Missing: *Which Technology Service Should My Business Start
  With?*, *How Long Does an Odoo ERP Implementation or Custom Build Take?*, *Can Your
  Technology Services Integrate With Our Existing Systems?*, *What Makes Your POS Systems
  Better Than Generic Options?*, *Is Odoo ERP Good for Small Businesses and Startups?*,
  *What Is Digital Transformation and Does My Business Need It?*, *Do You Build E-Commerce
  Websites and Landing Pages?*, *Can You Handle Social Media Marketing in Arabic and
  English?*, *What Industries Do Your Technology Services Cover?*, *How Do I Get Started?*
- **No Digital Transformation service block.** The spec lists it as a service; the build's
  six blocks stop at Social Media Marketing.
- **Sticky scroll-anchor nav (spec S02) is absent.**
- Service block heading differs: spec *"Point of Sale (POS) Systems"* vs build
  *"POS Systems Development"*.

### faqs.html

Content is strong — 32 questions across 8 categories, matching the spec's count exactly,
plus the three named testimonials. Structure drifts:

- **H1 truncated** to *"Frequently Asked Questions"*. Spec wants the full
  *"Frequently Asked Questions — Everything You Need to Know About Working With Big Binary Tech"*.
- **No FAQPage schema** — 32 Q&As, zero rich-result eligibility. Worst instance on the site.
- **Search bar is not sticky** (spec S02: sticky, `z-index: 1000`, shadow when stuck).
- **No fuzzy search** (spec asks for Fuse.js) and no no-results fallback CTA.
- **Section 05 has no form.** The *"Didn't Find Your Answer?"* heading is there; the
  7-field form (name, email, company, location, topic, question, consent) is not.
- **Section order differs in three places.** Spec: persona → categories → still-stuck →
  most-asked → … → bottom CTA → related. Build: categories → persona → most-asked →
  still-stuck → … → related → bottom CTA.
- **Stats bar has 4 of 6 items** — missing *4-Hour Average Response Time* and *Updated Monthly*.
- **Resources shows 3 cards, spec wants 4** — no Video Tutorials card.
- **Related pages shows Industries where the spec wants Outsourcing.**
- No "Was this helpful?" widgets, no GA4 tracking.

### blogs.html

- **0 FAQs and no schema.**
- All 11 spec sections present; newsletter section is built.

### pricing.html — closest to spec on the site

All 11 sections in spec order. Every price exact: $3,500 / $15,000 / $2,500 / $12,000 /
$25,000 / $50,000 / $35,000 / $20,000. All three MOST POPULAR badges on the correct cards.
Comparison table complete at 10 rows. 8 pricing FAQs as specified.

- **No FAQPage schema** (spec S09 requires it).
- **All CTAs lose their `?service=` params** — 10 distinct spec'd URLs collapse to `/contact-us/`.
- **No Outsourcing card** in Related Pages (S11) — target 404s.
- **No pricing-guide lead magnet** (S10 secondary CTA).

### contact-us.html

- **Missing H2: "What Happens After You Reach Out"**.
- **Missing H2: "Trusted By and Registered With"** — though a credentials section exists
  under a different heading.
- Missing service H3s in the "What We Can Help With" block: *Cloud Migration & DevOps*,
  *Legacy System Modernization*.
- Form exists (1 of 1 site-wide) but note: **CF7's REST API returns 200 without saving**,
  so any field changes must be made by hand in WP admin.

### industries.html

All 10 verticals built. Gaps are in the supporting sections:

- Missing **"Global Success Stories"** and **"Proven Results Across Verticals"** sections.
- Missing FAQ: *Do you offer Arabic support for GCC industries?*
- Vertical detail headings differ from spec (build uses plain names like *"Manufacturing"*;
  spec uses *"Smart Factory ERP and Production Control"*). Content is present under the
  simpler heading — this is a naming difference, not missing content.

### solutions.html

- **6 FAQs; all 7 spec questions differ** from what is built.
- Industry sections exist but as a combined grid rather than the spec's individual
  *"Manufacturing Solutions"* / *"Healthcare Solutions"* H3 blocks. Content present,
  structure flattened.

### Pages that match their spec structurally

These carry every section their spec defines. Their only outstanding items are the
site-wide ones (schema, query params, GA4):

| Page | Spec sections | Built | Note |
|---|---|---|---|
| `about-us.html` | 9 | 10 | Fixed 2026-08-05; has schema. Leadership bios and testimonial attribution still pending client content. |
| `web-development.html` | 13 | 18 | 10 FAQs |
| `ai-automation.html` | 12 | 18 | 15 FAQs |
| `custom-software-development.html` | 9 | 14 | 9 FAQs |
| `odoo-erp.html` | 6 | 15 | 10 FAQs |
| `pos-systems.html` | 9 | 11 | 8 FAQs |
| `social-media-marketing.html` | 12 | 15 | 8 FAQs |
| `retail-pos.html` | 9+ | 16 | 8 FAQs |
| `digital-transformation.html` | 13 | 16 | 8 FAQs |
| `enterprise-odoo.html` | — | 13 | 7 FAQs |
| `case-studies.html` | 10 | 10 | 6 FAQs |
| `careers.html` | 13 | 13 | 8 FAQs |
| `e-books-guides.html` | 12 | 12 | 10 FAQs |

---

## Content truth conflicts

Spec figures were confirmed authoritative, so these need changing **on the live site**:

| Claim | Spec says | Site currently says | Where the site value appears |
|---|---|---|---|
| Projects delivered | **60+** | 200+ | home hero, about hero stat strip |
| Successful launches | (not stated) | 100+ | about Global Reach |
| Countries served | 50+ | 50+ | ✅ agrees |
| Expert developers | (not stated) | 20+ | about Global Reach |
| Client retention | (not stated) | 95% | about Global Reach |

Note the direction of this change: it revises the headline project count **down** from 200+
to 60+ across the site. Worth a sanity check before it ships, since it makes the company
look smaller. The other three figures have no spec counterpart, so they stay as they are
unless told otherwise.

Named testimonials confirmed as usable: **Omar Hassan** (Gulf Manufacturing Co., Doha),
**Layla Al-Farsi** (Saudi Retail Group, Riyadh), **James Whitfield** (FinTech Solutions Ltd,
London) — already built on `faqs.html`.

---

## Recommended order of work

1. **FAQPage schema across all 16 FAQ-bearing pages.** Cheapest change with the largest
   return — ~150 questions currently invisible to rich results.
2. **`?service=` query params on every CTA.** Mechanical, site-wide, unlocks attribution.
3. **home.html rebuild** — the 7-step process, partnership section, industry section,
   and the missing capability and solution cards.
4. **faqs.html** — H1, sticky search, section reorder, stats bar, ask-a-question form.
5. **services.html** — 4 missing FAQs, Digital Transformation block, sticky anchor nav.
6. **resources.html** — newsletter section, populate the empty FAQ accordion.
7. **Missing pages** — `/outsourcing/`, `/resources/videos/`, `/resources/ebooks-guides/`.
8. **Metric alignment** to spec figures (pending the sanity check above).
9. **GA4 hooks, Fuse.js search, helpfulness widgets** — the remaining behavioural layer.
