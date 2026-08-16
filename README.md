# bigbinarytech.com

Source for the pages published at <https://bigbinarytech.com>.

The live site is **WordPress with Elementor**. Nothing in this repository is
served to visitors directly. The 26 HTML files in
`bigbinarytech-website-redesign/project/build/` are the source of truth for page
content, and they reach the site only when they are pushed into Elementor through
the WordPress REST API. Editing a file here changes nothing until it is published.

## Layout

```
bigbinarytech-website-redesign/project/build/   the 26 page sources, one per published page
docs/                                          manifest, backlog, runbooks, audits
wp-backup/                                     local REST snapshots, gitignored, not source
.claude/.wp-credentials                         WordPress application password, gitignored
```

`build/<slug>.html` is named for the live slug, with one exception:
`resources-videos.html` publishes to `/resources/videos/` as a child of
`resources`. [docs/page-manifest.md](docs/page-manifest.md) maps every file to its
WordPress page ID and is the authoritative list.

These files began life as exports from an AI design tool and are now maintained by
hand. Some still carry a `Generated from "BBT ....dc.html"` comment near the top.
That is history, not a live dependency. The prototypes it refers to, along with the
design tool's runtime and its handoff README, were removed once `build/` superseded
them. They remain in git history if anyone needs to see the original design.

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
generic placeholders and the descriptions are truncated on seven pages. See
[docs/seo-metadata-todo.md](docs/seo-metadata-todo.md).

**The host throttles sustained REST traffic.** Around twelve rapid requests in,
connections begin timing out for minutes at a time. Pace any sweep across all 26
pages, roughly three seconds between requests, and retry with backoff. `curl` has
proven more reliable here than Python's `urllib`.

## Docs

| File | What it covers |
| --- | --- |
| [page-manifest.md](docs/page-manifest.md) | Every page, its ID, its build file, and the publish procedure in full |
| [backlog.md](docs/backlog.md) | Delivery log, one entry per change, commit hash cited on completion |
| [seo-metadata-todo.md](docs/seo-metadata-todo.md) | Outstanding Yoast work and the build tag caveats |
| [ado-backlog-runbook.md](docs/ado-backlog-runbook.md) | Reconstructing this board in Azure DevOps |
| [spec-audit-2026-08-05.md](docs/spec-audit-2026-08-05.md) | Page copy audited against the client specs |
| [stats-verification.md](docs/stats-verification.md) | Provenance for the numbers quoted on the site |
| [portfolio-case-studies.md](docs/portfolio-case-studies.md) | Case study source copy |
| [ai-data-services-copy.md](docs/ai-data-services-copy.md) | Service page source copy |
| [qa-report-2026-07-28.html](docs/qa-report-2026-07-28.html) | QA sweep from 2026-07-28 |

## Working conventions

Changes land on a branch and merge to `master` by pull request in Azure DevOps.
There is no GitHub remote, so `gh` will not work. Every change gets a backlog entry
with its commit hash. No em dashes and no en dashes in backlog items.
