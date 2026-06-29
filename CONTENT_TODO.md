# Content TODO — placeholders to replace with real content

All pages were reconciled to their layout specs. Where the spec required assets/content
that don't exist yet, tasteful on-brand placeholders were used. Replace these with real
content before launch.

## Site-wide (affects every page)
- **Footer contact block** — address "123 Tech Park, Suite 400, City, Country", phone `0326 8880101`, email `info@bigbinarytech.com`, and social links (point to platform homepages) are placeholders from the footer spec.
- **Testimonials** — every testimonial across the site uses generic quotes + initials-avatar tiles (no real client names, companies, logos, or head-shots).
- **Partner / certification logos** — rendered as styled text tiles (Cisco, PSEB, NAVTTC, FAST, UMT, Hack The Box, Odoo, AWS, Microsoft, etc.), not real logo images.
- **Stat figures** (e.g. 50+ Projects, 98% Retention, 15+ Industries) — taken from the specs; confirm real numbers.
- **Forms are front-end only** — contact, newsletter, gated downloads, and the careers CV form have no backend (no Mailchimp/HubSpot/ATS/GA4 wiring); they don't submit anywhere yet.
- **Hero animated SVGs** — specs asked for bespoke animated SVG visuals; pages reuse existing `bbt.mza-t.com` images or CSS treatments instead.

## Per page
- **contact** — Office addresses (Lahore/Dubai), UAE phone `+971 4 000 0000`, Google Map embeds (navy placeholder blocks), WhatsApp/Calendly links, emails `hello@`/`gcc@`.
- **case-studies** — 8 case-card images (reused 3 stock images cycled); "Read Full Case Study" links → contact.html (individual case pages don't exist); industry-breakdown counts are sample figures; "Download Our Portfolio" → resources.html.
- **blogs** — Latest/Popular post images (Unsplash stock); author bylines (initials avatars, no head-shots); article/category permalinks → existing pages (real `/resources/blog/...` posts don't exist).
- **e-books** — 3D book-cover mockups (CSS placeholders); guide card thumbnails (stock); preview-page tiles (text, not real screenshots); gated downloads not wired.
- **resources** — Featured case-study image + 3D book mockups; blog/transformation-story cards link to hub pages (not individual posts); video thumbnails (gradient, non-functional play buttons, no embeds).
- **industries** — Per-industry detail blocks have no imagery; detail-CTAs point to nearest solution page (no dedicated per-industry landing pages); trust-bar stats.
- **enterprise-odoo** — Hero dashboard visual (CSS mockup); integration logos (text tiles); "Download Odoo Implementation Guide" → e-books.html.
- **retail-pos / digital-transformation / social-media-marketing** — Service-block visuals reuse existing CDN images (spec wanted before/after SVG diagrams); GCC/dashboard graphics are styled tiles; guide/buyer's-guide download CTAs → e-books.html/resources.html.
- **services** — Built without a spec PDF (no Services PDF was provided); structure modeled on the approved reference. Review its copy/section choices.
- **careers** — All 6 job listings, salaries, and tech tags are placeholder data (wire to a real ATS/job board); team testimonials (names + initials avatars); CV form submit is stubbed; apply links → contact.html.
- **pricing** — All prices/packages are from the spec; client-result testimonials (initials avatars); "Download Pricing Guide" → e-books.html.

## Navigation / IA (needs a decision)
- `careers.html` and `pricing.html` are new pages but are **not yet linked from the main nav or footer** on the other pages (careers.html added a lone "Careers" nav link to itself, creating a nav inconsistency). Decide where Careers & Pricing should live in the navigation (main nav vs footer), then it can be applied consistently to all pages.

## Known non-issues
- Unused `--bbt-teal-light: #D4EFE4` (and `--bbt-teal`/`--bbt-coral`) CSS variable *definitions* remain in a few pages' `:root` but are never referenced — they render nowhere. Can be deleted in a cleanup pass.
