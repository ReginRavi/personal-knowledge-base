# Personal Knowledge Base

A Hugo-based static site for documenting notes, experiments, and deep dives into Kubernetes, Terraform, and AWS.

## 🚀 Quick Start

### Prerequisites
- [Hugo](https://gohugo.io/) v0.155.2 or later
- Git

### Local Development

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/personal-knowledge-base.git
cd personal-knowledge-base

# Initialize theme submodule
git submodule update --init --recursive

# Start development server
hugo server -D

# Visit http://localhost:1313/personal-knowledge-base/
```

## 📝 Adding Content

### Create a New Post

```bash
# Kubernetes post
hugo new kubernetes/my-new-post.md

# Terraform post
hugo new terraform/my-new-post.md

# AWS post
hugo new aws/my-new-post.md
```

### Edit Post Frontmatter

```yaml
---
title: "Your Post Title"
date: 2026-02-07T12:00:00+05:30
tags: ["tag1", "tag2"]
draft: false  # Set to false when ready to publish
---

Your content here...
```

### Commit and Deploy

```bash
git add .
git commit -m "Add new post"
git push
```

The site automatically deploys via GitHub Actions! 🎉

## 📂 Project Structure

```
personal-knowledge-base/
├── content/
│   ├── kubernetes/     # Kubernetes notes
│   ├── terraform/      # Terraform notes
│   └── aws/           # AWS notes
├── themes/
│   └── PaperMod/      # Theme (git submodule)
├── .github/
│   └── workflows/
│       └── deploy.yml # CI/CD pipeline
└── hugo.toml          # Site configuration
```

## 🎨 Features

- ✅ Clean, modern design with PaperMod theme
- ✅ Automatic dark/light mode
- ✅ Reading time estimation
- ✅ Table of contents
- ✅ Code syntax highlighting with copy buttons
- ✅ Responsive design
- ✅ Automatic deployment to GitHub Pages
- ✅ RSS feed support

## 🌐 Deployment

This site is automatically deployed to GitHub Pages using GitHub Actions.

**Live Site**: `https://YOUR_USERNAME.github.io/personal-knowledge-base/`

### Setup GitHub Pages

1. Go to repository **Settings** → **Pages**
2. Under **Source**, select **GitHub Actions**
3. Push to main branch to trigger deployment

## 🛠 Configuration

Edit `hugo.toml` to customize:
- Site title and description
- Base URL
- Navigation menu
- Theme settings

## 📚 Content Sections

### Kubernetes
Notes, experiments, and deep dives into Kubernetes architecture, operations, and best practices.

### Terraform
Infrastructure as Code with Terraform - modules, state management, and automation patterns.

### AWS
Amazon Web Services notes covering IAM, EC2, S3, and cloud architecture best practices.

## 🔧 Troubleshooting

### Theme not loading?
```bash
git submodule update --init --recursive
```

### Build errors?
Check Hugo version:
```bash
hugo version
```

## 📖 Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [PaperMod Theme](https://github.com/adityatelange/hugo-PaperMod)
- [Markdown Guide](https://www.markdownguide.org/)

## 📄 License

This project is open source and available under the MIT License.

---

Built with [Hugo](https://gohugo.io/) and [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
