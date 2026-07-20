# Big Binary Tech — Front‑End & Brand Design System Handoff

> **For:** the incoming design / animation / front‑end team.
> **Purpose:** everything you need to (a) understand what this brand is and how it presents itself, (b) reproduce its design language exactly on new pages/products, and (c) know precisely what is solid and what must be fixed before/while revamping.
> **Scope:** front‑end only — HTML / CSS / motion. No back‑end.
> **Compiled:** 2026‑07‑19, from the live site (`bigbinarytech.com`) + the source of truth (`project/*.html`).

---

## 0. Read this first — the single most important constraint

This is **not** a normal component-based front-end. There is **no build step, no shared stylesheet, no framework**.

- The site is authored as **21 standalone `.html` files** in [`project/`](project/). Each file is **fully self-contained** — its own inline `<style>` and inline `<script>`. The design system is **copy‑pasted into every page**, not imported.
- Each page's HTML is **pasted into a WordPress page** as a single **Elementor "HTML" widget** (theme: *Hello Elementor* + *Elementor* + *ElementsKit‑lite*). The live WP page must render **pixel‑identical** to the local file.
- **The golden rule the current build follows:** *don't change content or layout — only responsiveness and cross‑page consistency.* If you revamp, that rule is now yours to re-negotiate with the client, but understand it's why the code looks duplicated.

**What this means for you practically:**
1. If you build a new page/product in this brand, you must **inline the full design system** into it (or the team must finally extract a shared stylesheet — see §12).
2. Two WordPress "gotchas" will bite you if you don't know them:
   - **Elementor confinement:** Elementor wraps the widget in `.elementor-section > .elementor-container{max-width:1140px}`, which crushes full‑bleed sections into a centered 1140px box. **Fix (site‑wide):** `.elementor-section > .elementor-container{max-width:100%!important;}` in *Appearance → Customize → Additional CSS*.
   - **Render cache:** editing a page's Elementor data via REST does **not** update the rendered output until you flush Elementor's cache (`DELETE /wp-json/elementor/v1/cache`). If a change "didn't take," this is usually why.
3. Class names **collide with the Hello Elementor theme** (e.g. `.site-footer`). The theme's CSS can win on specificity and silently box your layout. Namespace new classes (`bbt-` prefix is already the convention for CSS variables — extend it to classes).

**Live URL map** (slugs differ from filenames): `bigbinarytech.com` (home), `/about-us-2/`, `/contact-us/`, `/services/`, `/solutions-3/`, `/industries/`, `/digital-transformation-3/`, `/retail-pos-2/`, `/resources-2/`, `/enterprise-odoo/`, `/social-media-marketing/`, `/e-books/`, `/case-studies/`, `/blogs/`, `/pricing/`, `/careers/`.

---

## 1. What this brand is and the story it tells

**Big Binary Tech (BBT)** positions itself as a **global software, ERP, AI‑automation and digital‑transformation agency** — a "technology partner, not a vendor."

- **One‑line pitch (from About):** *"Big Binary Tech is a global Odoo ERP, AI automation, and custom software development company. We turn complex technology into simple, working solutions."*
- **Homepage promise:** *"Global Tech Solutions Built for Impact."* / *"We engineer future‑ready ecosystems… We don't just build apps; we solve bottlenecks."*
- **Target markets:** explicitly **GCC (Dubai/UAE + Saudi Arabia), USA (New York), UK (London)**, operated from a **Pakistan HQ (Lahore)**. Arabic‑ready / VAT‑ / ZATCA‑compliant messaging recurs on service pages — the GCC is the primary go‑to‑market.
- **Credibility hooks:** "PSEB‑registered, Government‑backed," partner/certification logos (Cisco, PSEB, NAVTTC, Odoo, AWS, Microsoft, etc.), stat counters (50+ projects, 98% retention, 15+ industries…).

**The three pillars it sells:** (1) **AI Automation & Integration**, (2) **Odoo / Enterprise ERP**, (3) **Custom Software** — plus Web Development, POS Systems, Social Media Marketing, and Digital Transformation.

### The narrative logic every page follows

The service/solution pages are built on one repeatable **conversion narrative**. When you design new pages, keep this spine:

