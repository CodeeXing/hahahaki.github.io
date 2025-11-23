# Hahahaki's Portfolio Website

Personal portfolio website and blog built with Jekyll and hosted on GitHub Pages.

🌐 **Live Site**: [https://hahahaki.github.io](https://hahahaki.github.io)

## About This Site

This is my personal website showcasing my projects, blog posts, and professional experience. It serves as a central hub for my online presence and a platform to share knowledge with the developer community.

## Features

- **Responsive Design**: Fully responsive layout that works on all devices
- **Project Portfolio**: Showcase of my development projects with detailed descriptions
- **Blog**: Technical articles about web development, programming, and technology
- **About Page**: Information about my background, skills, and experience
- **Contact Information**: Multiple ways to get in touch

## Site Structure

```
hahahaki.github.io/
├── _posts/              # Blog posts
├── _layouts/            # Page layouts
├── _includes/           # Reusable components
├── assets/              # CSS, images, and other assets
├── index.md             # Homepage
├── about.md             # About page
├── projects.md          # Projects portfolio
├── blog.md              # Blog index
├── contact.md           # Contact information
└── _config.yml          # Site configuration
```

## Technology Stack

- **Framework**: Jekyll (Static Site Generator)
- **Theme**: Tactile (Customized)
- **Hosting**: GitHub Pages
- **Markup**: Markdown, HTML, CSS
- **Version Control**: Git

## Local Development

To run this site locally:

### Prerequisites
- Ruby (version 2.5 or higher)
- Bundler
- Git

### Setup

1. Clone the repository:
```bash
git clone https://github.com/hahahaki/hahahaki.github.io.git
cd hahahaki.github.io
```

2. Install dependencies:
```bash
bundle install
```

3. Run the local server:
```bash
bundle exec jekyll serve
```

4. Open your browser and visit:
```
http://localhost:4000
```

The site will automatically rebuild when you make changes to files.

## Adding Content

### Creating a New Blog Post

1. Create a new file in `_posts/` following the naming convention:
```
YYYY-MM-DD-title-of-post.md
```

2. Add front matter at the top:
```yaml
---
layout: default
title: "Your Post Title"
date: YYYY-MM-DD
categories: [Category1, Category2]
author: Hahahaki
---
```

3. Write your content in Markdown below the front matter

### Adding a New Project

Edit `projects.md` and add your project details following the existing format.

## Customization

### Site Configuration

Edit `_config.yml` to update:
- Site title and description
- Author information
- Social media links
- Navigation menu
- SEO settings

### Styling

Custom styles can be added in:
- `assets/css/style.scss` - Custom CSS/SCSS
- `_sass/` - Sass partials

### Layouts

Modify page layouts in `_layouts/` to change the structure of pages.

## Deployment

This site is automatically deployed to GitHub Pages when changes are pushed to the main branch.

### Manual Deployment

1. Make your changes
2. Commit and push:
```bash
git add .
git commit -m "Description of changes"
git push origin main
```

3. GitHub Pages will automatically build and deploy the site

## Configuration

Key configuration settings in `_config.yml`:

```yaml
title: Hahahaki's Portfolio
description: Personal website and portfolio
url: https://hahahaki.github.io
remote_theme: pages-themes/tactile@v0.2.0
```

## Pages

- **Home** (`index.md`) - Landing page with overview
- **About** (`about.md`) - Professional background and skills
- **Projects** (`projects.md`) - Portfolio of work
- **Blog** (`blog.md`) - Technical articles and posts
- **Contact** (`contact.md`) - Contact information and social links

## License

This project uses the Tactile theme, which is licensed under [Creative Commons Zero v1.0 Universal](LICENSE).

Content on this site is © 2024 Hahahaki. Feel free to reference or learn from the code structure, but please don't copy content verbatim.

## Contact

- **GitHub**: [@hahahaki](https://github.com/hahahaki)
- **Email**: contact@example.com
- **Website**: [hahahaki.github.io](https://hahahaki.github.io)

## Acknowledgments

- [Jekyll](https://jekyllrb.com/) - Static site generator
- [GitHub Pages](https://pages.github.com/) - Free hosting
- [Tactile Theme](https://github.com/pages-themes/tactile) - Base theme
- The open-source community for inspiration and tools

## Contributing

This is a personal website, but if you notice any issues or have suggestions:

1. Open an issue describing the problem or suggestion
2. For typos or small fixes, feel free to submit a pull request

---

Built with ❤️ using Jekyll and GitHub Pages
