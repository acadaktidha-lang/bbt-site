# BigBinaryTech Website Redesign: Delivery Backlog

Source of truth for the Azure DevOps board in the **bigbinarytech.com** project
(organization: **bigbinarytech**). This file reconstructs the full delivery
history of this repository as Epics, User Stories, and Tasks so the work is
documented, logged, and traceable from the start.

## Conventions

- **Work item hierarchy:** Epic contains Issues. An Issue contains Tasks only
  where a task adds real detail beyond the story. The tier written below as
  "User Story" is created on the board as an **Issue** (see the process note).
- **Traceability:** every item cites the exact commit hashes it maps to, so the
  board and the git history stay in lock-step. This is the audit trail.
- **Assignment:** every item is assigned to **You** and, once closed, its
  Completed By (Closed By) is **You**. "You" resolves to the signed-in Azure
  DevOps account when the items are created.
- **State:** this process has three states, **To Do**, **Doing** and **Done**.
  Finished history is **Done**. Work still in progress is **Doing**. Future work
  is **To Do**. There is no New/Active/Closed triad in this process.
- **Writing standard:** no em dashes and no en dashes anywhere, in titles,
  descriptions, comments, or acceptance criteria. Plain sentences only. This
  standard is already part of the project (see US-3.4).
- **Process note, now confirmed:** the bigbinarytech.com project runs the
  **Basic** process. Its work item types are Epic, Issue and Task. There is no
  "User Story" type, so the middle tier was created as **Issue**. The project
  property string reads "Scrum" but that value is vestigial and does not match
  the actual type set. Two further consequences: this process has no Acceptance
  Criteria field, so each acceptance line lives inside the item description, and
  Azure DevOps refuses to create a work item directly in a non-initial state, so
  every item was created in To Do and then transitioned.

## Total scope

8 Epics, 24 Issues, mapping all 54 commits. Everything is Done.

## Board status: created

All 32 items exist on the bigbinarytech.com board, assigned to Ghazanfar Sheikh,
with Closed By set on every Done item.

- Work items 1 to 8 are EPIC-1 through EPIC-8, in that order. EPIC-1 reuses the
  pre-existing work item 1, which was retitled from "BigBinaryTech Website
  Layout".
- Work items 9 to 32 are the 24 Issues, in seed file order from US-1.1 to
  US-8.2, each parented to its Epic.
- Every item is Done. US-7.4 (work item 30) was the last one open, and closed
  when its work landed as commit 83aebc1.

The code now lives in Azure DevOps Repos as well. The repository
`bigbinarytech.com` in this same project holds all 54 commits on master plus the
feat/site-review-fixes and fix/hero-headline-crossfade branches, and is the
`origin` remote. The former GitHub remote is kept as `github`.

---

## EPIC-1: Website redesign build (foundation)

Stand up the initial multi-page website and its baseline content.

### US-1.1: Establish the initial site and page set

- Type: User Story
- State: Closed (Done)
- Commits: 416d9b5, 1203fbb, cc1a15c, ec63e6a
- What: created the first version of the site and its page structure, including
  the solution pages.
- How: added the initial HTML page set and asset folders to the repository.
- Why: to give the redesign a working baseline to iterate on.
- Acceptance: the site renders locally with its core pages in place.

### US-1.2: Populate core page content

- Type: User Story
- State: Closed (Done)
- Commits: 76213f9, a4ea331, 1dd59db
- What: filled in the page copy and updated the About page content.
- How: edited the page HTML to carry real content instead of placeholders.
- Why: to move the pages from scaffolding toward review-ready content.
- Acceptance: core pages carry their intended copy.

---

## EPIC-2: Responsive design and mobile experience

Make the pages work on phones and tablets and add mobile navigation.

### US-2.1: Add responsive layouts and mobile menu to About, Blogs, Industries