> **Hero** (headline + geo + 3 proof bullets + CTA) → **"Does this sound familiar?"** (pain/problem cards) → **capability blocks** (what we do, anchor‑linked) → **GCC / localization spotlight** → **"How we build it, start to finish"** (process timeline) → **results / stat band** → **testimonials** → **FAQ accordion** (+ schema) → **final CTA** → **related pages**.

The homepage variant is: hero → *How we work* → *More than a vendor* → *Core capabilities* → *We understand your industry* → *Solutions* → *Recognized by industry leaders* → *Tech stack* → *Why choose us* → *Insights & resources* → *FAQ*.

---

## 2. Color palette

The canonical brand is **navy + amber‑orange on white**. (Historical note: the original spec PDFs used blue/teal/coral — that palette is **dead**; the client chose navy/orange. A few unused `--bbt-teal` / `--bbt-coral` variables still linger in some files and should be deleted.)

### Core tokens (from the `:root` in `index.html` — the reference file)

| Token | Value | RGB | Role |
|---|---|---|---|
| `--bbt-navy` | `#0f172a` | 15, 23, 42 | Primary dark — hero bg, footer, headings, dark sections |
| `--bbt-orange` | `#f59e0b` | 245, 158, 11 | **Brand accent** — CTAs, highlights, eyebrows, icons |
| `--bbt-orange-600` | `#d97706` | 217, 119, 6 | Orange hover / pressed |
| `--bbt-orange-glow` | `rgba(245,158,11,.25)` | — | Orange shadow/glow under buttons & cards |
| `--bbt-ring` | `rgba(245,158,11,.4)` | — | Focus ring / active outline |
| `--bbt-heading` | `#0f172a` | 15, 23, 42 | Heading text |
| `--bbt-text` | `#334155` | 51, 65, 85 | Body text (slate‑700) |
| `--bbt-muted` | `#64748b` | 100, 116, 139 | Secondary/muted text (slate‑500) |
| `--bbt-bg` / `--bbt-white` | `#ffffff` | 255,255,255 | Page background / surfaces |
| `--bbt-bg-soft` | `#f8fafc` | 248, 250, 252 | Alternating section background |
| `--bbt-surface` | `#f1f5f9` | 241, 245, 249 | Card / chip surface |
| `--bbt-surface-hover` | `#e2e8f0` | 226, 232, 240 | Surface hover |
| `--bbt-border` | `rgba(15,23,42,.1)` | — | Hairline borders |
| `--bbt-glass` | `rgba(255,255,255,.85)` | — | Frosted/glass overlays |

**Dead tokens to remove:** `--bbt-teal:#2A9D8F`, `--bbt-teal-light:#D4EFE4`, `--bbt-coral:#E76F51`. Some service pages *alias* them to the real brand (`--bbt-coral:#f59e0b`, `--bbt-teal:#0f172a`) so classes like `.btn-coral` still render orange — but this is confusing debt; rename `.btn-coral` → `.btn-primary` in a cleanup.

**⚠️ Token drift to normalize:** body text is `#334155` in most pages but `#1e293b` (slate‑800) in a few; `--bbt-border` appears as `rgba(15,23,42,.1)`, `.08`, and `rgba(0,0,0,.08)`. Pick one value each and standardize — this is exactly the kind of inconsistency a shared stylesheet would kill.

### Palette usage rules (how the color system reads on screen)
- **60/30/10:** ~60% white/`#f8fafc`, ~30% navy (heroes/footer/dark bands), **10% orange** used *only* for accents (never large orange fills).
- Sections **alternate white ↔ `#f8fafc`** for rhythm.
- Dark sections are navy `#0f172a` (sometimes a `linear-gradient(135deg,#0f172a,#1e293b)`), with orange as the single highlight and white/`rgba(255,255,255,.7)` text.
- **Contrast caution:** `--bbt-muted #64748b` on white passes AA for normal text but is borderline for small text; orange `#f59e0b` text on white is **~2.1:1 — fails AA for body text** (fine for large headings/graphical accents only). Never set body copy in orange.

---

## 3. Typography

**One family: `Inter`.** No secondary/display face. This is a deliberate, clean, engineering-brand choice — keep it.

