# SEO metadata to set in WordPress

These two lists are QA findings that cannot be fixed from the site code. Yoast
stores them per page, outside `_elementor_data`, so republishing a build file
never touches them.

**Correction, 2026-08-12:** this file previously said Yoast's REST route will not
persist a change and that every value had to be pasted in through the admin. That
is wrong. The bulk editor route does persist:

```
POST /wp-json/yoast/v1/bulk_editor/update_search
{"items":[{"id":<id>,"seo_title":"...","meta_description":"..."}]}
```

Verified on 2026-08-12 by setting `seo_title` on `/pos-systems/` (715), re-reading
it from `GET /wp-json/yoast/v1/bulk_editor/posts?content_type=page&per_page=100`,
and confirming the rendered `<title>` on the live URL. No Elementor cache flush
was needed. Always re-read and re-fetch the live page to confirm, because the
endpoint returns `{"success":true}` either way.

The admin UI remains a valid fallback: edit the page, scroll to the Yoast SEO
panel below the editor, open the Google preview and use the SEO title and Meta
description fields.

**Note:** the build files carry their own `<title>` and `<meta name="description">`,
but the publish transform strips both, so they are not the source of truth for
anything on this list. Most are generic placeholders of the form
`Page Name · Big Binary Tech`. Do not copy them into Yoast.

---

## 1. Page titles that are too long

Google cuts titles off around 60 characters. Each of these currently runs over.

| Page | Replace with | Chars |
| --- | --- | --- |
| `/services/` | Business Automation Services | Big Binary Tech | 46 |
| `/industries/` | Industry Software Solutions | Big Binary Tech | 45 |
| `/contact-us/` | Contact Us | Free Consultation in 24h | Big Binary Tech | 55 |
| `/pricing/` | Transparent Project Pricing | Big Binary Tech | 45 |
| `/case-studies/` | Client Case Studies and Results | Big Binary Tech | 49 |
| `/blogs/` | Blog: AI, Odoo and Cloud Insights | Big Binary Tech | 51 |
| `/enterprise-odoo/` | Enterprise Odoo ERP Implementation | Big Binary Tech | 52 |
| `/retail-pos/` | Retail POS Systems and Solutions | Big Binary Tech | 50 |
| `/ai-automation/` | AI Automation and Integration | Big Binary Tech | 47 |

For reference, what each one says now:

- `/services/` currently `Technology Services to Automate Your Business | Big Binary Tech` (63 chars)
- `/industries/` currently `Industry-Specific Technology & Software Solutions | Big Binary Tech` (67 chars)
- `/contact-us/` currently `Contact Big Binary Tech | Get Free 30-minutes Consultation in 24h` (65 chars)
- `/pricing/` currently `Transparent Pricing for Digital Transformation | Big Binary Tech` (64 chars)
- `/case-studies/` currently `Case Studies | Big Binary Tech | Proven Results from Our Clients` (64 chars)
- `/blogs/` currently `Digital Transformation Blog | AI Automation, Odoo & Cloud Insights` (66 chars)
- `/enterprise-odoo/` currently `Enterprise Odoo ERP Solutions & Implementation | Big Binary Tech` (64 chars)
- `/retail-pos/` currently `Retail POS Solutions & systems for Your business | Big Binary Tech` (66 chars)
- `/ai-automation/` currently `AI Automation & Integration Services | Free Audit | Big Binary Tech` (67 chars)

Note on `/retail-pos/`: the current title reads "systems for Your business",
with an odd capital on "Your" and a lowercase "systems". The replacement fixes both.

---

## 2. Pages with no meta description

With no description set, Google invents one from whatever text it finds on the page.

| Page | Paste this | Chars |
| --- | --- | --- |
| `/odoo-erp/` | Odoo ERP implementation, migration and support from a certified Odoo partner, for businesses across Pakistan, the Gulf, the UK and the US. | 138 |
| `/custom-software-development/` | Custom software built around how your business actually works, from first specification through to launch and ongoing support. | 126 |
| `/privacy-policy/` | How Big Binary Tech collects, uses and protects personal information through this website, and the rights you have over your own data. | 134 |
| `/terms-of-service/` | The terms that apply when you use the Big Binary Tech website, covering acceptable use, intellectual property and limits of responsibility. | 139 |

