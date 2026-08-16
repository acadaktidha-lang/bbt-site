# bigbinarytech.com

Source for the pages published at <https://bigbinarytech.com>.

The live site is **WordPress with Elementor**. Nothing in this repository is
served to visitors directly. The 26 HTML files at the repository root are the
source of truth for page content, and they reach the site only when they are
pushed into Elementor through the WordPress REST API. Editing a file here changes
nothing until it is published.

## Layout

```
*.html                    the 26 page sources, one per published page
assets/                   image masters (logos)
wp-backup/                local REST snapshots, gitignored, not source
.claude/.wp-credentials   WordPress application password, gitignored
```

`<slug>.html` is named for the live slug, with one exception:
`resources-videos.html` publishes to `/resources/videos/` as a child of
`resources`.

Every image referenced by a page is an absolute `https://bigbinarytech.com/...`
URL served by WordPress. `assets/` holds source masters only; no page loads
anything from it, so moving or renaming a file there cannot break the site.

These files began life as exports from an AI design tool and are now maintained by
hand. Some still carry a `Generated from "BBT ....dc.html"` comment near the top.
That is history, not a live dependency. The prototypes it refers to, along with the
design tool's runtime and its handoff README, were removed once these page files
superseded them. They remain in git history if anyone needs to see the original
design.

## Publishing a page

1. Transform the build file into the Elementor HTML widget. The transform drops the
   document wrapper: `<!DOCTYPE html>`, `<html>`, `<head>`, `<meta charset>`,
   `<meta name="viewport">`, `<title>`, `<meta name="description">`, `</head>`,
   `<body>`, `</body>` and `</html>`. Everything else in the head, including the
   preconnect and style blocks, is kept. Each page holds exactly one `widget/html`
   inside one `section` inside one `column`.
2. `POST /wp-json/wp/v2/pages/<id>` with the result as `meta._elementor_data`.
3. `DELETE /wp-json/elementor/v1/cache`.
4. **Re-read the page and confirm.** The REST API returns 200 and echoes the
   payload back even when nothing persisted. A write is not done until a fresh
   `GET` proves it.

Authenticate with HTTP Basic using the WordPress application password in
`.claude/.wp-credentials`. That file is gitignored and must stay that way.

## Two things that will catch you out

**The `<title>` and `<meta name="description">` in these files never reach the
site.** The transform strips both. SEO metadata lives in Yoast, stored separately
from `_elementor_data`, so republishing a page does not touch it. Read and write it
through the Yoast bulk editor:

```
GET  /wp-json/yoast/v1/bulk_editor/posts?content_type=page&per_page=100
POST /wp-json/yoast/v1/bulk_editor/update_search
     {"items":[{"id":<id>,"seo_title":"...","meta_description":"..."}]}
```

Do not treat the build file tags as a source for those fields. The titles are
generic placeholders and the descriptions are truncated on seven pages.

**The host throttles sustained REST traffic.** Around twelve rapid requests in,
connections begin timing out for minutes at a time. Pace any sweep across all 26
pages, roughly three seconds between requests, and retry with backoff. `curl` has
proven more reliable here than Python's `urllib`.

## Page IDs

Each file publishes to the WordPress page ID below. These IDs are what
`POST /wp-json/wp/v2/pages/<id>` needs, so keep this table current.

| ID | File | Live slug |
| --- | --- | --- |
| 478 | `about-us.html` | `about-us` |
| 550 | `ai-automation.html` | `ai-automation` |
| 459 | `blogs.html` | `blogs` |
| 471 | `careers.html` | `careers` |
| 460 | `case-studies.html` | `case-studies` |
| 461 | `contact-us.html` | `contact-us` |
| 551 | `custom-software-development.html` | `custom-software-development` |
| 462 | `digital-transformation.html` | `digital-transformation` |
| 463 | `e-books-guides.html` | `e-books-guides` |
| 464 | `enterprise-odoo.html` | `enterprise-odoo` |
| 716 | `faqs.html` | `faqs` |
| 482 | `home.html` | `home` |
| 465 | `industries.html` | `industries` |
| 553 | `odoo-erp.html` | `odoo-erp` |
| 720 | `outsourcing.html` | `outsourcing` |
| 715 | `pos-systems.html` | `pos-systems` |
| 472 | `pricing.html` | `pricing` |
| 3 | `privacy-policy.html` | `privacy-policy` |
| 466 | `resources.html` | `resources` |
| 721 | `resources-videos.html` | `videos`, child of `resources`, at `/resources/videos/` |
| 467 | `retail-pos.html` | `retail-pos` |
| 470 | `services.html` | `services` |
| 468 | `social-media-marketing.html` | `social-media-marketing` |
| 469 | `solutions.html` | `solutions` |
| 703 | `terms-of-service.html` | `terms-of-service` |
| 552 | `web-development.html` | `web-development` |

Six pages were renamed off WordPress attachment collision suffixes
(`/about-us-2/`, `/ai-automation-2/`, `/digital-transformation-3/`,
`/resources-2/`, `/retail-pos-2/`, `/solutions-3/`). The old URLs are kept alive
by 301s in the Redirection plugin, so do not reuse those names.

## Working conventions

Changes land on a branch and merge to `master` by pull request in Azure DevOps.
There is no GitHub remote, so `gh` will not work.

The delivery log, page manifest, SEO tracker, spec audits and QA reports used to
live in a `docs/` folder. It was removed to keep this repository to one README,
the page files and their assets. Nothing is lost: every one of those files is
still in git history, recoverable with
`git log --diff-filter=D --name-only -- docs/` and `git show <commit>^:<path>`.
