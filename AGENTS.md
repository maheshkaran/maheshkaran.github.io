---
title: maheshkaran.github.io — agent guide
type: agent-instructions
updated: 2026-09-27
registry_id: website
---

Global rules: `~/hub/AGENTS.md` (this file adds project specifics and wins on conflict).

# maheshkaran.github.io

Karan Mahesh's personal academic site (https://maheshkaran.github.io), a fork of the
[al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. `README.md` is al-folio's upstream
README, not project docs. In practice the site is a single **About** page (bio, news, selected
papers, social links) plus the CV PDF; the Projects, Publications, Teaching and CV pages exist but
are hidden (`nav: false`). Recent work (2026-06 to 2026-09-16) has been bio/headshot/CV updates.
Deploys automatically on push to `master`.

## Commands
| Task | Command |
|---|---|
| local serve (docker, recommended) | `bin/docker_build_image.sh` (once; builds `al-folio:latest`), then `bin/docker_run.sh` → http://localhost:8080 |
| local serve (native) | `bundle install && bundle exec jekyll serve` — **not currently possible**: only system Ruby 2.6 (`/usr/bin/ruby`) is installed; CI uses Ruby 3.2. Install Ruby 3.2 (e.g. Homebrew) first |
| production build (what CI runs) | `JEKYLL_ENV=production bundle exec jekyll build` → `_site/` |
| deploy | `git push origin master` → `.github/workflows/pages.yml` builds and deploys to GitHub Pages |
| tests | none |

Not verified 2026-09-27 (no Jekyll install; no servers per task rules). Both `bin/docker_*.sh` scripts delete `Gemfile.lock` first (it is gitignored).

## Layout — user content (edit these)
| Path | What it is |
|---|---|
| `_pages/about.md` | the home page (`permalink: /`): front matter (subtitle, profile image, `news`/`selected_papers`/`social` toggles) + bio prose |
| `_news/announcement_N.md` | news items on the About page (`layout: post`, `date`, `inline: true`); add a new numbered file |
| `_bibliography/papers.bib` | publications (jekyll-scholar). Entries with `selected={true}` appear on About; `preview=` refers to `assets/img/publication_preview/` |
| `assets/pdf/Karan_Mahesh_CV.pdf` | CV linked from About and the CV icon; replace in place to update |
| `assets/img/Karan_Mahesh.png` | profile photo (set in `about.md` `profile.image`); other headshots in `assets/img/` |
| `_config.yml` | name, URL, social IDs (e.g. `scholar_userid`), feature flags (`enable_*`), scholar settings |
| `_includes/social.html` | which social icons appear (user-edited) |
| `_data/cv.yml`, `_data/coauthors.yml` | CV-page data and coauthor links — still al-folio placeholders (Einstein) |
| `_pages/{cv,projects,publications,teaching,dropdown}.md` | hidden pages (`nav: false`); `publications.md` `years:` list is still placeholder |
| `_projects/`, `_posts/`, `blog/` | al-folio sample content (placeholders) |
| `google59db35dd7b770187.html`, `robots.txt` | Google site verification; crawler rules |

## Layout — theme internals (don't touch unless changing the theme)
`_layouts/`, `_includes/` (except `social.html`), `_sass/`, `assets/{css,js}/`, `_plugins/`,
`404.html`, `Dockerfile`, `bin/`, `.github/{ISSUE_TEMPLATE,FUNDING.yml,stale.yml}`, `.all-contributorsrc`,
`CONTRIBUTING.md`, `LICENSE`, `README.md`. The user has made small edits to `_layouts/about.html`,
`_includes/{header,metadata,selected_papers}.html`, `_includes/scripts/mathjax.html`, `_sass/_base.scss`,
`_sass/_themes.scss` — preserve them if merging upstream al-folio.

## Conventions
- Content is Markdown/HTML with YAML front matter; news files are numbered `announcement_N.md`.
- No runs/outputs; `_site/` is the build output (gitignored).
- Commit messages are short and informal (e.g. "new headshot").

## Gotchas
- `_config.yml` `scholar.last_name: Einstein` is still the al-folio default, so the site's own name is not highlighted in author lists — set it to `Mahesh` (and `first_name`) if touching publications.
- `papers.bib` still contains ~7 Einstein placeholder entries (only visible on the hidden Publications page).
- `.github/workflows/deploy-image.yml` / `deploy-docker-tag.yml` are upstream Docker-Hub workflows; `deploy-image` is gated to `repository_owner == 'alshedivat'`, `deploy-docker-tag` runs on `v*` tags.
- `bin/deploy` (gh-pages branch push) is the legacy al-folio deploy; the live site uses the Pages Actions workflow. A stale `origin/gh-pages` branch exists.
- Profile image and publication previews are large (`Karan_Mahesh.png` 2.9 MB, `publication_preview/` 12 MB; one file name contains a space: `intro_fig_heron (1).png`). `imagemagick.enabled: false`.
- `resources/announcement_2.md` and `_pages/announcement_1.md` are stray duplicates of news items.

## Don't touch
- Theme internals above; `_site/`, `.jekyll-cache/`, `vendor/`, `Gemfile.lock` (generated).
- `google59db35dd7b770187.html` (verification token file — must stay verbatim).

## Related
- Live site: https://maheshkaran.github.io; remote `git@github.com:maheshkaran/maheshkaran.github.io.git`.
- Upstream theme: https://github.com/alshedivat/al-folio.
- No project-scoped memories in `~/hub/memory/` as of 2026-09-27.
