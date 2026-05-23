## Copilot Instructions for dhouse109.github.io

### Project Overview
Jekyll-based static site using [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) remote theme, deployed via GitHub Pages. Features a custom category system with data-driven display names and a home page that showcases featured posts by category.

### Architecture & Custom Features

**Custom Category System** (Core differentiator from stock Minimal Mistakes):
- Category slugs (e.g., `product-and-systems-architecture`) map to human-readable names via `_data/categories.yml`
- Each category entry has `display_name` and `description` fields
- Three custom includes handle the mapping: `category-list.html`, `posts-taxonomy.html`, `_includes/posts-taxonomy.html`
- Liquid logic iterates through `site.data.categories` to match slugs to display names, with fallback to formatted slug
- **Important**: When adding new categories, update BOTH `_data/categories.yml` AND post front matter

**Home Page Architecture**:
- Uses custom `home-custom` layout (not Minimal Mistakes default)
- `index.html` defines `featured_categories` array in front matter with name, display_name, description
- Layout renders 3 most recent posts per category with custom excerpt truncation (160 chars)
- Categories act as TOC anchors (`id="{{ category_item.name | slugify }}"`)

**External Links & Security**:
- Custom JS handler (`assets/js/externalLinksHandler.js`) auto-adds `target="_blank"` and `rel="noopener"` to external links and PDFs
- Loaded via `_includes/head/custom.html` alongside favicon and Bootstrap Icons CDN
- Overrides `jekyll-target-blank` plugin for finer control (plugin config: `noopener: false, noreferrer: false`)

### Developer Workflows

**Local Development**:
```bash
bundle exec jekyll serve  # Builds and serves at localhost:4000
```
- **CRITICAL**: Changes to `_config.yml` require server restart (not auto-reloaded)
- `_site/` is build output (excluded from git via default Jekyll settings)
- Uses Windows-compatible gems: `tzinfo-data` and `wdm` for file watching

**Adding Content**:
1. **New Post**: Create `_posts/YYYY-MM-DD-title.md` with front matter:
   ```yaml
   ---
   title: "Post Title"
   last_modified_at: YYYY-MM-DDTHH:MM:SS-05:00  # Optional, shows modified date
   categories:
     - product-and-systems-architecture  # Must match _data/categories.yml key
   tags:
     - tag1
   ---
   ```
2. **New Category**: Add to `_data/categories.yml` first, then use in posts

**Styling Customization**:
- Custom styles in `assets/css/main.scss` (imports Minimal Mistakes, then custom overrides)
- Example: `.categories-showcase`, `.intro-section` for homepage layout
- Site skin set via `minimal_mistakes_skin: default` in `_config.yml`

### Configuration Patterns

**Permalinks**: `/:categories/:title/` (SEO-friendly, category-based URLs)
**Pagination**: 5 posts per page, path `/page:num/`
**Site Configuration & Defaults**:
- Posts use `single` layout with `author_profile: true`, `read_time: true`, no comments/sharing
- Pages use `single` layout with `author_profile: true`
- Category/tag archives use liquid-based generation (`type: liquid`)
- Custom site title: "Software Studies" with tagline about software engineering
- Analytics: Google Analytics (GA4) via `analytics.provider: "google-gtag"`
**Plugins** (all in `Gemfile` and `_config.yml` plugins array):
- `jekyll-target-blank`: Set `noopener: false, noreferrer: false` (custom JS handles security)
- `jekyll-include-cache`: Performance optimization for cached includes
- `jekyll-paginate`, `jekyll-sitemap`, `jekyll-feed`: Standard SEO/navigation
- `jekyll-redirect-from`, `jemoji`, `jekyll-gist`, `jekyll-algolia`: Additional features
- Uses `github-pages` gem group for GitHub Pages compatibility

**Author & Footer Links**:
- Defined in `_config.yml` under `author.links` and `footer.links` (typically identical)
- Include resume PDF path: `/assets/portfolio/resume.dhouse.v14.01.2025.pdf`
- Custom Substack icon via Bootstrap Icons CDN + inline CSS in `_includes/head/custom.html`
- Uses mix of Font Awesome (`fab`, `fas`) and Bootstrap Icons (`bi bi-substack`) for social links

### Common Modifications

**Update Homepage Featured Categories**:
Edit `index.html` front matter `featured_categories` array (not `_data/categories.yml`)

**Add New Social Link**:
Update both `author.links` and `footer.links` in `_config.yml` with label, icon (Font Awesome class), and url

**Change Theme Skin**:
Modify `minimal_mistakes_skin` in `_config.yml` (options: default, air, aqua, contrast, dark, dirt, neon, mint, plum, sunrise)

**Custom Head Elements**:
Add to `_includes/head/custom.html` (favicons, CDN links, inline styles)

### Key Files Reference
- `_config.yml`: Site-wide settings, author/footer links, plugin config
- `_data/categories.yml`: Category display name mapping (required for custom system)
- `index.html`: Homepage front matter with featured categories
- `_layouts/home-custom.html`: Custom homepage rendering logic
- `_includes/category-list.html`: Renders category tags on post pages
- `_includes/posts-taxonomy.html`: Renders category archive page
- `assets/css/main.scss`: Custom styling overrides
- `assets/js/externalLinksHandler.js`: External link security handler

---
**Critical Gotcha**: The custom category system requires three-way consistency between `_data/categories.yml` keys, post front matter `categories` values, and `index.html` featured category `name` fields. Mismatches cause rendering failures.
