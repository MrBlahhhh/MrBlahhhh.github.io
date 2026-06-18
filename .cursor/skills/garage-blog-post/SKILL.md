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

1. Create branch: `cursor/<short-description>-3a4b`
2. Add post: `_posts/YYYY-MM-DD-slug.md`
3. Add images: `assets/images/<slug>/`
4. Open PR to `main` — GitHub Actions deploys automatically
5. Confirm Actions build is green before merging

## New post checklist

- [ ] **New file only** — never append to an existing post
- [ ] Filename: `_posts/YYYY-MM-DD-short-slug.md`
- [ ] Date at **midnight** or early morning (`00:00:00 -0400`) so Jekyll does not treat it as a future post
- [ ] Short intro paragraph, then `<!--more-->` for homepage excerpt
- [ ] Quote **numeric tags** in YAML: `"11553453300"` not `11553453300`
- [ ] Images in workflow order, **before** related text
- [ ] Images constrained to ~420px wide (see template below)
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
article_header:
  type: cover
  align: center
  theme: dark
  image:
    src: /assets/images/SLUG/hero.jpg
---

Short one-line intro for the home page only.

<!--more-->

## First section

![Alt text](/assets/images/SLUG/photo.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Caption in italics below the image.*

Body text for this step.
```

## Images

### Storage

```
assets/images/<post-slug>/
├── hero.jpg
├── step-one.jpg
└── step-two.jpg
```

Use lowercase, hyphenated filenames. `.jpg` preferred.

### Sizing (required)

Full-width images look bad on TeXt. Always use:

```markdown
![Alt](/assets/images/SLUG/photo.jpg){:.border.rounded style="max-width:420px;display:block;margin:1.25rem auto;"}
*Caption.*
```

- `max-width:420px` — good default for repair/detail shots
- `max-width:640px` — hero or wide shots if needed
- Place the image **above** its caption and explanatory text

### Hero / header

Set `cover` and `article_header.background_image.src` to the best wide or action shot. Enable `lightbox: true` for click-to-enlarge.

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

Languages: `cpp`, `bash`, `yaml`, etc.

### YouTube

```html
<div>{%- include extensions/youtube.html id='VIDEO_ID' -%}</div>
```

Extract ID from `https://www.youtube.com/watch?v=VIDEO_ID`.

### Image classes

TeXt supports `{:.border}`, `{:.rounded}`, `{:.shadow}` — combine as `{:.border.rounded}`.

## Common build failures

| Error | Cause | Fix |
|-------|-------|-----|
| `jekyll-text-theme could not be found` | Using gem theme | Use `remote_theme: kitian616/jekyll-TeXt-theme@v2.2.6` |
| `404` downloading theme zip | Missing `v` on tag | Use `@v2.2.6` not `@2.2.6` |
| Post missing after deploy | Future-dated post | Set date to `00:00:00 -0400` or earlier |
| `comparison of Array with Array failed` in `tags.html` | Unquoted numeric tag | Quote it: `"11553453300"` |
| Post URL 404 | Wrong path | Include category: `/car/YYYY/MM/DD/slug.html` |

## Categories and tags

- `categories: car` → URL starts with `/car/`
- `categories: car tech` → URL `/car/tech/...` (avoid unless intentional)
- Keep tags as lowercase strings; quote anything that looks like a number

## Example posts in repo

| Post | Use as reference for |
|------|---------------------|
| `_posts/2025-10-15-135i-l92-track-build.md` | Long build writeup, multiple photos |
| `_posts/2026-06-18-bmw-prop-driveshaft-seal-replacement.md` | Repair procedure, image sizing, workflow order |
| `_posts/2026-06-15-first-blog-post.md` | Code blocks, firmware content |

## When user provides photos

1. Ask them to upload to `assets/images/<slug>/` or upload via PR
2. Match filenames in the post markdown to actual uploaded names
3. If they rename files (e.g. `engine-photo.jpg` vs `engine-bay.jpg`), update all references in the post

## Prompts that work well

> "Make me a new blog post titled X with today's date about Y. Photos attached."

Tell the agent:
- Post title
- Topic / steps / part numbers
- Whether photos are included
- Preferred section order if it matters

The agent should produce a **new** `_posts/` file, image folder, and PR — not edit existing posts.
