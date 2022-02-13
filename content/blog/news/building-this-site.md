---
# categories: ["Examples"]
tags: ["Hugo", "Docsy"]
title: "Building This Site"
linkTitle: "Building This Site"
date: 2022-01-26
description: >
  This blog post provides a description of how I built this site using Hugo and the Docsy theme.
---

## Introduction

I wanted to build a site to store all the information I'm gathering for my course, *Practical Molecular Dynamics*. I've been happy using Hugo to build my *PyMOL Notes* website, and I like the automatic deploy to [GitHub Pages](https://pages.github.com/). This time I'll use the [Docsy theme](https://themes.gohugo.io/themes/docsy/) for Hugo. It is a theme designed for documentation. It looks great and has all the features I want. For a list of features and helpful tips, check out the [Docsy documentation](https://www.docsy.dev/docs/).

See the [Getting Started](https://www.docsy.dev/docs/getting-started/) page of the Docsy documentation to get started on installing this theme.

I already have Hugo installed.

```bash
hugo version
    hugo v0.89.3 darwin/amd64 BuildDate=unknown
# The latest version is v0.92.0, I'll update.
port installed
# hugo was listed, which means I installed hugo using MacPorts
# Update hugo
 sudo port selfupdate
 sudo port upgrade outdated
 # this will take a while
```

```bash
hugo version
    hugo v0.92.0 darwin/amd64 BuildDate=unknown
```

Success!

## Create a project

```bash
cd ~/Sites
hugo new site md-sims
```

### Install PostCSS

This is from the [Docsy documentation](https://www.docsy.dev/docs/getting-started/).

```bash
# in the ~/Sites/md-sims directory
npm --version
    8.1.0
# npm is installed
npm install -D autoprefixer
npm install -D postcss-cli
npm install -D postcss
```

Success!

## Install the theme

Follow the *Option 2: Use the Docsy theme in your own site* section from the [Docsy documentation](https://www.docsy.dev/docs/getting-started/).

```bash
# in the ~/Sites directory
hugo new site md-sims
cd md-sims
git init
# rename master branch
git branch -m main
git submodule add https://github.com/google/docsy.git themes/docsy
echo 'theme = "docsy"' >> config.toml
git submodule update --init --recursive
```

## Preview the Site

```bash
hugo server
# preview site at http://localhost:1313/
```

Site would not build. I needed the `extended` variant of Hugo.

```bash
sudo port install hugo +extended
```

Spin up the site.

```bash
hugo server
# preview site at http://localhost:1313/
```

Success!

## Configure the Site

Start by copying the `config.toml` file from the [example project](https://github.com/google/docsy-example/blob/master/config.toml) into the `md-sims` directory.

To get a better idea of how the theme structures content, I'll clone the [Docsy example site](https://github.com/google/docsy-example). The steps, below are from the [Docsy documentation](https://www.docsy.dev/docs/getting-started/#using-the-command-line).

```bash
cd ~/Sites
git clone https://github.com/google/docsy-example.git
cd docsy-example
git submodule update --init --recursive
hugo server
```

Success! Now I have a site that I can use as a template.

## Modify `config.toml`

- Add the site name to `title = " "`. There are two places, one at the top, and one under `[languages]`. For now I'll name the site *Molecular Dynamics*.
- I'll comment out the sections I don't need like the other languages and some of the social media sections.
- Comment out the *Privacy Policy* link.
- Change links to GitHub and StackOverflow. I'll modify the link to point to the repository of this site after I get things up and running.
- In `[markup]`, change `[markup.highlight]` to `style = "emacs"`.

## Modify Home Page

- Copy the content from the example site into `~/Sites/md-sims/content/`.
- Move a background image into the `~/Sites/md-sims/content/` directory. I used a `.jpg` image the same size (1290 x 1280 pixels) as the example image.
- Rename the image to `actin-spheres-background.jpg`. The `cover.html` file in `themes/docsy/layouts/shortcodes/blocks` specifies the background image as one with `background` in the image name. See the [Docsy documentation](https://www.docsy.dev/docs/adding-content/iconsimages/) for details.
- Change the text in `_index.html` in `~/Sites/md-sims/content/`. Add the site title and the subtitle,*A place to collect and organize my notes on MD simulations*. Eh, not particularly engaging, but good enough for now.
- There are spaces for additional text on the landing page under the main image and additional sections, but I don't need them right now. I'll comment out the things I don't need.

## Modify Footer

Copy the `md-sims/themes/docsy/layouts/partials/footer.html` to `md-sims/layouts/partials`, then modify the copy, This will override the theme file. Using overrides allows you to update the theme files without destroying your modifications.

{{% alert title="Note" color="primary" %}}
The copyright notice, Privacy Policy, and links to StackOverflow and GitHub are found in `config.toml`, not in `footer.html`. To change the text of the "About" link in the footer, change the `title` text in `content/about/_index.html`.
{{% /alert %}}

## Alert Shortcodes

Here are the different colors assigned to the alert shortcodes. You can change the title to whatever you like.

Shortcode format:

```go
{{%/* alert title="Warning" color="warning" */%}}
This is a warning. Use `title="Warning" color="warning"`
{{%/* /alert */%}}
```

{{% alert title="Warning" color="warning" %}}
This is a warning. Use `title="Warning" color="warning"`
{{% /alert %}}

{{% alert title="Success" color="success" %}}
This is a success alert. Use `title="Success" color="success"`
{{% /alert %}}

{{% alert title="Information" color="info" %}}
This is an information alert. Use `title="Information" color="info"`
{{% /alert %}}

{{% alert title="Tip" color="primary" %}}
This is a tip alert. Use `title="Tip" color="primary"`
{{% /alert %}}

{{% alert title="Dark" color="dark" %}}
This is a dark alert. Use `title="some title" color="dark"`
{{% /alert %}}

{{% alert title="Orange" color="secondary" %}}
This is an orange alert. Use `title="some title" color="secondary"`
{{% /alert %}}

## Modify the "About" Page

Make appropriate changes to `content/about/_index.html`.

## Add a Logo

I made an image in the graphics design program, Affinity Designer, exported it as `logo.svg`, and put it in the `assets/icons/` directory. Once I created the `logo.svg` file, I removed the `width="100%" height="100%"` from the `<svg  >` tag. Otherwise, the logo was moved away from the left margin.

Also, to get the new `favicon` to show up in the browser, refresh the page.

## Modify the `content` Directory

Because this is not a "true" documentation website, I will eventually rename the *Documentation* directory. I'll also delete the unneeded subdirectories, and rename others.

Rename the `md-sims/content/blog/releases` directory to `reference` and change the `_index.md` contents to:

```toml
---
title: "Reference"
linkTitle: "Reference"
weight: 20
---
```

Because I will not be using multiple languages, I moved the content out of the `en/` directory and into the root of the `content/` directory.

## Get Links to Open in a New Tab

To get external links to open in a new browser tab, you need use a [render hook template](https://gohugo.io/getting-started/configuration-markup/#render-hook-templates). Just add the following code to `layouts/_default/_markup/render-link.html` and you are good to go!

```html
<a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if strings.HasPrefix .Destination "http" }} target="_blank" rel="noopener"{{ end }}>{{ .Text | safeHTML }}</a>
```

## Git Stuff

I need to do the following to clean up the version control of the site.

- Add a `.gitignore` file
- Remove `.DS_store` and similar files from tracking by git
- Push to `GitHub`
- Set up automatic deploy

### Add a `.gitignore` File

- Untrack the `.DS_Store` file.

```bash
git rm --cached .DS_Store
```

### Badges

[Binder](https://mybinder.org/)
I will include `open in colab`, `launch binder`, and `render in nbviewer` badges to each notebook in my GitHub repository. Binder

## Images

I use [Hugo shortcodes](https://gohugo.io/content-management/shortcodes/) for displaying images on this site, but on Colab I have to use Markdown:

```markdown
![image description](https://github.com/path/to/image/image.png)
```

Unfortunately, the limited markdown renderer used by Colab does not allow the image size to be adjusted--it takes up the container width. This does not look good for many images. I found [this workaround on Stackoverflow](https://stackoverflow.com/questions/14675913/changing-image-size-in-markdown):

```html
[<img src="/logo-stackoverflow.png" width="250"/>](/logo-stackoverflow.png)
```

Which renders as:

[<img src="/logo-stackoverflow.png" width="250" />](/logo-stackoverflow.png)

For Colab, I store the images in a repository on GitHub. To properly display the images, you need to append `?raw=true` to the end of the address for the image. For example:

```html
[<img src="https://github.com/dgoppenheimer/notebook-images/blob/main/logo-stackoverflow.png?raw=true" alt="Stackoverflow logo" width="250" />](https://github.com/dgoppenheimer/notebook-images/blob/main/logo-stackoverflow.png?raw=true)
```

renders as:

[<img src="https://github.com/dgoppenheimer/notebook-images/blob/main/logo-stackoverflow.png?raw=true" alt="Stackoverflow logo" width="250" />](https://github.com/dgoppenheimer/notebook-images/blob/main/logo-stackoverflow.png?raw=true)

## Automatic Build and Deploy Using GitHub Actions

See the following tutorial from `peaceiris`: [GitHub Actions for Hugo](https://github.com/peaceiris/actions-hugo)

Additional information is found in the `README.md` file on the GitHub repo for [actions-gh-pages](https://github.com/peaceiris/actions-gh-pages).

First, create a `.github/workflows/` directory in your project root.

Next, create a `gh-pages.yml` file in the `.github/workflows/` directory that has the following contents:

```yaml
name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.91.2'
          extended: true

      - name: Build
        run: hugo # --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

Note: I removed `–minify` from `run: hugo`, and changed `extended:` to `extended: true`.

### Set Up the Remote Repository

- Set up the new `md-sims` repository in GitHub.
- Go to GitHub and change the settings for the `md-sims` repository so that it uses the `main` branch for GitHub Pages.
- Upload content:

```bash
git remote add origin git@github.com:dgoppenheimer/md-sims.git
git branch -M main
git push -u origin main
```

{{% alert title="Tip" color="primary" %}}
Due to the limitations of the `GITHUB_TOKEN` the first deployment will fail. Go back to GitHub and change the GitHub Pages Source to the `gh-pages` branch. The build will succeed. Then change back to the `main` branch.
{{% /alert %}}

- Create a new `gh-pages` branch.

```bash
# in the root of my project
git switch -c gh-pages
git status
# On branch gh-pages
# nothing to commit, working tree clean
git push origin gh-pages
```

### Create a Deploy Key

See [Create SSH Deploy Key](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-create-ssh-deploy-key) for instructions.

```bash
# in the project directory
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
```

Two files are generated by `ssh-keygen`:

`gh-pages.pub` is the public key  
`gh-pages` is the private key

- Go to the repository **Settings** &#8594; **Deploy Keys**.
- Click `Add deploy key`.
- Add a `Title`: "Public key of ACTIONS_DEPLOY_KEY"
- Add the contents of the `gh-pages.pub` public key file to the `Key` box and click `Allow write access`, and `Add key`.
- Go to **Secrets** and add the contents of the `gh-pages` private key file with the title, `ACTIONS_DEPLOY_KEY`.

### Set the `deploy_key` Option

See [Set SSH Private Key deploy_key](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-set-ssh-private-key-deploy_key)

- Add the following to the `gh-pages.yml` file.

```yaml
- name: Deploy
  uses: peaceiris/actions-gh-pages@v3
  with:
    deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
    publish_dir: ./public
```

Change `config.toml` to `baseURL = "https://dgoppenheimer.github.io/md-sims/"`

Check out the site at `https://dgoppenheimer.github.io/md-sims/`!

change `submodules: true` to `submodules: recursive` in `gh-pages.yml`

