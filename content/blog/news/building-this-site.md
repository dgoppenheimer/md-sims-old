---
# categories: ["Examples"]
tags: ["Hugo", "Docsy"]
title: "Building This Site"
linkTitle: "Building This Site"
date: 2022-01-26
description: >
  This blog post provides a description of how I built this site using Hugo and the Docsy theme.
---

### Introduction

I wanted to build a site to store all the information I'm gathering for my course, *Practical Molecular Dynamics*. I've been happy using Hugo to build my *PyMOL Notes* website, and I like the automatic deploy to [GitHub Pages](https://pages.github.com/). This time I'll use the [Docsy theme](https://themes.gohugo.io/themes/docsy/) for Hugo. It is a theme designed for documentation. It looks great and has all the features I want. For a list of features and helpful tips, check out the [Docsy documentation](https://www.docsy.dev/docs/).

See the [Getting Started](https://www.docsy.dev/docs/getting-started/) page of the Docsy documentation to get started on installing this theme.

I already have Hugo installed.

```zsh
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

```zsh
hugo version
    hugo v0.92.0 darwin/amd64 BuildDate=unknown
```

Success!

### Create a project

```zsh
cd ~/Sites
hugo new site md-sims
```

#### Install PostCSS

This is from the [Docsy documentation](https://www.docsy.dev/docs/getting-started/).

```zsh
# in the ~/Sites/md-sims directory
npm --version
    8.1.0
# npm is installed
npm install -D autoprefixer
npm install -D postcss-cli
npm install -D postcss
```

Success!

### Install the theme

Follow the *Option 2: Use the Docsy theme in your own site* section from the [Docsy documentation](https://www.docsy.dev/docs/getting-started/).

```zsh
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

### Preview the Site

```zsh
hugo server
# preview site at http://localhost:1313/
```

Site would not build. I needed the `extended` variant of Hugo.

```zsh
sudo port install hugo +extended
```

Spin up the site.

```zsh
hugo server
# preview site at http://localhost:1313/
```

Success!

### Configure the Site

Start by copying the `config.toml` file from the [example project](https://github.com/google/docsy-example/blob/master/config.toml) into the `md-sims` directory.

To get a better idea of how the theme structures content, I'll clone the [Docsy example site](https://github.com/google/docsy-example). The steps, below are from the [Docsy documentation](https://www.docsy.dev/docs/getting-started/#using-the-command-line).

```zsh
cd ~/Sites
git clone https://github.com/google/docsy-example.git
cd docsy-example
git submodule update --init --recursive
hugo server
```

Success! Now I have a site that I can use as a template.

### Modify `config.toml`

- Add the site name to `title = " "`. There are two places, one at the top, and one under `[languages]`. For now I'll name the site *Molecular Dynamics*.
- I'll comment out the sections I don't need like the other languages and some of the social media sections.
- Comment out the *Privacy Policy* link.
- Change links to GitHub and StackOverflow. I'll modify the link to point to the repository of this site after I get things up and running.
- In `[markup]`, change `[markup.highlight]` to `style = "emacs"`.

### Modify Home Page

- Copy the content from the example site into `~/Sites/md-sims/content/en`.
- Move a background image into the `~/Sites/md-sims/content/en` directory. I used a `.jpg` image the same size (1290 x 1280 pixels) as the example image.
- Rename the image to `actin-spheres-background.jpg`. The `cover.html` file in `themes/docsy/layouts/shortcodes/blocks` specifies the background image as one with `background` in the image name. See the [Docsy documentation](https://www.docsy.dev/docs/adding-content/iconsimages/) for details.
- Change the text in `_index.html` in `~/Sites/md-sims/content/en`. Add the site title and the subtitle,*A place to collect and organize my notes on MD simulations*. Eh, not particularly engaging, but good enough for now.
- There are spaces for additional text on the landing page under the main image and additional sections, but I don't need them right now. I'll comment out the things I don't need.

### Modify Footer

Copy the `md-sims/themes/docsy/layouts/partials/footer.html` to `md-sims/layouts/partials`, then modify the copy, This will override the theme file. Using overrides allows you to update the theme files without destroying your modifications.

{{% alert title="Note" color="primary" %}}
The copyright notice, Privacy Policy, and links to StackOverflow and GitHub are found in `config.toml`, not in `footer.html`. To change the text of the "About" link in the footer, change the `title` text in `content/en/about/_index.html`.
{{% /alert %}}

#### Alert Shortcodes

Here are the different colors assigned to the alert shortcodes, and how each will render. You can change the title to whatever you like.

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

### Modify the "About" Page

Make appropriate changes to `content/en/about/_index.html`.

### Modify the `content` Directory

Because this is not a "true" documentation website, I will rename the *Documentation* directory. I'll also delete the unneeded subdirectories, and rename others.

Rename the `md-sims/content/en/blog/releases` directory to `reference` and change the `_index.md` contents to:

```toml
---
title: "Reference"
linkTitle: "Reference"
weight: 20
---
```

Because I will not be using multiple languages, I moved the content out of the `en/` directory and into the root of the `content/` directory.

### Git Stuff

I need to do the following to clean up the version control of the site.

- Add a `.gitignore` file
- Remove `.DS_store` and similar files from tracking by git
- Push to `GitHub`
- Set up automatic deploy