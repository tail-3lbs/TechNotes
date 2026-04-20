# Jiaqi's Tech Notes

Quarto-based public notes/blog site for technical notes, math notes, and longer-form writeups.

## Purpose

This repo builds a static website whose official name is `Jiaqi's Tech Notes`. The homepage lists note cards, and each note lives in its own folder under `notes/`.

The current scaffold is intentionally simple:

- Quarto website project
- Homepage generated from note metadata
- Shared site-level defaults
- One sample post
- Lightweight custom styling

## Repo Layout

```text
.
├── _metadata.yml        # Shared defaults for pages/posts
├── _quarto.yml          # Quarto website config
├── index.qmd            # Homepage: "Jiaqi's Tech Notes"
├── notes/
│   └── ppo-vs-grpo/
│       ├── index.qmd    # Note page
│       ├── images/      # Note-local images
│       └── videos/      # Note-local videos
├── styles.css           # Site styling
└── _site/               # Generated output (ignored by git)
```

## Local Development

Preview locally with live reload:

```bash
quarto preview
```

If your Linux desktop shows a GTK browser-launch warning, use:

```bash
make preview
```

That runs `quarto preview --no-browser`, which avoids the auto-open step. Then open the printed local URL yourself.

Build the static site once:

```bash
quarto render
```

Generated output is written to `_site/`.

Optional helper commands:

```bash
make render
make clean
```

## GitHub Pages Hosting

This repo is configured to publish with GitHub Actions using [.github/workflows/publish.yml](/home/jiaqi/workspace/tech_notes_repo/.github/workflows/publish.yml:1).

How publishing works:

1. Push the repo to GitHub.
2. GitHub Actions runs `quarto render`.
3. The workflow uploads `_site/`.
4. GitHub Pages serves the uploaded artifact.

### One-Time GitHub Setup

After creating the GitHub repo and pushing this code:

1. Open the repo on GitHub.
2. Go to `Settings` -> `Pages`.
3. Under `Build and deployment`, set `Source` to `GitHub Actions`.

That is the key setting. After that, every push to the `master` branch will trigger a fresh site deployment.

### Site URL

The expected Pages URL is:

```text
https://tail-3lbs.github.io/TechNotes/
```

### Google Search Console

After the site is deployed, register the site in Google Search Console as a `URL prefix` property using:

```text
https://tail-3lbs.github.io/TechNotes/
```

Recommended verification options for this repo:

1. `HTML file` upload
2. `HTML tag`

`HTML file` works well with this Quarto/GitHub Pages setup because the verification file can be committed at the repo root and published as a static asset.

`HTML tag` also works, but it requires adding the verification meta tag into the rendered page head.

### Indexing Checklist

Use this checklist after a deploy:

1. Confirm the site is live at `https://tail-3lbs.github.io/TechNotes/`.
2. Confirm `https://tail-3lbs.github.io/TechNotes/robots.txt` loads.
3. Confirm `https://tail-3lbs.github.io/TechNotes/sitemap.xml` loads.
4. Open Google Search Console and verify the `URL prefix` property.
5. Submit `https://tail-3lbs.github.io/TechNotes/sitemap.xml` in Search Console.
6. Use `URL Inspection` on the homepage and one or two note pages.
7. If Google reports the pages are not indexed yet, click `Request Indexing`.

### Search Visibility Notes

- Indexing is not immediate. It often takes days, sometimes longer.
- A successful deploy and a valid sitemap improve crawlability, not ranking.
- Very new GitHub Pages sites may not appear in Google results until Google recrawls them.
- If you later move to a custom domain, add the new domain as its own Search Console property.

## Adding a New Note

1. Create a new folder under `notes/`, for example `notes/my-new-note/`.
2. Put the page at `notes/my-new-note/index.qmd`.
3. Add `images/` and `videos/` inside that folder if needed.
4. Add front matter with the required metadata.
5. Write the body in normal Quarto/Markdown.
6. Run `quarto preview` and check the homepage card and note page.

Suggested front matter:

```text
notes/
  your-note-slug/
    index.qmd
    images/
    videos/
```

```yaml
---
title: "Your Note Title"
date: 2026-04-19
date-modified: 2026-04-19
categories:
  - Tag1
  - Tag2
description: "Short teaser text for the homepage card."
---
```

## Metadata Conventions

This scaffold uses Quarto-native fields so the homepage listing works without custom code:

- `title`: note title
- `date`: manual "created at"
- `date-modified`: manual "last edit at"
- `categories`: tags
- `description`: teaser text

If you want to display extra metadata inside the note body, add it directly to the page content or extend the template later.

## Content Features

Posts currently support:

- Markdown / Quarto authoring
- LaTeX math
- Local images
- Local or remote video embeds

Example patterns:

```markdown
![Figure](images/example.png)
```

```html
<video controls width="100%">
  <source src="videos/example.mp4" type="video/mp4">
</video>
```

## Homepage Behavior

The homepage is defined in `index.qmd` and automatically lists all files matching `notes/*/index.qmd`.

Current behavior:

- Grid/card layout
- Sorted by `date-modified` descending, then `date` descending
- Shows title, created date, modified date, teaser, and tags

## Styling

Site styling lives in `styles.css`.

Keep the visual direction clean and readable. If the site grows, consider moving to SCSS and splitting homepage vs post styles.

## Git Notes

- `_site/` is generated and ignored
- `.quarto/` is local build state and ignored
- Source files to commit are the `.qmd`, `.yml`, `.css`, and note-local asset files

## Next Likely Steps

- Add a reusable post template or starter file
- Add richer homepage card styling
- Add tag filtering/search if the note count grows
