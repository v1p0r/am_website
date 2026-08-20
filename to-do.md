# Site Setup To-Do

Personal academic site for Alexandre V. Morozov, hosted on GitHub Pages.
Working test deployment: <https://v1p0r.github.io/am_website/> — final home will be `https://<advisor-username>.github.io`.

## Already done

- [x] Name set in `_config.yml` (`first_name` / `middle_name` / `last_name`)
- [x] `url` / `baseurl` set for the test location (marked TEMPORARY — see "At transfer time" below)
- [x] GitHub Pages enabled, serving from `gh-pages`; deploy workflow green
- [x] Docker files and Docker workflows removed
- [x] Hardcoded `/al-folio/publications/` link fixed in about pages

## 1. Config identity fields (~10 min, fixes all template branding on the live site)

All in `_config.yml`:

- [x] `description:` — set: Professor of Physics and Astronomy at Rutgers; statistical physics, computational biophysics, evolutionary dynamics
- [x] `keywords:` — set: Alexandre Morozov, Rutgers physics, computational biophysics, statistical physics, evolutionary dynamics, gene regulation
- [x] `footer_text:` — decided: keep the Jekyll/al-folio/GitHub Pages credit as-is
- [x] `blog_name:` and `blog_description:` — set to "blog" / "my blog" (blog page is being kept)
- [x] `scholar:` → `last_name: [Morozov]`, `first_name: [Alexandre, A., A. V.]` — controls which author gets bolded on the publications page (docs/CUSTOMIZE.md:834). Re-check variants against the real papers.bib once it exists (`grep -i morozov _bibliography/papers.bib`)
- [x] Comments: decided — none. `giscus:` left unset, `disqus_shortname` off. (Can be enabled later after the repo transfer: enable Discussions + giscus app — docs/TROUBLESHOOTING.md:362)
- [x] Analytics: decided — off (all `analytics:` IDs left blank, no tracking, no cookie banner needed). Revisit only if visitor stats are ever wanted
- [x] `protect_email: true` — enabled (email split at build time, click-to-copy instead of mailto; docs/CUSTOMIZE.md:414)
- [x] SEO flags: `serve_og_meta: true` and `serve_schema_org: true` enabled (docs/SEO.md §112–245)
- [ ] SEO, deferred: `og_image` (needs a real 1200×630 image in `assets/img/`); `google_site_verification` — do at the final domain after transfer (also listed in section 4)

## 2. Prune workflows (stops CI failures/noise)

In `.github/workflows/`:

- [ ] DEFERRED by decision (2026-08-08): all workflows kept for now — deletion was staged then reverted. Candidates if CI noise/failures become annoying: `lighthouse-badger` (fails without token), `update-citations` (Scholar blocks CI; refresh locally via `conda run -n scholar python bin/update_scholar_citations.py`), `render-cv`, `release`, `star-history`, `update-screenshots`, `visual-regression`, `codeql`, `axe`, `unit-tests`, `upgrade-check`, `copilot-setup-steps`, `prettier-comment-on-pr`, `prettier-html`
- Keep regardless: `deploy.yml`, `prettier.yml`, `update-tocs.yml`, `broken-links.yml`, `broken-links-site.yml`

## 3. Replace demo content (the bulk of the work)

Docs recommend excluding via `_config.yml` `exclude:` rather than deleting, to ease upgrades (docs/CUSTOMIZE.md:1293). When removing pages, fix `nav_order` on the rest and update `_pages/dropdown.md`.

- [ ] `_pages/about.md` — write the real bio
- [ ] `assets/img/prof_pic.jpg` — real profile photo
- [ ] `_bibliography/papers.bib` — replace Einstein's papers with real publications
- [ ] `_data/coauthors.yml`, `_data/venues.yml` — align with the real bibliography (coauthor keys lowercase, no accents)
- [ ] `_data/socials.yml` — real email / Scholar / ORCID / etc. links
- [ ] `_data/repositories.yml` — set to real GitHub username(s), or remove `_pages/repositories.md`
- [ ] Remove demo collections:
  - [ ] `_posts/` — 33 demo posts
  - [ ] `_projects/1_project.md` … `9_project.md`
  - [ ] `_news/announcement_1..3.md`
  - [ ] `_books/the_godfather.md`
  - [ ] `_teachings/` — both demo courses
  - [ ] `_pages/about_einstein.md`, `_pages/profiles.md` (multi-person demo), `_pages/plugins.md`
- [ ] CV: pick ONE of `_data/cv.yml` (RenderCV) or `assets/json/resume.json` (JSONResume), delete the other, set `cv_format` in `_pages/cv.md` — or drop the CV page (docs/CUSTOMIZE.md:346–379)
- [ ] Clean out demo images once nothing references them: `assets/img/1.jpg`–`12.jpg`, `rhino.png`, `book_covers/`, `publication_preview/*` demos

## 4. At transfer time (repo → advisor's account)

- [ ] Move/rename the repo to `<advisor-username>.github.io`
- [ ] `_config.yml`: set `url: https://<advisor-username>.github.io` and blank out `baseurl:` (leave the key, don't delete it) — both lines carry TODO/TEMPORARY comments marking them
- [ ] On the new repo: Settings → Actions → General → Workflow permissions → "Read and write"
- [ ] After first deploy: Settings → Pages → Deploy from a branch → `gh-pages`
- [ ] Custom domain (if ever): add a `CNAME` file on `main`, or the domain setting is wiped every deploy (docs/FAQ.md:40)

## Standing warnings

- Never commit to `gh-pages` — it is overwritten on every deploy; all work happens on `main`.
- Very short posts/news can crash the build via `related_blog_posts` ("Zero vectors can not be normalized") — set `related_posts: false` in that post's front matter if it bites (docs/FAQ.md:64).
