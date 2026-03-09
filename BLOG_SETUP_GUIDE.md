# Hugo PaperMod Blog Setup & Maintenance Guide

## 📋 Initial Setup & Configuration

### 1. **Update Hugo Configuration (hugo.yaml)**

Your `hugo.yaml` has been configured with:
- ✅ GitHub Pages baseURL (update `yourusername` and `your-blog-repo`)
- ✅ PaperMod theme settings
- ✅ Menu with AI, .NET, and Tags categories
- ✅ SEO and social media settings

**To customize:**
- Update `baseURL` with your GitHub Pages URL: `https://yourusername.github.io/repository-name/`
- Update author info, description, and social links
- Add/remove categories in the `menu.main` section

### 2. **GitHub Pages Configuration**

#### Option A: Project Site (Recommended for multiple projects)
```
Repository: yourusername/your-blog-repo/
Branch: main (store source)
Branch: gh-pages (auto-generated public folder)
```

---

**Note on Categories:** The menu has been simplified to avoid template conflicts. Individual categories (AI, .NET, etc.) are accessible through:
- **Categories menu** → Browse all categories
- **Category tags in posts** → Click on post category links
- **Direct URL** → `https://yoursite.com/categories/ai/`

#### Option B: User/Organization Site
```
Repository: yourusername.github.io
All content goes directly in main branch
```

### 3. **Create `.github/workflows/hugo.yml` for Auto-Deployment**

Create this file in your repository root:

```yaml
name: Hugo Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        submodules: true
        fetch-depth: 0

    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v4
      with:
        hugo-version: 'latest'
        extended: true

    - name: Build
      run: hugo --minify

    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      if: github.ref == 'refs/heads/main'
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./public
        cname: yourdomain.com  # Optional: if using custom domain
```

---

## ✍️ Adding New Blog Posts

### Step 1: Create a New Post

**Option A: Using Hugo Command (Recommended)**
```powershell
hugo new content posts/my-new-post.md
```

**Option B: Manual Creation**
Create a file: `content/posts/post-name.md`

### Step 2: Front Matter Template

Each post must have proper front matter:

```markdown
---
title: "Your Post Title"
date: 2026-03-09T10:30:00Z
draft: false
author: Your Name
description: "Brief description for SEO (150-160 characters)"
categories:
  - AI
  - dotnet
tags:
  - tag1
  - tag2
  - tag3
---

<!--more-->

Your content starts here...
```

### Step 3: Available Categories

Currently configured categories in menu:
- **AI** - Artificial Intelligence, Machine Learning, LLMs
- **dotnet** - .NET, C#, ASP.NET Core
- Add more in `hugo.yaml` menu section and content posts

### Step 4: Write Your Content

```markdown
## Section 1

Your content here with **bold**, *italic*, `code`

## Section 2

You can embed:
- [Links](https://example.com)
- Images: ![alt text](image.jpg)
- Code blocks:

\`\`\`csharp
var result = await service.GetData();
\`\`\`

## Conclusion

Wrap up your post.
```

---

## 🚀 Publishing Workflow

### Local Testing

1. **Start Hugo development server:**
```powershell
hugo server -D
```
- `-D` flag shows draft posts
- Navigate to `http://localhost:1313`

2. **Build for production:**
```powershell
hugo --minify
```
- Creates optimized `public/` folder

### Publishing to GitHub Pages

1. **Commit and push:**
```powershell
git add .
git commit -m "Add new post: Post Title"
git push origin main
```

2. **GitHub Actions automatically:**
   - Builds the site
   - Deploys to `gh-pages` branch
   - Site appears at your GitHub Pages URL in 1-2 minutes

---

## 📁 Project Structure

```
content/
├── posts/
│   ├── MCP (Model Context Protocol).md    # Your AI post
│   ├── dotnet-post.md                     # .NET category
│   └── another-post.md
└── _index.md

themes/
└── PaperMod/                              # Theme files (don't edit)

public/                                    # Auto-generated (don't commit)

hugo.yaml                                  # Main configuration

.github/
└── workflows/
    └── hugo.yml                           # CI/CD pipeline
```

---

## 🎯 Post Best Practices

### 1. **Naming Convention**
- Use lowercase with hyphens: `my-post-title.md`
- Or use natural names: `MCP (Model Context Protocol).md`

### 2. **Front Matter Checklist**
- ✅ `draft: false` for publishing
- ✅ `date:` in ISO format
- ✅ `categories:` (choose from AI, dotnet, or add new ones)
- ✅ `tags:` (3-5 relevant tags)
- ✅ `description:` (150-160 chars for SEO)

### 3. **Content Tips**
- Start with introduction/summary
- Use `<!--more-->` after intro for preview
- Include code examples
- Add relevant images
- Use header hierarchy (# ## ###)
- Link to related posts

---

## 🔧 Maintenance Checklist

### Weekly
- [ ] Write new posts
- [ ] Review analytics (if enabled)

### Monthly
- [ ] Update author info if needed
- [ ] Review and fix any broken links
- [ ] Check for theme updates

### Before Publishing
- [ ] Proofread post content
- [ ] Verify `draft: false` is set
- [ ] Test links in post
- [ ] Check category/tags are spelled correctly
- [ ] Run `hugo server` locally to verify rendering

---

## 📝 Adding New Categories

Categories are **automatically managed** - just use them in your posts!

To add a new category (e.g., "Cloud Architecture"):

### 1. Use in Posts:
Simply add the category to your post frontmatter:
```yaml
categories:
  - Cloud
```

### 2. Categories will automatically appear:
- **Categories Menu** → Shows all available categories
- **Category Pages** → `https://yoursitename.com/categories/cloud/`
- **All categories** → `https://yoursitename.com/categories/`
- **On individual posts** → As clickable category tags

**Note:** You do NOT need to edit the menu for individual categories. The Categories menu item gives you access to all categories automatically!

---

## 🐛 Troubleshooting

### Post Not Showing
1. Check `draft: false`
2. Verify `date:` is in past or current
3. Confirm file is in `content/posts/` folder

### Links Broken
- Use relative paths: `/posts/` not `http://...`
- Check file names match exactly

### Images Not Showing
1. Place in `static/` folder: `static/images/photo.jpg`
2. Reference: `![alt](/images/photo.jpg)`

### Theme Not Updating
1. Delete `public/` folder
2. Run `hugo --minify` again

---

## 📚 Useful Resources

- **Hugo Docs:** https://gohugo.io/documentation/
- **PaperMod Theme:** https://github.com/adityatelange/hugo-PaperMod
- **Markdown Guide:** https://www.markdownguide.org/
- **GitHub Pages Help:** https://pages.github.com/

---

## ✨ Next Steps

1. **Update baseURL** in `hugo.yaml` with your GitHub Pages URL
2. **Create `.github/workflows/hugo.yml`** for auto-deployment
3. **Push to GitHub** and enable GitHub Pages in Settings
4. **Test locally:** `hugo server -D`
5. **Write your next post!**

Happy blogging! 🎉