- Type: User Story
- State: Closed (Done)
- Commits: 12e16eb (merged via PR #8, e4cb449), ef6ffaa
- What: added responsive breakpoints and a working mobile menu to the About,
  Blogs, and Industries pages.
- How: introduced responsive CSS and a mobile navigation pattern on those pages.
- Why: the pages were not usable on small screens without it.
- Acceptance: the three pages reflow cleanly and the mobile menu opens and
  closes on a phone viewport.

### US-2.2: Integrate responsive-page contributions and reconcile revert churn

- Type: User Story
- State: Closed (Done)
- Commits: PR #1 (2f302df), revert PR #2 (ea8fc43, c1e5264), PR #3 (080b230),
  revert PR #4 (8558622, fabdc35), PR #7 (ef6ffaa, cf00db2), uploads 0538795
- What: integrated several responsive-page contributions, some of which were
  reverted and then re-applied cleanly.
- How: merged the contribution branches, reverted the uploads that regressed
  the layout, and kept the versions that matched the specs.
- Why: to land the responsive work without carrying the broken uploads forward.
- Acceptance: the responsive pages that survived reconciliation are the ones on
  master, with no orphaned upload commits active.

---

## EPIC-3: Content and layout-spec reconciliation

Bring every page in line with the layout and content specifications.

### US-3.1: Reconcile all pages to layout specs and add three new pages

- Type: User Story
- State: Closed (Done)
- Commits: 60b059f
- What: reconciled all existing pages to their layout specs, added responsive
  behavior, and added three new pages.
- How: reworked the page markup and styles to match the spec layouts.
- Why: to close the gap between the built pages and the agreed designs.
- Acceptance: pages match their layout specs and the three new pages exist.

### US-3.2: Reconcile solution and service copy to content specs

- Type: User Story
- State: Closed (Done)
- Commits: 5d8edba, 79bf2e4
- What: aligned the copy on the solution and service pages, and across seven
  pages, with the content specs.
- How: replaced drifted copy with the spec-approved wording.
- Why: the live copy had drifted from the approved content.
- Acceptance: the reconciled pages carry spec-approved copy.

### US-3.3: Preserve reconciled About, Blogs, Industries through merge

- Type: User Story
- State: Closed (Done)
- Commits: c997144 (merged via PR #13, bb100be)
- What: kept the reconciled About, Blogs, and Industries pages when merging
  origin/master.
- How: resolved the merge in favor of the reconciled versions.
- Why: to avoid regressing the reconciled pages during the merge.
- Acceptance: the merged tree carries the reconciled versions of those pages.

### US-3.4: Professionalize typography and page metadata

- Type: User Story
- State: Closed (Done)
- Commits: 5550580, 4c5807e
- What: rewrote em and en dashes into plain sentences across all pages, added a
  B-monogram favicon, and cleaned up the page titles.
- How: edited the copy to remove em and en dashes and updated the head metadata.
- Why: to make the site read professionally and to set the no-dashes standard
  the project follows.
- Acceptance: no em or en dashes remain in page copy, the favicon shows, and
  titles are clean.
- Note: this story is the origin of the no-dashes rule applied to this backlog.

---

## EPIC-4: Service pages build-out

Build the individual service pages to the sections their specs call for.

### US-4.1: Add the first four built service pages and fix footer full-bleed

- Type: User Story
- State: Closed (Done)
- Commits: a6c0fbc
- What: added four built-out service pages and fixed the footer so it spans the
  full width.
- How: authored the service page markup and corrected the footer container.
- Why: to expand the service coverage and fix the boxed footer.
- Acceptance: the four service pages render and the footer is full-bleed.

### US-4.2: Add the POS Systems service page and link it from the menu

- Type: User Story
- State: Closed (Done)
- Commits: 39c9c62
- What: added the POS Systems service page and linked it from the Services menu.
- How: authored the page and added its menu entry.
- Why: to complete the service menu coverage.
- Acceptance: the POS Systems page exists and is reachable from the menu.

### US-4.3: Build the AI Automation service sections

- Type: User Story
- State: Closed (Done)
- Commits: f36483f
- What: built the six service sections the AI Automation spec calls for.
- How: authored the six sections to match the content spec.
- Why: the page was missing the sections its spec required.
- Acceptance: all six AI Automation sections are present and match the spec.

### US-4.4: Build the Web Development service sections

- Type: User Story
- State: Closed (Done)
- Commits: 16a2c1c
- What: built the seven service sections the Web Development spec calls for.
- How: authored the seven sections to match the content spec.
- Why: the page was missing the sections its spec required.
- Acceptance: all seven Web Development sections are present and match the spec.

### US-4.5: Restore the Social Media Marketing copy from spec

- Type: User Story
- State: Closed (Done)
- Commits: 7751e3b
- What: restored the Social Media Marketing page copy from its content spec.
- How: replaced the drifted copy with the spec-approved wording.
- Why: the page copy had drifted from the approved content.
- Acceptance: the page carries the spec-approved copy.

---

## EPIC-5: Bug-fix hardening (site review)

Fix the defects surfaced by the site review across navigation, layout, and
interactive components.

### US-5.1: Fix mobile navigation defects

- Type: User Story
- State: Closed (Done)
- Commits: 40a1b45, b67b070
- What: fixed the mobile nav scroll-lock, dead mobile-nav links caused by an
  overlay painting over the drawer, dead CSS variables, and a blank page when a
  CDN failed to load.
- How: corrected the scroll-lock handling, layered the drawer above the overlay,
  removed the dead variables, and added a fallback path for CDN failure.
- Why: the mobile navigation was partly unusable and could blank the page.
- Acceptance: the mobile menu links work, scrolling locks correctly, and a CDN
  failure no longer blanks the page.

### US-5.2: Fix layout clipping and off-screen elements

- Type: User Story
- State: Closed (Done)
- Commits: 1da6010, 790447e, 3aef15d, 4633c43
- What: let the home hero heading box grow instead of clipping, stopped the
  process heading flying off-screen, stopped the process section clipping its
  last card row on short laptops, and removed a stray closing div that
  unbalanced the Industries markup.
- How: adjusted the affected layout rules and fixed the broken markup.
- Why: content was being clipped or pushed off-screen and the markup was
  unbalanced.
- Acceptance: the hero heading, process heading, and process cards display fully
  and the Industries markup is balanced.

### US-5.3: Fix interactive components

- Type: User Story
- State: Closed (Done)
- Commits: 929b1a8, afdcb04, 23a230b, ca52add, 5a92c3f
- What: centered the active trust-slider card instead of assuming a fixed 300px
  width, removed the hero headline flicker with a valley-free one-at-a-time
  crossfade, stopped the Enterprise Odoo section orphaning its fourth module
  card, and fixed the scroll-progress bar while removing stale root duplicates.
- How: reworked the slider centering math, the headline rotation, the Odoo card
  grid, and the scroll-progress calculation.
- Why: these components behaved incorrectly and looked unpolished.
- Acceptance: the slider centers correctly, the headline rotates without
  flicker, the Odoo cards align, and the scroll-progress bar reports correctly.

### US-5.4: Apply Home, About, Contact review and contact-spec fixes

- Type: User Story
- State: Closed (Done)
- Commits: fb2ab04
- What: applied the review-report fixes and the contact-spec fixes to the Home,
  About, and Contact pages.
- How: edited the three pages per the review report and the contact spec.
- Why: to clear the outstanding review items on those pages.
- Acceptance: the review-report and contact-spec items for those pages are
  resolved.

---

## EPIC-6: Performance optimization

Reduce load cost and runtime jank on the homepage.

### US-6.1: Remove the homepage relayout loop and eager background video

- Type: User Story
- State: Closed (Done)
- Commits: 3d04854
- What: stopped a homepage relayout loop and removed 8 MB of eager background
  video that loaded on page open.
- How: broke the layout-thrash loop and stopped auto-loading the heavy video.
- Why: the loop and the eager video hurt load time and caused jank.
- Acceptance: the homepage no longer runs the relayout loop and does not eagerly
  load the 8 MB video.

---

## EPIC-7: WordPress deployment readiness

Prepare the pages to run inside WordPress and Elementor and finalize the build.

### US-7.1: Prepare WP-ready assets and clean the Resources mega-menu

- Type: User Story
- State: Closed (Done)
- Commits: f59e9ce
- What: prepared WordPress-ready assets and cleaned up the Resources mega-menu
  across the site.
- How: adjusted the assets and menu markup for the WordPress context.
- Why: to make the assets and menu behave correctly once deployed to WordPress.
- Acceptance: the assets are WordPress-ready and the Resources mega-menu is
  clean sitewide.

### US-7.2: Cancel the Elementor boxed container for full-bleed layout

- Type: User Story
- State: Closed (Done)
- Commits: 43ab4ff
- What: added a fix that cancels the Elementor boxed 1140px container.
- How: added a full-bleed override so the layout is not constrained by the
  theme container.
- Why: the theme container boxed the layout that was designed to be full-bleed.
- Acceptance: the layout spans full width inside Elementor.

### US-7.3: Nav cleanup and real footer contact data across all pages

- Type: User Story
- State: Closed (Done)
- Commits: 8d82e96
- What: cleaned up the navigation and put real footer contact data on all pages.
- How: edited the shared nav and footer markup across the pages.
- Why: the footer carried placeholder contact data and the nav needed cleanup.
- Acceptance: every page shows the real footer contact data and a clean nav.

### US-7.4: Finalize the WordPress document-container builds and logo

- Type: Issue
- State: Done
- Commits: 83aebc1
- What: finalized the WordPress document-container (.dc) builds, swapped the
  light logo, and produced the build/ and build14/ output.
- How: edited the .dc.html builds, replaced the light logo with the higher
  resolution asset, and regenerated the build output.
- Why: to produce the final WordPress-ready bundle for deployment.
- Acceptance: the .dc builds and logo are final and the build output is
  generated. Met.
- Note: the same commit swapped the services list across the nav and footer to
  the client's final six, so POS Systems is out and Social Media Marketing is
  in. Build output is build/ with 7 pages and build14/ with 13 pages.

---

## EPIC-8: Repository structure and handoff

Document the frontend handoff and reduce the repo to the redesign bundle.

### US-8.1: Add the frontend brand handoff documentation

- Type: User Story
- State: Closed (Done)
- Commits: see BBT-FRONTEND-BRAND-HANDOFF.md
- What: added the frontend brand handoff document.
- How: authored the handoff markdown describing the brand and frontend.
- Why: to give the client and any future developer a clear handoff reference.
- Acceptance: the handoff document exists and describes the brand and frontend.

### US-8.2: Clean the repo down to the redesign bundle

- Type: User Story
- State: Closed (Done)
- Commits: e662ccc, 5a92c3f, 8dc64ea
- What: removed the old project tree and stale root duplicates, ignored the
  root-level audit screenshots, and reduced the repo to the redesign bundle.
- How: deleted the superseded files, updated .gitignore, and kept only the
  redesign bundle.
- Why: to leave a clean repository focused on the redesign.
- Acceptance: the repo contains only the redesign bundle and ignores the audit
  screenshots.

---

## Going forward

New code changes are tracked the same way: a User Story per change, assigned to
You, with what changed, how, and why, and the commit hash cited on completion.
No em dashes and no en dashes in any item.
