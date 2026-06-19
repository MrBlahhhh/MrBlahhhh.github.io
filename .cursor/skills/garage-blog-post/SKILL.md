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
| URL pattern | `/{category}/YYYY/MM/DD/{slug}.html` |

**Do not** use `theme: jekyll-text-theme` gem — GitHub Pages build does not include it. Always use `remote_theme` with the **`v` prefix** on the tag (`@v2.2.6`, not `@2.2.6`).

## Workflow

1. Create branch: `cursor/<short-description>-e418`
2. Add post: `_posts/YYYY-MM-DD-slug.md`
3. Add images: `assets/images/<slug>/` — use the author's original filenames
4. Open PR to `main` — GitHub Actions deploys automatically
5. Confirm Actions build is green before merging

## New post checklist

- [ ] **New file only** — never append to an existing post
- [ ] Filename: `_posts/YYYY-MM-DD-short-slug.md`
- [ ] Date at **midnight** or early morning (`00:00:00 -0400`) so Jekyll does not treat it as a future post
- [ ] Homepage excerpt in front matter (`excerpt:`) — **not** as body text before `<!--more-->`
- [ ] `article_header` uses `overlay` with `background_image: false` (title band, no hero photo)
- [ ] Article body starts with `<!--more-->`, then `##` heading, then content
- [ ] Quote **numeric tags** in YAML: `"11553453300"` not `11553453300`
- [ ] Sections: heading → intro text → images → more detail (see layout rules below)
- [ ] Images constrained to ~420px wide
- [ ] PR created and build passes

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
  background_image: false
---

<!--more-->

## First section

Intro paragraph for this section.

![Alt text](/assets/images/SLUG/photo.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Caption in italics below the image.*

More detail after the photo.
```

## Article page layout (critical)

TeXt needs an `article_header` block to show the title, date, and tags on the article page. Use **overlay with no background image** — omitting `article_header` hides the title, and a background image makes a giant hero.

```yaml
article_header:
  type: overlay
  theme: dark
  background_image: false
```

`cover:` stays in front matter for homepage/archive thumbnails only. `background_image: false` prevents TeXt from using `cover` as the article hero.

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
| `overlay` + `background_image: false` | Title, date, tags band — **use this** |
| `overlay` + `background_image.src` or omitted (with `cover:` set) | Giant hero; excerpt text overlaid on photo |
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

```markdown
![Alt](/assets/images/SLUG/photo.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Caption.*
```

- `max-width:420px` — default for repair/detail shots
- `max-width:640px` — wide shots if needed
- Image on its own line, caption on the next line in italics, then body text
- Do **not** use `{:.text-center}` block attributes after captions — they can break layout in TeXt

## Writing style

Match existing posts:

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
| Text stacked on / under images | `overlay` with background image, or body text before `<!--more-->` | Use `background_image: false`; put excerpt in front matter |
| Giant image before title | `cover` type, or `overlay` using `cover` as background | Use `overlay` with `background_image: false` |
| No visible title | `article_header` omitted | Add `overlay` header with `background_image: false` |

## Categories and tags

- `categories: car` → URL starts with `/car/`
- `categories: car tech` → URL `/car/tech/...`
- Keep tags as lowercase strings; quote anything that looks like a number

## Example posts in repo

| Post | Use as reference for |
|------|---------------------|
| `_posts/2026-06-18-trackday-pyrometer-helper.md` | App/hardware writeup, photo order, parts list |
| `_posts/2026-06-18-bmw-prop-driveshaft-seal-replacement.md` | Repair procedure, image sizing |
| `_posts/2025-10-15-135i-l92-track-build.md` | Long build writeup, multiple photos |

## When user provides photos

1. Upload to `assets/images/<slug>/` keeping original filenames
2. Match filenames exactly in the post markdown
3. Order photos to match the author's narrative

The agent should produce a **new** `_posts/` file, image folder, and PR — not edit existing posts.
