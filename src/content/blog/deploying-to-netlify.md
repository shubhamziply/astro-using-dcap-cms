---
title: "Deploying Your Astro + Decap CMS Site to Netlify"
date: 2026-05-30
description: "A step-by-step guide to deploying your Astro site with Decap CMS to Netlify, including setting up Netlify Identity so your editors can log in."
author: "Shubham Acharya"
tags: ["netlify", "deployment", "decap cms", "ci/cd"]
draft: false
---

## Overview

Netlify is the easiest place to host an Astro + Decap CMS site because it provides **Netlify Identity** and **Git Gateway** — the two services that Decap CMS needs to authenticate users and commit files to your repo.

## Step 1: Push Your Code to GitHub

Decap CMS is Git-based, so your site must live in a GitHub, GitLab, or Bitbucket repository.

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main
```

## Step 2: Connect Netlify to Your Repo

1. Log in to [netlify.com](https://netlify.com)
2. Click **"Add new site" → "Import an existing project"**
3. Choose GitHub and select your repository
4. Set the **build command**: `npm run build`
5. Set the **publish directory**: `dist`
6. Click **Deploy site**

Netlify will build and deploy your site automatically on every push.

## Step 3: Enable Netlify Identity

1. In your Netlify dashboard, go to **Site Settings → Identity**
2. Click **"Enable Identity"**
3. Under **Registration preferences**, set to **Invite only** (so random people can't sign up)
4. Under **Services → Git Gateway**, click **"Enable Git Gateway"**

This is what allows Decap CMS to commit files to your repo on behalf of editors.

## Step 4: Invite Your Editors

1. Go to **Identity → Invite users**
2. Enter the email addresses of your editors
3. They'll receive an email to set a password

## Step 5: Update Your Decap CMS Config

Make sure your `public/admin/config.yml` uses the `git-gateway` backend:

```yaml
backend:
  name: git-gateway
  branch: main

media_folder: "public/images"
public_folder: "/images"

collections:
  - name: "blog"
    label: "Blog Posts"
    folder: "src/content/blog"
    create: true
    # ...
```

## Step 6: Add the Netlify Identity Widget

Your `public/admin/index.html` should already include the Decap CMS script. Also add the Identity widget script to your main site layout so editors can confirm their accounts:

```html
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
```

And this redirect snippet:

```html
<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("init", user => {
      if (!user) {
        window.netlifyIdentity.on("login", () => {
          document.location.href = "/admin/";
        });
      }
    });
  }
</script>
```

## That's It!

Push your changes, Netlify rebuilds, and your editors can now visit `https://yoursite.netlify.app/admin/` to log in and write posts. Every save creates a real Git commit — you have a full history of every content change.
