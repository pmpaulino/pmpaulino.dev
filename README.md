# pmpaulino.dev

This repository contains the source code for [pmpaulino.dev](https://pmpaulino.dev), a personal website built using Hugo and the PaperMod theme.

## Installation

To set up the project locally, follow these steps:

1. **Clone the repository:**

    ```sh
    git clone https://github.com/yourusername/pmpaulino.dev.git
    cd pmpaulino.dev
    ```

2. **Install Hugo:**

    Make sure you have Hugo installed. Follow the [Hugo Quick Start](https://gohugo.io/getting-started/quick-start/) guide for installation instructions.

3. **Setup new Hugo site with the PaperMod theme:**

    By following the [PaperMod theme installation instructions](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation), you can install the theme in your Hugo project.

## Usage

To start the Hugo server and view the site locally, run:

```sh
hugo server
```

The site will be available at `http://localhost:1313`.

## Configuration

The main configuration file for the site is `hugo.yaml`. You can customize various settings such as the site title, author, and theme parameters.

Example configuration:

```yaml
baseURL: "https://pmpaulino.dev/"
languageCode: "en-us"
title: "pmpaulino.dev"
theme: ["PaperMod"]

params:
  ShowToc: false
  TocOpen: false
  social:
    twitter: "your_twitter_handle"
    github: "your_github_handle"
```

## Content

Content for the site is stored in the `content` directory. You can add new posts and pages by creating Markdown files in the appropriate subdirectories. Currently the `hugo new` command is not supported for creating new posts.

Example content file (`content/posts/hello-world.md`):

```markdown
---
author: pmpaulino
title: Hello world
date: 2024-05-06
description: Mic check 1, 2
tags:
  - Hello
ShowToc: false
TocOpen: false
---

My first post, testing some things out!
```

## Deployment

The site is deployed using Cloudflare Pages. You can deploy your own site by following the [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/).

## Contributing

Contributions are welcome! If you have any suggestions or improvements, please open an issue or submit a pull request.

## To-Do

- [ ] Document Obsidian notes integration for writing posts
- [ ] Fix `hugo new` command for creating new posts to work alongside Obsidian setup
- [ ] Document Cloudflare Pages deployment process and configuration
