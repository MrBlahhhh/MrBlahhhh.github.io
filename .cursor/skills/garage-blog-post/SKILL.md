---
name: garage-blog-post
description: Create and publish blog posts for mrblahhhh.github.io (Garage & Code). Use when writing car builds, repair writeups, embedded/telemetry projects, or any new Jekyll post for this GitHub Pages site.
paths: _posts/**, assets/images/**
---

# Garage & Code — Blog Post Skill

Create new posts for **https://mrblahhhh.github.io** — a Jekyll site using the **TeXt** theme on GitHub Pages.

## Site facts

| Item | Value |
|------|-------|
| Repo | `MrBlahhhh/MrBlahhhh.github.io` |
| Theme | `remote_theme: kitian616/jekyll-TeXt-theme@v2.2.6` |
| Timezone | `America/New_York` |
| Post layout | `article` (set via `_config.yml` defaults) |
| Homepage | cover-image card grid (`index.html`, `articles` layout) — every post **must** have a `cover:` or its card has no photo |
| Site CSS/helpers | `_includes/head/custom.html` (image classes, card styles) |
| URL pattern | `/{category}/YYYY/MM/DD/{slug}.html` |

**Do not** use `theme: jekyll-text-theme` gem — GitHub Pages build does not include it. Always use `remote_theme` with the **`v` prefix** on the tag (`@v2.2.6`, not `@2.2.6`).

## Workflow

1. Work on **`main`** — commit and push directly; do **not** create feature branches or PRs unless explicitly asked
2. Add post: `_posts/YYYY-MM-DD-slug.md`
3. Add images: `assets/images/<slug>/` — use the author's original filenames
4. `git push origin main` — GitHub Actions deploys automatically
5. Confirm Actions build is green after push

## New post checklist

- [ ] **New file only** — never append to an existing post
- [ ] Filename: `_posts/YYYY-MM-DD-short-slug.md`
- [ ] Date at **midnight** or early morning (`00:00:00 -0400`) so Jekyll does not treat it as a future post
- [ ] Homepage excerpt in front matter (`excerpt:`) — **not** as body text before `<!--more-->`
- [ ] `article_header` uses `overlay` with a **gradient + the cover photo** as `background_image` (hero with readable title — see layout rules below)
- [ ] `cover:` set — it is used **twice**: homepage grid card thumbnail *and* article hero background
- [ ] Article body starts with `<!--more-->`, then `##` heading, then content
- [ ] Quote **numeric tags** in YAML: `"11553453300"` not `11553453300`
- [ ] Sections: heading → intro text → images → more detail (see layout rules below)
- [ ] Images sized with the site's helper classes: `{:.img-md}` default, `{:.img-lg}` for wide shots
- [ ] Committed and pushed to `main`; Actions build passes

## Post template

Copy and fill in. See `references/post-template.md` for a ready-to-edit file.

```yaml
---
title: "Your Post Title"
date: YYYY-MM-DD 00:00:00 -0400
categories: car
tags: [bmw, keyword-one, keyword-two]
cover: /assets/images/SLUG/hero.jpg
lightbox: true
excerpt: "One-line summary for the home page only."
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/SLUG/hero.jpg
---

<!--more-->

## First section

Intro paragraph for this section.

![Alt text](/assets/images/SLUG/photo.jpg){:.img-md}
*Caption in italics below the image.*

More detail after the photo.
```

`background_image.src` should be the **same image as `cover:`** — pick the best photo of the project; it becomes both the homepage card and the article hero.

## Article page layout (critical)

TeXt needs an `article_header` block to show the title, date, and tags on the article page. The site standard (since 2026-07) is an **overlay hero using the cover photo behind a dark gradient** — the gradient keeps the title/date/tags readable over any photo.

```yaml
article_header:
  type: overlay
  theme: dark
  background_color: "#1f1f1f"
  background_image:
    gradient: "linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))"
    src: /assets/images/SLUG/hero.jpg
```

Rules:

- `src` = the same file as `cover:`. Keep the gradient — a bare `src` without it makes the title unreadable on bright photos.
- Do **not** use `background_image: false` anymore (that was the old flat title band).
- Do **not** use `type: cover` (puts a giant image *above* the title instead of behind it).
- Omitting `article_header` entirely hides the title on the article page.

### Excerpt

Put the homepage excerpt in front matter:

```yaml
excerpt: "Short summary for the archive and home page."
```

Then start the markdown body with `<!--more-->` immediately. **Do not** put paragraphs before `<!--more-->` in the body — Jekyll includes that text at the top of the full article page, above your first heading and images.

### Section order

Within each section:

1. `##` or `###` heading
2. Intro paragraph (optional)
3. Image(s) with caption on the next line
4. Remaining explanatory text

Do **not** put images before the section heading. Do **not** put the first image before the first `##` heading in the body.

### `article_header` — safe vs broken

| Config | Result |
|------|---------|
| `overlay` + `background_image.gradient` + `background_image.src` | Photo hero with readable title — **use this** |
| `overlay` + `background_image: false` | Flat title band, no photo — old style, don't use for new posts |
| `overlay` + `src` without `gradient` | Title can be unreadable over bright photos |
| `cover` type | Giant full-width image before the title |
| omitted entirely | Title hidden on article page |

## Images

### Storage

```
assets/images/<post-slug>/
├── hero.jpg
├── step-one.jpg
└── step-two.jpg
```

Use the author's original filenames when provided. `.jpg` preferred.

### Sizing (required)

Use the site's helper classes (defined in `_includes/head/custom.html` — they also give every article image rounded corners and a drop shadow automatically):

```markdown
![Alt](/assets/images/SLUG/photo.jpg){:.img-md}
*Caption.*
```

- `{:.img-md}` (420px) — default for repair/detail shots
- `{:.img-lg}` (640px) — wide shots, engine bays, CAD overviews
- `{:.img-sm}` (320px) — small detail crops
- **Tall portrait phone screenshots: pre-scale the file to ~300px wide before committing** (a 1080×2410 capture at `img-md` renders ~940px tall — too big). Do **not** add new site-wide CSS classes to fix image sizing; resize the files.
- Do **not** hand-write inline `style="max-width:..."` attributes — that was the old pattern; the classes replace it
- Image on its own line, caption on the next line in italics, then body text
- Do **not** use `{:.text-center}` block attributes after captions — they can break layout in TeXt

## Writing style

Match existing posts:

- **First person** — write as the site owner: *I*, *I'm*, *my*. Never use "author" in posts or PR text.
- **Concise and technical** — specs, part numbers, steps
- **No fluff** — skip sales pitch language and cost commentary unless asked
- **Workflow order** — sections and photos follow the order work was actually done
- **Part numbers** in bold: `**11553453300**`
- Short `##` sections, bullet lists for steps

## TeXt features

### Code blocks

```liquid
{% highlight cpp %}
void example() {
    // code here
}
{% endhighlight %}
```

### YouTube

```html
<div>{%- include extensions/youtube.html id='VIDEO_ID' -%}</div>
```

Extract ID from `https://www.youtube.com/watch?v=VIDEO_ID` or `https://www.youtube.com/shorts/VIDEO_ID`.

## Common build failures

| Error | Cause | Fix |
|-------|-------|-----|
| `jekyll-text-theme could not be found` | Using gem theme | Use `remote_theme: kitian616/jekyll-TeXt-theme@v2.2.6` |
| `404` downloading theme zip | Missing `v` on tag | Use `@v2.2.6` not `@2.2.6` |
| Post missing after deploy | Future-dated post | Set date to `00:00:00 -0400` or earlier |
| `comparison of Array with Array failed` in `tags.html` | Unquoted numeric tag | Quote it: `"11553453300"` |
| Text stacked on / under images | Body text before `<!--more-->` | Put excerpt in front matter only |
| Title unreadable on hero photo | `background_image.src` without `gradient` | Add the standard `linear-gradient(rgba(0, 0, 0, .45), rgba(0, 0, 0, .65))` |
| Giant image before title | `cover` type header | Use `overlay` with gradient + `src` |
| No visible title | `article_header` omitted | Add the standard overlay header block |

## Categories and tags

- `categories: car` → URL starts with `/car/`
- `categories: car tech` → URL `/car/tech/...`
- Keep tags as lowercase strings; quote anything that looks like a number

## Example posts in repo

| Post | Use as reference for |
|------|---------------------|
| `_posts/2026-06-18-trackday-pyrometer-helper.md` | App/hardware writeup, photo order, parts list |
| `_posts/2026-06-18-bmw-prop-driveshaft-seal-replacement.md` | Repair procedure |

Note: older posts still size images with inline `style=` attributes; that works but new posts should use the `{:.img-md}` / `{:.img-lg}` classes instead.
| `_posts/2025-10-15-135i-l92-track-build.md` | Long build writeup, multiple photos |

## When user provides photos

1. Upload to `assets/images/<slug>/` keeping original filenames
2. Match filenames exactly in the post markdown
3. Order photos to match the author's narrative

The agent should produce a **new** `_posts/` file and image folder on `main` — not edit existing posts.
