# Page manifest

Every published page on bigbinarytech.com and the build file it is pushed from.
Generated from the live site, not from memory. One file per page, filename always
equal to the live slug.

To publish a page: transform its build file into the Elementor HTML widget, POST it
to `/wp-json/wp/v2/pages/<id>` as `meta._elementor_data`, then
`DELETE /wp-json/elementor/v1/cache`. Always re-GET afterwards: the REST API returns
200 and echoes the payload even when nothing persisted.

| Page ID | Slug | Build file | Title |
| --- | --- | --- | --- |
| 478 | `about-us` | `project/build/about-us.html` | About Us |
| 550 | `ai-automation` | `project/build/ai-automation.html` | AI & Automation |
| 459 | `blogs` | `project/build/blogs.html` | Blog & Insights |
| 471 | `careers` | `project/build/careers.html` | Careers |
| 460 | `case-studies` | `project/build/case-studies.html` | Case Studies |
| 461 | `contact-us` | `project/build/contact-us.html` | Contact Us |
| 551 | `custom-software-development` | `project/build/custom-software-development.html` | Custom Software Development |
| 462 | `digital-transformation` | `project/build/digital-transformation.html` | Digital Transformation |
| 463 | `e-books-guides` | `project/build/e-books-guides.html` | Ebooks & Guides |
| 464 | `enterprise-odoo` | `project/build/enterprise-odoo.html` | Enterprise & Odoo ERP |
| 716 | `faqs` | `project/build/faqs.html` | FAQs |
| 482 | `home` | `project/build/home.html` | Home |
| 465 | `industries` | `project/build/industries.html` | Industries |
| 553 | `odoo-erp` | `project/build/odoo-erp.html` | Odoo & ERP Services |
| 720 | `outsourcing` | `project/build/outsourcing.html` | Outsourcing |
| 715 | `pos-systems` | `project/build/pos-systems.html` | POS Systems |
| 472 | `pricing` | `project/build/pricing.html` | Pricing |
| 3 | `privacy-policy` | `project/build/privacy-policy.html` | Privacy Policy |
| 466 | `resources` | `project/build/resources.html` | Resources |
| 721 | `videos` (child of `resources`, lives at `/resources/videos/`) | `project/build/resources-videos.html` | Video Tutorials & Walkthroughs |
| 467 | `retail-pos` | `project/build/retail-pos.html` | Retail & POS Solutions |
| 470 | `services` | `project/build/services.html` | Services |
| 468 | `social-media-marketing` | `project/build/social-media-marketing.html` | Social Media Marketing |
| 469 | `solutions` | `project/build/solutions.html` | Solutions |
| 703 | `terms-of-service` | `project/build/terms-of-service.html` | Terms of Service |
| 552 | `web-development` | `project/build/web-development.html` | Web Development |

26 published pages, 26 build files. `faqs` (716) and `pos-systems` (715) were
created and published on 2026-08-01; `outsourcing` (720) and `videos` (721)
followed, so every page now has a live URL behind it.

Note the build filename and the live slug differ for one page: `resources-videos.html`
publishes to slug `videos` under parent `resources`, i.e. `/resources/videos/`.

The whole set was last published on 2026-08-07: all 26 pages written to
`meta._elementor_data`, verified by re-read, Elementor cache flushed. Every live
URL was then re-fetched and checked for HTTP 200, exactly one `<header>` and one
`<footer>` (no theme chrome doubling), valid JSON-LD, and the Wyoming postal
address present in the structured data.

## Page content is not the only place copy lives

The 2026-08-07 Pakistan-to-Wyoming sweep found that pushing `_elementor_data`
leaves **Yoast SEO fields untouched**. `/careers/` and `/enterprise-odoo/` still
served "Pakistan" in the `<title>` and `<meta name="description">` after all 26
pages had been republished and the cache flushed.

Those fields are not in `meta` on `/wp/v2/pages/<id>` — they are not registered
for REST. Read and write them through the Yoast bulk editor instead:

- `GET  /wp-json/yoast/v1/bulk_editor/posts?content_type=page&per_page=100`
- `POST /wp-json/yoast/v1/bulk_editor/update_search` with
  `{"items":[{"id":<id>,"seo_title":"...","meta_description":"..."}]}`

After any sitewide copy change, sweep `focus_keyphrase`, `seo_title`,
`meta_description`, `social_title` and `social_description` across every page.

## Slugs that changed

These six pages were renamed off WordPress attachment collision suffixes. The old
URLs are kept alive by 301s in the Redirection plugin, so do not reuse the old names.

| Old URL | Current URL |
| --- | --- |
| `/about-us-2/` | `/about-us/` |
| `/ai-automation-2/` | `/ai-automation/` |
| `/digital-transformation-3/` | `/digital-transformation/` |
| `/resources-2/` | `/resources/` |
| `/retail-pos-2/` | `/retail-pos/` |
| `/solutions-3/` | `/solutions/` |

## The /pos-systems/ redirect was disabled on 2026-08-01

`/pos-systems/` used to 301 to `/retail-pos/`, left over from the deleted draft.
POS Systems is now a service page in its own right and sits in the Services mega
menu, so that redirect had to go.

Redirection rule id 7 is now **disabled, not deleted**. The plugin's REST API has
no DELETE route for a single redirect; disabling it via
`POST /wp-json/redirection/v1/bulk/redirect/disable` with `{"items":[7]}` achieves
the same result and is reversible. Note that `POST /redirection/v1/redirect/7` with
`enabled:false` looks like it works, returning 200 with the payload echoed, but the
flag does not persist. The bulk endpoint is the one that actually writes.

The rule as it stood is saved at `wp-backup/2026-08-01-spec-sections/removed-redirect-7.json`.
`/pos-systems/` now returns 200 with no redirect hop.
