---
title: "What is Decap CMS and Why Use It?"
date: 2026-05-22
description: "Decap CMS is an open-source, Git-based content management system. Learn how it works, what makes it different, and why it pairs beautifully with Astro."
author: "Shubham Acharya"
tags: ["decap cms", "cms", "git", "content management"]
draft: false
---

## What is Decap CMS?

**Decap CMS** (formerly known as Netlify CMS) is an open-source, **Git-based content management system**. Instead of storing your content in a database, it saves everything as files directly in your Git repository — Markdown files, JSON, YAML, whatever you choose.

That means your content is versioned, backed up, and editable by developers just like code — but non-developers get a clean visual editor at `/admin`.

## How Does It Work?

1. **Editor opens `/admin`** in the browser
2. **Decap CMS authenticates** via Netlify Identity (or another OAuth provider)
3. **Editor creates or edits a post** using the visual form UI
4. **Decap CMS commits the Markdown file** directly to your Git repo
5. **Your CI/CD pipeline** (Netlify, Vercel, GitHub Actions) rebuilds and deploys

No database. No backend. Just files in Git.

## Key Concepts

### Backend
Decap CMS needs a "backend" to read and write your repo. The most common is **Git Gateway** (provided by Netlify), but it also supports GitHub, GitLab, and Bitbucket directly.

### Collections
A **collection** defines a type of content — like "Blog Posts". You configure it in `config.yml`:

```yaml
collections:
  - name: "blog"
    label: "Blog Posts"
    folder: "src/content/blog"
    create: true
    fields:
      - { label: "Title", name: "title", widget: "string" }
      - { label: "Body", name: "body", widget: "markdown" }
```

### Widgets
Each field in a collection uses a **widget** — a UI element in the editor. Common widgets include `string`, `text`, `markdown`, `datetime`, `image`, `boolean`, and `list`.

## Why Use It With Astro?

Astro already stores content as Markdown files in `src/content/`. Decap CMS writes Markdown files to the same folder. They're a **perfect match** — Astro reads what Decap writes, and your editors never need to touch code.

| Feature | Decap CMS |
|---|---|
| Database required | No |
| Developer-friendly | Yes — everything in Git |
| Non-dev editor UI | Yes — visual form at `/admin` |
| Cost | Free (open source) |
| Hosting lock-in | None |

## The Bottom Line

If you want to give non-technical team members the ability to write and publish blog posts — without touching code, without a database, and without paying for a headless CMS — **Decap CMS is the answer**.