| Property | Value |
|---|---|
| Family | `"Inter", system-ui, -apple-system, sans-serif` (var `--font-sans`) |
| Weights loaded | **300, 400, 500, 600, 700, 800, 900** |
| Body | 16px / **line-height 1.6** / color `#334155` |
| H1 (desktop hero) | **64px / weight 900** / white on dark hero |
| Headings | weight 800–900, color `#0f172a` |
| Eyebrow / kicker | 12–13px, weight 600–700, **UPPERCASE**, `letter-spacing: .14em`, color orange |
| Accent word | one word per headline wrapped in `.highlight` → orange (or an orange gradient wipe) |

**How it's loaded today:** Google Fonts via `@import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap")` **and/or** a `<link>` (with `preconnect`). It's declared inconsistently across pages (some `@import`, some `<link>`, some request only `400–800`). **Standardize on one method** (a `<link rel=preconnect>` + one `<link>` with the full weight range) and add `font-display:swap` (already present via `&display=swap`).

**Noise to remove:** the live site also pulls **Google `Roboto`** — that's injected by the Elementor/ElementsKit theme, *not* the brand. It's unused weight and a wasted request. Dequeue it (or ignore Elementor's default typography by scoping everything under a `bbt-` container).

**Recommendation for new products:** self‑host Inter (woff2, subset to Latin) to kill the render‑blocking third‑party font request and CLS; keep the same weight ladder.

---

## 4. Design language / visual system

The look is **"modern enterprise‑tech"**: dark technical heroes, generous whitespace, soft rounded cards, one confident orange accent, and lots of motion.

### 4.1 Layout & spacing
- **Full‑bleed sections**, content constrained to ~**1140–1280px** centered containers.
- **Vertical rhythm:** `--section-pad: 96px` (sections are `96px 0`; mobile hero uses `padding:96px 26px 40px`).
- **Grid:** flexbox/CSS‑grid card rows, typically 3‑ or 4‑up, collapsing to 1‑up on mobile.

### 4.2 Radii (the corner language)
Rounded and friendly. The scale actually in use (by frequency): **`999px` (pills/buttons/chips)** · **`24px` (default card)** · `12px` (small cards/inputs) · `20px` · `28px` · `50%` (avatars, icon chips) · `40px 40px 0 0` / `60px 60px 0 0` (top‑rounded panels). **Standardize to a 4‑step scale:** `12 / 24 / 999px` + `50%`.

### 4.3 Elevation / shadows
| Use | Value |
|---|---|
| Primary button | `0 8px 32px rgba(245,158,11,.25)` (orange glow) |
| Card (light) | `0 30px 60px rgba(15,23,42,.08)` |
| Card (subtle) | `0 8px 32px rgba(0,0,0,.05)` |
| Floating card on dark hero | `0 50px 100px rgba(0,0,0,.5)` |
| Mobile nav drawer | `-8px 0 40px rgba(0,0,0,.45)` |

Shadows are **large, soft, low‑opacity** — never hard/short. Orange‑glow shadow is reserved for the primary CTA.

### 4.4 Buttons
```css
.btn        { display:inline-flex; align-items:center; gap:8px;
              padding:14px 28px; border-radius:999px; font-weight:600; }
.btn-primary{ background:#f59e0b; color:#fff; border:none;
              box-shadow:0 8px 32px rgba(245,158,11,.25); }   /* hover → #d97706, lifts + glows */
.btn-outline{ background:#fff; color:#0f172a; border:1px solid rgba(15,23,42,.1); } /* hover → navy fill, white text */
```
Pill shape, arrow icon (`→`) common on primary CTAs, subtle `translateY(-2/3px)` lift on hover.

### 4.5 Heroes (two variants, both valid)
1. **Illustrated tech hero** (homepage): navy background with a detailed **isometric circuit‑board / server illustration**, eyebrow pill, big 900‑weight headline with an orange‑highlighted/animated word, 3 proof bullets with icon chips, primary CTA.
2. **Photographic hero** (About, service pages): full‑bleed photo with a **navy `linear-gradient(135deg, rgba(11,21,40,.85)…)` overlay**, orange eyebrow, white headline with one orange word, sub‑paragraph, scroll‑down chevron.

### 4.6 Iconography & imagery
- **Icons:** Font Awesome 6 (line/solid), usually inside a rounded orange‑tinted chip.
- **Imagery:** isometric tech illustrations + stock photography (industry/office). Partner logos are currently **text tiles**, not real logos (see §11).

### 4.7 Signature components
Mega‑menu header · right‑side mobile drawer · **FAQ accordion** (on nearly every page) · **testimonial carousel** · **animated stat counters** (`data-target`) · **pricing/comparison tables** · **filter pills / tabs** · industry cards · **colorful gradient "tech‑stack" cards** (Frontend / Backend / Database / ERP&CRM / Cloud&DevOps — the one place bright per‑domain gradients are allowed) · lead‑gen / newsletter forms.

---

## 5. Motion & animation system

Motion is central to the brand and is where the "premium" feel comes from — but it's also where most of the technical risk lives.

**Libraries (all from CDN):** GSAP `3.12.5` + **ScrollTrigger** (every page) + **TextPlugin** (homepage hero) + **Lenis `1.1.13`** smooth‑scroll (6 pages).

**Animation vocabulary in use:**
- **`fade-up` / reveal on scroll** — the default entrance for almost every block (ScrollTrigger, `gsap.from` opacity+`y`, staggered).
- **Cycling hero headline** — the homepage H1 rotates through *"AI & Cloud Automation" → "Software Development" → "Digital Transformation"* via TextPlugin, with a grey→orange gradient wipe.
- **Animated counters** — stats count up when scrolled into view.
- **Pinned + scrubbed sections** — *"How we work"* and the *tech‑stack* section use `pin:true` + `scrub` (the section sticks and content is driven by scroll position). This is scroll‑jacking.
- **Scroll‑progress bar** at the top.
- Testimonial / industry **carousels**.

**⚠️ Motion risks you must design around (important):**
1. **Content is invisible until scrolled into view** (`opacity:0` start state). If JS fails, is slow, or a crawler/no‑JS user arrives, large sections render **blank**. (This is literally why a full‑page screenshot of the homepage shows empty "How we work" / "Core Capabilities" bands.) Provide a **no‑JS / reduced‑motion fallback** that leaves content visible.
2. **Pinned/scrubbed sections intercept programmatic scrolling** — anchor jumps and `scrollIntoView` land in the wrong place while a pin is active. Test all in‑page anchor links.
3. **CLS on ScrollTrigger refresh** — the pinned section's spacer is re‑measured on image load / resize, causing layout shift (the code has hand‑rolled `ScrollTrigger.refresh()` guards to fight this). Fragile.
4. **No global `prefers-reduced-motion` support** — must be added.

**For new products:** define a small motion spec — standard **easings** (e.g. `power2.out`), **durations** (150–600ms), a single **`data-animate` reveal utility**, and a mandatory `@media (prefers-reduced-motion: reduce){ * { animation:none!important; transition:none!important } }` block. Prefer lightweight reveals over pinned scroll‑jacking on conversion pages.

---

## 6. Page inventory (all 21 pages)

Shared header nav on **every** page: **Home · About · Services▾ · Solutions▾ · Industries · Resources▾ · Contact** + a `Get Free Consultation` pill.
- **Services▾:** AI & Automation, Custom Software Development, Web Development, Odoo & ERP Services, Social Media Marketing, POS Systems.
- **Solutions▾:** Enterprise Odoo ERP, Digital Transformation, Retail & POS Solutions.
- **Resources▾:** Case Studies, Blogs, E‑Books.
- **Footer nav (different, shorter):** About · Services · **Pricing · Careers** · Contact.

| Page | H1 | Purpose | Notable components |
|---|---|---|---|
| **index** | *Global Tech Solutions Built for Impact.* | Brand homepage (5,471 lines — the richest) | cycling hero, pinned "How we work", tech‑stack grid, FAQ, JSON‑LD |
| **about** | *Where Odoo ERP, AI Automation, and Custom Software Meet* | Story, values, team, credibility | stat counters, "Binary Evolution" timeline, team grid, FAQ, JSON‑LD |
| **services** | *Services That Build, Automate & Scale Your Business* | Services hub | service cards, tab pills, process, FAQ |
| **solutions** | *Solutions That Power Modern Business Operations* | Solutions hub (challenge + industry) | ecosystem grid, delivery framework, FAQ |
| **pricing** | *Transparent Pricing for AI, Odoo, Cloud & Digital Transformation* | Pricing tables | multiple package tables, comparison table, FAQ — *footer‑only in nav* |
| **industries** | *Industry‑Specific Technology Solutions for GCC, USA & UK* | 10 verticals (2,788 lines) | anchor jump‑links, counters, per‑vertical blocks, FAQ |
| **case-studies** | *Real Results, Real Businesses, Real Growth.* | Portfolio | filter pills, case cards, stat boxes, process, FAQ |
| **blogs** | *Blog: AI, Odoo, Cloud & Digital Transformation Insights* | Blog index | topic pills, newsletter form, video area — **no FAQ**; *Resources‑dropdown only* |
| **careers** | *Build Your Career With Us in AI, Cloud, Odoo & Beyond* | Hiring | job cards, CV form, hiring timeline, FAQ — *footer‑only* |
| **contact** | *Let's Build Something Great Together.* | Lead capture | contact form, office cards, FAQ — **map placeholders** |
| **custom-software-development** | *Software Built Around Your Workflows…* | Service | capability cards, process, FAQ |
| **web-development** | *Websites Built to Load Fast and Turn Visitors Into Customers* | Service | 7 service blocks, GCC/RTL spotlight, FAQ |
| **ai-automation** | *AI Automation & Integration Services That Save Hours Every Day* | Service | problem cards, GCC/WhatsApp spotlight, FAQ |
| **odoo-erp** | *Odoo Implementation, Customisation & Support* | Service | capability grid, phased rollout, FAQ (smallest, 1,249 lines) |
| **enterprise-odoo** | *Enterprise & Odoo ERP Solutions Built for Operational Excellence* | Solution | pillars, journey timeline, integrations, FAQ |
| **digital-transformation** | *Digital Transformation Solutions for GCC, USA & UK* | Solution | capability blocks, packages, FAQ |
| **retail-pos** | *Retail & POS Solutions for GCC, USA & UK* | Solution | problem cards, integrations, packages, FAQ |
| **pos-systems-development** | *Custom POS Systems* | Service | problem cards, POS‑type blocks, FAQ (thinnest hero) |
| **social-media-marketing** | *Social Media Marketing That Turns Followers Into Customers* | Service | analytics tiles, packages, FAQ |
| **e-books** | *Free E‑Books, Playbooks & Guides…* | Lead magnets | topic grid, preview accordion, 2 forms, FAQ — *Resources‑dropdown only* |
| **resources** | *Resources, Insights & Guides…* | Aggregated hub | featured card, carousels, video, newsletter, FAQ |

---

## 7. What's working well (keep these)

1. **A real, coherent design language.** Navy + orange + Inter + soft rounded cards is consistent across 21 pages and reads as a single confident brand. This is the biggest asset — protect it.
2. **Strong conversion architecture.** The hero → problem → capability → process → proof → FAQ → CTA spine is disciplined and repeatable. Copywriting is benefit‑led and specific ("save hours every day," "cut reporting from 3 days to 15 minutes").
3. **Mobile responsiveness is genuinely good.** No horizontal overflow at 390px; the mobile nav drawer works cleanly (navy panel, active‑state orange underline, proper close). The responsive work is the most mature part of the build.
4. **Clean, current housekeeping.** Copyright is `© 2026` everywhere; **no dead `href="#"` links, no "Lorem", no "TODO/coming soon"** anywhere in the source (older typo‑link bugs like `enterprise-odo.html` are fixed).
5. **Lean plugin footprint & fast base load.** Only Hello Elementor + Elementor + ElementsKit‑lite; homepage DOMContentLoaded ≈ **0.8s**. Good starting point.
6. **Decent baseline SEO/a11y hygiene.** Title (62 chars) and meta description (142 chars) are well‑formed; `lang="en-US"`; **all 24 homepage images have alt text**; JSON‑LD present on home/about.
7. **Motion adds real polish** — the cycling hero, counters, and reveals feel premium when they work.

---

## 8. What needs fixing (prioritized for a revamp)

### P0 — correctness / credibility (fix before any relaunch)
- **Live is a snapshot behind source.** The live homepage hero still reads **"We engineering future‑ready ecosystems"** (grammatical error) while the local source is correctly **"We engineer."** Because pages are hand‑pushed to WP, live and source drift. **Re‑publish all pages and establish a sync process.** (When a client reports "old/missing content," suspect stale WP *first*, not the source file.)
- **Phone number mismatch.** A **US number (+1 307‑443‑4365, Wyoming)** is presented as the contact for the **Pakistan HQ** *and* the **Dubai office** *and* in every footer. Fix with real per‑office numbers.
- **`og:image` points to a dead domain** (`https://bbt.mza-t.com/…`). Social shares will show a broken image. Move to `bigbinarytech.com`.
- **Contact page maps are literal placeholders** ("Lahore, Pakistan: Map placeholder" / "Dubai, UAE: Map placeholder"). Embed real maps.
- **E‑books "DOWNLOAD FREE PDF" buttons have no file targets** — downloads are non‑functional. Wire real assets or remove the promise.

### P1 — SEO / structure / IA
- **4× `<h1>` on the homepage.** The cycling hero renders each rotating phrase as its own `<h1>`. There must be **exactly one** `<h1>`; make the rotating words `<span>`/`<div>`.
- **Invalid HTML on `resources.html`** — nav anchor has a **duplicate `class` attribute** (`class="…" class="active"`). Merge them.
- **Navigation IA is incomplete.** Careers, Pricing, Blogs, E‑Books, and Case Studies are **not in the main nav** (footer‑ or dropdown‑only). Decide a coherent IA so every page is reachable from the header.
- **JSON‑LD only on home/about.** Add `Service`/`FAQPage`/`BreadcrumbList` schema to the service, solution, and FAQ‑bearing pages.

### P2 — content architecture / duplication
- **Near‑duplicate page pairs** dilute SEO and confuse users: `odoo-erp` ↔ `enterprise-odoo`, `pos-systems-development` ↔ `retail-pos`, and `resources` overlaps `blogs`/`e-books`/`case-studies`. Define canonical vs supporting pages (or merge).
- **Careers page mixes audiences** — it injects client‑sales sections ("Digital Marketing Packages: For Businesses, Not Candidates," "Need a Dedicated Team?") into a candidate‑facing page. Split the audiences.
- **Thin pages** (`odoo-erp`, `enterprise-odoo`, `pos-systems-development` "Custom POS Systems" hero) need depth to match the rest.

### P3 — performance & front‑end quality
- **Images: 20 PNGs on the homepage, `loading="lazy"` on 0 of them.** Convert to **WebP/AVIF** (the `assests/` folder already has `.avif`/`.webp` versions available) and add lazy‑loading + explicit `width`/`height` (helps CLS).
- **Render‑blocking third‑party fonts** (Google Inter *and* a stray theme‑injected **Roboto**). Self‑host Inter; dequeue Roboto.
- **GSAP + Lenis loaded from CDN on every page** — consider bundling/deferring; ensure they don't block first paint.
- **Design‑token drift** (`#334155` vs `#1e293b`; three `--bbt-border` values; dead teal/coral vars; `.btn-coral` aliasing orange). Normalize.
- **No `prefers-reduced-motion` support**, and heavy scroll‑jacking harms accessibility. Add reduced‑motion fallbacks and ensure keyboard/screen‑reader users aren't trapped by pinned sections.
- **26+ copies of the same CSS** (one per page). The single highest‑leverage refactor is to **extract a shared `bbt-design-system.css`** (see §12).

---

## 9. Content upgrades still outstanding (placeholders to replace)

These are on‑brand placeholders currently standing in for real assets/content:

- **Testimonials** — generic quotes + initials‑avatar tiles; **no real client names/logos/photos** anywhere.
- **Partner / certification logos** — rendered as **styled text tiles** (Cisco, PSEB, NAVTTC, FAST, UMT, Odoo, AWS, Microsoft…), not real logo images.
- **Stat figures** — aspirational/demo numbers (25,000+ readers, 5,000+ downloads, 12M+ users, 50+ projects, 98% retention; social tiles 45,200 reach / 127 leads / $12 CPL). Confirm real values.
- **Forms are front‑end only** — contact, newsletter, gated downloads, and the careers CV form **don't submit anywhere** (no CRM/ATS/GA4/Mailchimp wiring).
- **Hero visuals** — specs called for bespoke animated SVGs; pages reuse stock images / CSS treatments.
- **Per‑page:** contact office maps + real phone/emails; case‑studies (3 stock images cycled for 8 cards, individual case pages don't exist); blogs/e‑books/resources (Unsplash stock, no real posts, non‑functional video play buttons); industries (no per‑industry landing pages); careers (placeholder roles/salaries, stubbed apply flow).

Full list lives in [`CONTENT_TODO.md`](CONTENT_TODO.md).

---

## 10. How to design NEW products in this brand (the practical kit)

**Do:**
- Use the token block below verbatim. Keep **Inter** (300–900), navy `#0f172a`, orange `#f59e0b`.
- Follow **60/30/10** color, alternate white ↔ `#f8fafc` sections, `96px` vertical padding, ~1140–1280px content width.
- Pill buttons (`999px`), 24px cards, large soft shadows, orange‑glow only on the primary CTA.
- Orange as **accent only** — one highlighted word per headline, icons, CTAs. Never orange body text.
- Reuse the conversion spine (hero → problem → capability → process → proof → FAQ → CTA).
- Add `prefers-reduced-motion` fallbacks and keep content visible without JS.

**Don't:**
- Don't reintroduce blue/teal/coral (dead palette).
- Don't set more than one `<h1>`.
- Don't rely on scroll‑jacking for essential content.
- Don't collide class names with Hello Elementor (`.site-footer`, `.site-header`, etc.) — prefix `bbt-`.
- Don't forget the Elementor full‑bleed fix and cache flush when publishing.

**Drop‑in design‑system starter (`:root`):**
```css
:root{
  /* brand */
  --bbt-navy:#0f172a; --bbt-orange:#f59e0b; --bbt-orange-600:#d97706;
  --bbt-orange-glow:rgba(245,158,11,.25); --bbt-ring:rgba(245,158,11,.4);
  /* neutrals */
  --bbt-bg:#ffffff; --bbt-bg-soft:#f8fafc; --bbt-surface:#f1f5f9; --bbt-surface-hover:#e2e8f0;
  --bbt-heading:#0f172a; --bbt-text:#334155; --bbt-muted:#64748b; --bbt-border:rgba(15,23,42,.1);
  /* system */
  --font-sans:"Inter",system-ui,-apple-system,sans-serif;
  --section-pad:96px; --radius-sm:12px; --radius-md:24px; --radius-pill:999px;
  --shadow-btn:0 8px 32px var(--bbt-orange-glow);
  --shadow-card:0 30px 60px rgba(15,23,42,.08);
}
@media (prefers-reduced-motion:reduce){*{animation:none!important;transition:none!important}}
```

**The #1 refactor recommendation:** stop copy‑pasting CSS into 21 files. Extract one **`bbt-design-system.css`** (tokens + buttons + cards + typography + FAQ/accordion + nav/footer) and enqueue it once in WordPress (child‑theme `functions.php`). Each page then ships only its page‑specific styles. This alone removes the drift documented in §2/§8 and makes every future product automatically on‑brand.

---

## Appendix — technical snapshot (live, 2026‑07‑19)

- **Stack:** WordPress · Hello Elementor `3.4.9` · Elementor `4.1.4` · ElementsKit‑lite `3.9.9` · jQuery · GSAP `3.12.5` (gsap + ScrollTrigger + TextPlugin) · Lenis `1.1.13`.
- **Homepage:** 61 requests · DOMContentLoaded ≈ 800ms · load ≈ 857ms · 24 images (20 PNG / 2 AVIF / 2 JPG) · 0 lazy‑loaded.
- **SEO:** title 62 chars ✓ · meta description 142 chars ✓ · canonical ✓ · `lang=en-US` ✓ · **4 `<h1>` ✗** · `og:image` on dead `bbt.mza-t.com` ✗ · JSON‑LD present (home/about).
- **Source of truth:** [`project/*.html`](project/) (21 files, self‑contained). Specs: 15 layout PDFs in repo root (content/structure reference — **ignore their blue/teal/coral palette**).
- **Reference screenshots captured for this report:** `live-home-desktop.jpeg`, `live-about-hero.jpeg`, `live-techstack.jpeg`, `live-home-mobile-hero.jpeg`, `live-mobile-nav.jpeg`, `live-howwework.jpeg` (repo root).
