# Transfer This Site to macOS

## What transfers unchanged

The website itself is portable. It uses Jekyll, Markdown, YAML, SCSS, SVG, and a GitHub-hosted theme; none of these depend on Windows. GitHub Pages builds the deployed site on its own servers, so visitors will see the same homepage regardless of whether you edit it from Windows or macOS.

The only machine-specific part is the local preview environment: Ruby, Bundler, Git, and a terminal need to be set up again on the Mac.

## Before leaving Windows

1. Make the repository complete: add your GitHub remote, commit the website files, and push them. Check the result first:

   ```powershell
   git status
   git remote -v
   git add .
   git commit -m "Initial academic homepage"
   git push -u origin main
   ```

2. Confirm the repository on GitHub contains the expected public files: `_config.yml`, `index.md`, `_posts/`, `_pages/`, `_data/`, `assets/`, `Gemfile`, and `Gemfile.lock` once Bundler has created it.

3. Do not add `private-drafts/` to Git. That folder is deliberately ignored. Move genuinely private drafts separately using an encrypted personal storage method, not a public GitHub repository.

4. Do not copy `_site/`, `vendor/`, `.bundle/`, or `.jekyll-cache/`. They are generated locally and will be recreated on the Mac.

## Set up the Mac

Open the macOS Terminal application. The commands use the default `zsh` shell.

1. Install Apple's Command Line Tools:

   ```zsh
   xcode-select --install
   ```

   macOS may say they are already installed. That is fine.

2. Install Homebrew using the command shown on [brew.sh](https://brew.sh/). Homebrew installs packages in an architecture-specific prefix, so the commands below work on both Apple Silicon and Intel Macs.

3. Install Git and a current Homebrew Ruby:

   ```zsh
   brew install git ruby
   ```

4. Put Homebrew Ruby before the Apple-supplied system Ruby, then reload your shell:

   ```zsh
   echo 'export PATH="$(brew --prefix ruby)/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```

5. Check the tools. `which ruby` should point somewhere inside Homebrew, not `/usr/bin/ruby`.

   ```zsh
   git --version
   ruby --version
   which ruby
   ```

## Clone and run the site

1. Configure Git identity on the Mac if this is a new setup. Use the same email address associated with your GitHub account:

   ```zsh
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   git config --global core.autocrlf input
   ```

2. Clone your public repository. Replace the example URL with your own repository:

   ```zsh
   git clone https://github.com/your-github-username/your-repository.git
   cd your-repository
   ```

   If you use SSH instead, add an SSH key for the Mac to GitHub before cloning or pushing. GitHub documents HTTPS and SSH authentication options in its [Git setup guide](https://docs.github.com/en/get-started/git-basics/set-up-git).

3. Install Bundler and the exact project dependencies:

   ```zsh
   gem install bundler
   bundle install
   ```

   The first successful `bundle install` creates `Gemfile.lock`. Commit that file. It keeps the Ruby gem versions consistent between your machines.

4. Verify and preview locally:

   ```zsh
   bundle exec jekyll doctor
   bundle exec jekyll serve
   ```

   Open `http://localhost:4000`. Stop the preview with `Ctrl+C`.

## Everyday workflow on either computer

Use GitHub as the source of truth. Before editing on a device, pull the current work. After editing and checking the preview, commit and push.

```zsh
git pull --ff-only
bundle exec jekyll serve
git status
git add _posts _pages _data assets index.md _config.yml
git commit -m "Add reading note"
git push
```

Use the same commands from Windows PowerShell except that the shell prompt and installation commands differ. The repository's `.gitattributes` file normalizes text files to LF line endings, so switching systems will not create a mass line-ending change.

## macOS-specific checks

### Photo and asset names

Some Mac volumes can be case-sensitive, and GitHub Pages runs on Linux, which is case-sensitive. Make every referenced path match its filename exactly. For example, `_config.yml` currently references:

```yaml
avatar: "/assets/images/profile-placeholder.svg"
```

If you replace the image with `my-photo.jpg`, update the configuration to the exact lowercase/uppercase filename. Do the same for images linked from blog posts.

### A local build fails after Ruby changes

After upgrading macOS, Homebrew, or Ruby, rebuild installed gems:

```zsh
bundle pristine
bundle install
```

If a gem reports compiler errors, rerun `xcode-select --install`, accept any macOS prompt, open a new Terminal window, and run `bundle install` again.

### The site works locally but not on GitHub Pages

Check these first:

1. The repository's `_config.yml` has the correct `url`, `baseurl`, and `repository` values.
2. Every image and Markdown link uses the exact filename case.
3. `Gemfile` and `Gemfile.lock` are committed.
4. The post filename follows `YYYY-MM-DD-title.md`, and its YAML front matter is valid.
5. GitHub Pages is enabled for the repository and its deployment log has no error.

## No manual site conversion is needed

Do not rewrite the Jekyll configuration, SCSS, or Markdown when changing computers. Clone the same repository, install the Mac-local Ruby environment, and continue editing. GitHub Pages is the common deployment environment that keeps the published result consistent.
