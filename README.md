# AstroBlog — Astro + Decap CMS

A static blog built with [Astro](https://astro.build) and [Decap CMS](https://decapcms.org). Blog posts are Markdown files stored in Git — no database required. Editors manage content through a visual UI at `/admin`.

---

## Project Structure

```
/
├── public/
│   ├── admin/
│   │   ├── config.yml        ← Decap CMS configuration (backend, collections)
│   │   └── (index.html is at src/pages/admin.html)
│   └── images/               ← uploaded media files land here
├── src/
│   ├── content/
│   │   └── blog/             ← blog posts as .md files (Decap CMS writes here)
│   ├── content.config.ts     ← Astro content collection schema
│   ├── layouts/
│   │   ├── BaseLayout.astro
│   │   └── BlogLayout.astro
│   ├── components/
│   │   ├── Header.astro
│   │   ├── Footer.astro
│   │   └── BlogCard.astro
│   └── pages/
│       ├── index.astro           ← home page
│       ├── admin.html            ← Decap CMS editor UI (served at /admin)
│       └── blog/
│           ├── index.astro       ← blog listing
│           └── [...slug].astro   ← individual post pages
└── package.json
```

---

## Prerequisites

- Node.js >= 22
- npm

Install dependencies:

```bash
npm install
```

---

## Running Locally

Local development requires **two terminals running at the same time** — one for the Astro frontend and one for the Decap CMS backend proxy.

### Terminal 1 — Frontend (Astro dev server)

```bash
npm run dev
```

Site is available at `http://localhost:4321`

### Terminal 2 — Backend (Decap CMS local proxy)

```bash
npx decap-server
```

This starts a local proxy at `http://localhost:8081` that allows the CMS editor to read and write files in your local `src/content/blog/` folder without needing Netlify credentials.

Once both are running, open `http://localhost:4321/admin` to use the CMS editor.

---

## config.yml — Local vs Production

The file `public/admin/config.yml` has two settings that must be toggled depending on whether you are running locally or deploying to production.

### For local development

Uncomment `local_backend: true` so the CMS uses the local proxy instead of Netlify:

```yaml
# backend is always required. local_backend: true overrides it for local dev.
backend:
  name: git-gateway
  branch: main

# Enables the local proxy (run `npx decap-server` in a second terminal).
# Remove this line before deploying to production.
local_backend: true        # <-- UNCOMMENT THIS for local dev
```

### For production (before pushing / deploying)

Comment out `local_backend: true` so the CMS uses Netlify Identity + Git Gateway:

```yaml
# backend is always required. local_backend: true overrides it for local dev.
backend:
  name: git-gateway
  branch: main

# Enables the local proxy (run `npx decap-server` in a second terminal).
# Remove this line before deploying to production.
# local_backend: true      # <-- COMMENT THIS OUT for production
```

> **Never push `local_backend: true` to production.** It would allow anyone to edit content without authentication.

---

## Other Commands

| Command             | Action                                      |
| :------------------ | :------------------------------------------ |
| `npm run dev`       | Start Astro dev server at `localhost:4321`  |
| `npm run build`     | Build production site to `./dist/`          |
| `npm run preview`   | Preview the production build locally        |
| `npx decap-server`  | Start Decap CMS local proxy (port 8081)     |

---

## Deploying to Netlify

1. Push the repo to GitHub
2. Connect repo in [Netlify](https://app.netlify.com) — build command: `npm run build`, publish dir: `dist`
3. In Netlify: **Site configuration → Identity → Enable Identity**
4. Under **Identity → Services → Enable Git Gateway**
5. Under **Identity → Registration → set to Invite only**
6. Invite editors via **Identity → Invite users**
7. Make sure `local_backend: true` is commented out in `config.yml` before deploying

---

## Adding Blog Posts

- **Via CMS editor:** Go to `/admin`, log in, click "Blog Posts" → "New Blog Post"
- **Via code:** Create a `.md` file directly in `src/content/blog/` with the required frontmatter:

```markdown
---
title: "Your Post Title"
date: 2026-06-04
description: "A short summary of the post."
author: "Your Name"
tags: ["tag1", "tag2"]
draft: false
---

Post content goes here...
```
