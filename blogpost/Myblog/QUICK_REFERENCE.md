# 🚀 Quick Commands Reference

## Common Tasks

### Create a New Post
```powershell
hugo new content posts/post-name.md
```
Then edit the file and set `draft: false` when ready.

### Local Development
```powershell
# View all posts including drafts
hugo server -D

# View only published posts
hugo server
```
Then visit: http://localhost:1313

### Build for Production
```powershell
hugo --minify
```
This creates the `public/` folder ready for deployment.

### Deploy to GitHub
```powershell
git add .
git commit -m "Add post: Post Title"
git push origin main
```
GitHub Actions will automatically build and deploy!

---

## Directory Quick Reference

| Path | Purpose |
|------|---------|
| `content/posts/` | Your blog posts (markdown files) |
| `public/` | Generated static site (don't edit) |
| `themes/PaperMod/` | Theme files (don't edit) |
| `static/` | Images and static assets |
| `hugo.yaml` | Main configuration file |

---

## Post Template Reminder

```markdown
---
title: "Your Post Title"
date: 2026-03-09T10:30:00Z
draft: false
author: Your Name
description: "2-3 sentence summary"
categories:
  - AI  # or dotnet, or add new
  - Category2
tags:
  - tag1
  - tag2
---

<!--more-->

## Your Post Content

Content here...
```

**Key reminders:**
- `draft: false` to publish
- Use `<!--more-->` to set preview length
- **Categories are automatic!** Any category name works (AI, dotnet, Cloud, etc.)
- 3-5 relevant tags per post

---

## About Categories

Categories are **automatically managed** - just add them to your post! No menu configuration needed.

When you use a category like `categories: [Cloud]`, Hugo automatically creates:
- ✅ A category page: `/categories/cloud/`
- ✅ Category listings
- ✅ Clickable category links on posts
- ✅ Categories menu shows all available categories

---

## Troubleshooting

**Post not showing?**
- [ ] Set `draft: false`
- [ ] Check date is not in future
- [ ] Delete `public/` and rebuild

**Local server not starting?**
```powershell
# Clear cache
Remove-Item -Recurse resources/

# Rebuild
hugo server -D
```

**Theme issues?**
```powershell
# Update theme
git submodule update --remote

# Or manually delete and clone latest
```

---

## GitHub Pages Setup

1. Create GitHub repository: `yourusername/your-blog-repo`
2. Push your code to `main` branch
3. Go to repository Settings → Pages
4. Set source to Deploy from a branch
5. Select `gh-pages` branch
6. Site will be live at: `https://yourusername.github.io/your-blog-repo/`

**Already using GitHub Actions workflow?** ✅ Auto-deploy enabled!

---

Happy blogging! 📝
