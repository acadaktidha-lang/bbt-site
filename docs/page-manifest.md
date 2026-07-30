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
| 482 | `home` | `project/build/home.html` | Home |
| 465 | `industries` | `project/build/industries.html` | Industries |
| 553 | `odoo-erp` | `project/build/odoo-erp.html` | Odoo & ERP Services |
| 472 | `pricing` | `project/build/pricing.html` | Pricing |
| 3 | `privacy-policy` | `project/build/privacy-policy.html` | Privacy Policy |
| 466 | `resources` | `project/build/resources.html` | Resources |
| 467 | `retail-pos` | `project/build/retail-pos.html` | Retail & POS Solutions |
| 470 | `services` | `project/build/services.html` | Services |
| 468 | `social-media-marketing` | `project/build/social-media-marketing.html` | Social Media Marketing |
| 469 | `solutions` | `project/build/solutions.html` | Solutions |
| 703 | `terms-of-service` | `project/build/terms-of-service.html` | Terms of Service |
| 552 | `web-development` | `project/build/web-development.html` | Web Development |

22 published pages, 22 build files, one to one.

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
| `/pos-systems/` | `/retail-pos/` |
