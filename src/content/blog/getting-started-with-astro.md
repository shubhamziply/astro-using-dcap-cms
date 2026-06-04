---
title: "Getting Started with Astro"
date: 2026-05-15
description: "Astro is a modern static site builder that lets you build faster websites with less JavaScript. Learn the basics and get up and running in minutes."
author: "Shubham Acharya"
tags: ["astro", "web development", "javascript"]
draft: false
---

## What is Astro?

Astro is a modern **static site generator** that ships zero JavaScript by default. It lets you build faster websites using your favorite UI components — React, Vue, Svelte, or just plain HTML.

One of Astro's key ideas is **islands architecture**: interactive components are isolated "islands" in an otherwise static HTML page. This means your users only download JavaScript for the parts of the page that actually need it.

## Why Choose Astro?

- **Zero JS by default** — pages are pure HTML unless you opt in
- **Component-friendly** — use React, Vue, Svelte, or just `.astro` files
- **Content-focused** — first-class Markdown and MDX support with Content Collections
- **Fast builds** — incremental builds and smart caching out of the box

## Your First Astro Page

Every `.astro` file is a component. The frontmatter (between `---`) runs at build time on the server:

```astro
---
const greeting = "Hello, Astro!";
---

<h1>{greeting}</h1>
```

That's all there is to it — the `{greeting}` is evaluated at build time, and the output is plain HTML.

## Content Collections

Astro's **Content Collections** API lets you organise and validate your Markdown files using Zod schemas. Every blog post in this site is a collection entry — Astro validates the frontmatter and gives you fully typed data in your pages.

```ts
import { getCollection } from 'astro:content';

const posts = await getCollection('blog');
```

## What's Next?

Now that you have a feel for Astro, try:

1. Adding an integration like Tailwind or React
2. Deploying to Netlify or Vercel
3. Connecting Decap CMS so non-developers can write posts

Happy building!