All four sit inside the 160 character limit.

Flag on `/odoo-erp/`: the copy above says "across Pakistan, the Gulf, the UK and
the US". Pakistan was removed sitewide by the 2026-08-07 Pakistan to Wyoming
sweep, so this line predates that decision and must be reworded before it is
pasted anywhere.

---

## 2b. Audit of 2026-08-12: the two lists above are incomplete

A full sweep of `seo_title` and `meta_description` across all 26 pages found more
empty fields than section 2 records. Live state at the time of the sweep:

**Empty `seo_title`** (page falls back to the Yoast template, which renders as the
WordPress page title plus the site name):

| Page | ID | Status |
| --- | --- | --- |
| `/pos-systems/` | 715 | **Set on 2026-08-12** to `Custom POS System Development Company \| Big Binary Tech`, the value commit 337258f had put in the build file. Verified live. |
| `/custom-software-development/` | 551 | still empty |
| `/odoo-erp/` | 553 | still empty |
| `/faqs/` | 716 | still empty |
| `/outsourcing/` | 720 | still empty |
| `/privacy-policy/` | 3 | still empty |
| `/terms-of-service/` | 703 | still empty |
| `/resources/videos/` | 721 | still empty |

**Empty `meta_description`**, beyond the four in section 2: `/faqs/` (716),
`/outsourcing/` (720), `/pos-systems/` (715) and `/resources/videos/` (721).

Section 1's nine replacement titles are deliberately **not** applied. Every one of
those pages carries hand-authored SEO copy today, and the standing instruction is
to leave live SEO copy alone. Applying them is a separate, explicit decision.

The empty `seo_title` fields are also left alone on purpose. The build file title
tags are generic, of the form `Page Name · Big Binary Tech`, and Yoast's own
fallback already renders effectively the same string from the WordPress page title
plus the site name. Writing them would add no value.

### Descriptions applied on 2026-08-12

Unlike the title tags, the build file `<meta name="description">` values are real
page-specific copy, so they are a usable source. These four were written into
previously **empty** `meta_description` fields, which disturbed no existing copy:

| Page | ID | Chars |
| --- | --- | --- |
| `/pos-systems/` | 715 | 139 |
| `/privacy-policy/` | 3 | 142 |
| `/terms-of-service/` | 703 | 135 |
| `/resources/videos/` | 721 | 145 |

### Still open, and why

| Page | ID | Blocker |
| --- | --- | --- |
| `/faqs/` | 716 | Build copy is 162 chars, 2 over the limit. Needs a trim decision. |
| `/outsourcing/` | 720 | Build copy is 161 chars, 1 over the limit. Needs a trim decision. |
| `/custom-software-development/` | 551 | **Build copy is truncated mid-word**, ending "from internal management syste". Fix the build file first. |
| `/odoo-erp/` | 553 | **Build copy is truncated**, ending "keeps finance, HR, inventory,". Fix the build file first. |

Those last two are a defect in the build files themselves, not just a WordPress
gap. Whatever wrote those `<meta name="description">` tags cut them off at 180
characters. Do not paste either one anywhere until the sentence is finished.

---

## 3. Contact form notification address: already done

**Resolved.** Checked on 2026-08-12 via
`GET /wp-json/contact-form-7/v1/contact-forms/704`. The primary mail `recipient`
is already `business@bigbinarytech.com`, not `[_site_admin_email]`, so there is
nothing left to change. `mail_2`, the autoresponder, is inactive.

Worth knowing for next time: Contact Form 7 does expose a REST route at
`/wp-json/contact-form-7/v1/contact-forms/<id>`, so its settings can be read, and
written, without opening WP Admin. This file previously sent the reader to the
admin UI for this.

Every submission is also stored under Flamingo, Inbound Messages.

