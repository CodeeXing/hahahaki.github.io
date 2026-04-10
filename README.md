# Personal academic website — Cheng Xing

Source for my personal academic website, built with plain Jekyll and hosted
on GitHub Pages.

## Structure

```
_config.yml             # Site title, nav, contact links
_layouts/default.html   # Single layout shared by all pages
assets/css/style.scss   # All styling (hand-written, no theme)
assets/images/          # Profile photo etc.
index.md                # Home (About + Research + Education + Contact)
publications.md         # Publications + Academic Awards
teaching.md             # Mentoring, Talks, News
```

## Editing content

- **Bio / research / education:** edit `index.md`.
- **Papers and awards:** edit `publications.md`.
- **Mentoring, talks, news:** edit `teaching.md`.
- **Nav links, contact info, site title:** edit `_config.yml`.
- **Profile photo:** replace `assets/images/profile.jpg` with your own image
  (keep the same filename, or update the path in `index.md`).

Anything marked `TODO` in the source is a placeholder meant to be filled in
(email, Google Scholar ID, LinkedIn URL, profile photo).

## Local preview

```bash
bundle init
bundle add jekyll jekyll-seo-tag
bundle exec jekyll serve
```

Then open <http://localhost:4000>.

## Branch

Active development branch: `claude/setup-academic-website-jvALe`.
