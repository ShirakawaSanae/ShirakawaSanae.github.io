# Content guide

## Your profile

1. Replace `assets/images/profile-placeholder.svg` with a photo of yourself, keeping the same filename, or update `author.avatar` in `_config.yml`.
2. Replace every `Your Name`, placeholder email, institution, username, and biography in `_config.yml` and `index.md`.
3. Add a CV link to `author.links` in `_config.yml` when your PDF or CV page is ready.
4. Edit `_data/publications.yml`, `_data/projects.yml`, and `_data/friends.yml`. These files power the homepage and friend-links page.

## Blog structure

The Blog page at `/blog/` is both an index of latest notes and a directory of sub-blogs. A sub-blog is a Jekyll category with its own address:

```text
_posts/                           # Every public blog post lives here
_pages/blog.md                    # Main /blog/ landing page
_pages/blog/research.md           # /blog/research/ category archive
_pages/blog/reading.md            # /blog/reading/ category archive
_pages/blog/building.md           # /blog/building/ category archive
_data/blog_topics.yml             # Topic cards on the Blog landing page
_includes/blog-category.html      # Shared template for each category archive
```

The provided `Research`, `Reading`, and `Building` sub-blogs are ready to use. Their pages always list the matching public posts newest first.

## Publish a blog post

Save each public post in `_posts/` with this filename pattern:

```text
YYYY-MM-DD-short-title.md
```

For example, `_posts/2026-08-12-reproducible-experiments.md` becomes `/blog/reproducible-experiments/`.

Start the file with YAML front matter, then write normal Markdown below it:

```yaml
---
title: "Reproducible Experiments"
date: 2026-08-12 09:00:00 +0800
excerpt: "A concise one-sentence summary used on the Blog landing page."
categories:
  - research
tags:
  - methods
  - reproducibility
published: true
---
```

`title`, `date`, and `excerpt` control the visible blog card. `categories` determines the sub-blog. `tags` are optional labels for your own organization. `published` is optional and defaults to `true`.

To make a post appear in more than one sub-blog, list multiple categories:

```yaml
categories: [research, building]
```

After the YAML block, write the article in Markdown. An optional `<!--more-->` marker can control where Jekyll generates excerpts if you prefer that over setting `excerpt` yourself.

## Create another sub-blog

For a new sub-blog called `Teaching`, use the lowercase, URL-safe category name `teaching` consistently.

1. Add this item to `_data/blog_topics.yml`:

   ```yaml
   - title: "Teaching"
     slug: "teaching"
     description: "Courses, materials, and reflections on teaching."
   ```

2. Create `_pages/blog/teaching.md` with this YAML and Liquid template:

   ```markdown
   ---
   layout: single
   title: "Teaching"
   permalink: /blog/teaching/
   author_profile: true
   classes: wide
   ---

   Courses, materials, and reflections on teaching.

   {% include blog-category.html category="teaching" %}
   ```

3. Put `teaching` in the `categories` YAML field of any related post.

The new topic card will appear automatically on `/blog/`, and matching posts will appear on `/blog/teaching/` after GitHub Pages builds the site.

## Visibility and private drafts

Set `published: false` in a post's YAML to keep Jekyll from rendering it. This is useful for local previews, but it is **not private** when the source is committed to a public GitHub repository: visitors can still read the source file on GitHub.

For genuinely private writing, keep it in `private-drafts/`, which is excluded by `.gitignore`, or use a private repository. GitHub Pages itself does not provide post-level access control on a public site.

Use `_drafts/` only for posts you are comfortable committing. Jekyll does not publish that folder normally, but its files are still visible in a public repository.

## Local preview

This site keeps Minimal Mistakes as a local Bundler theme gem. It does not use Jekyll's `remote_theme` download mechanism, so `bundle exec jekyll serve` does not fetch the theme from GitHub. The first `bundle install` downloads the theme from RubyGems and stores it in `vendor/bundle` on this computer.

Install Ruby 3.3 with the MSYS2 DevKit from a VS Code PowerShell terminal:

```powershell
winget install --exact --id RubyInstallerTeam.RubyWithDevKit.3.3 --source winget --accept-package-agreements --accept-source-agreements
```

Restart the VS Code terminal after installation so Windows can refresh `PATH`. RubyInstaller includes RubyGems; install or update Bundler, then install this site's dependencies:

```powershell
ruby --version
gem install bundler
bundle --version
bundle install
bundle exec jekyll serve
```

Open `http://localhost:4000`. Use `bundle exec jekyll serve --drafts` to preview files stored in `_drafts`, or `bundle exec jekyll serve --unpublished` to preview a post with `published: false`.

If the theme needs to be refreshed later, run `bundle update minimal-mistakes-jekyll`. Do not add `jekyll-remote-theme` or `remote_theme` back to the configuration unless you deliberately want every fresh build to download the theme from GitHub.

## Upload to GitHub Pages

When a post or sub-blog is ready, commit and push its Markdown, YAML, and any images to your GitHub repository. For a typical `main` branch workflow:

```powershell
git add _posts _pages _data assets
git commit -m "Add research note"
git push origin main
```

GitHub Pages will rebuild the site after the push. Do not add `private-drafts/`; `.gitignore` keeps it out of `git add` by default.

After creating your GitHub repository, update `_config.yml`:

- For `username.github.io`, leave `baseurl: ""` and set `url: "https://username.github.io"`.
- For a project repository named `homepage`, set `baseurl: "/homepage"` and `url: "https://username.github.io"`.
- Set `repository` to `username/repository-name`.

Then enable **GitHub Pages** in the repository settings with the deployment source set to GitHub Actions or a branch. The `github-pages` Gemfile dependency keeps local builds close to GitHub Pages.
