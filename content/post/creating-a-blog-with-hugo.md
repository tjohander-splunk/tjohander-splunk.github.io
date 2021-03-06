---
title: "Creating & Publishing Github-hosted Blog With Hugo"
date: 2022-04-27T10:50:54-05:00
draft: false
description: "For my first post, I'd like to teach anyone how to create an attractive, professional Blog with minimal technical skills.  Once things are set up, adding content is as simple as creating a Markdown files and pushing to Github."
---

How to make a simple, attractive and expressive blog to convey your ideas with a Github Pages-hosted blog.

## Initial Setup of Github Pages Hosting Feature

These steps are covered in more detail [here](https://pages.github.com/), but to summarize:
1. Create a new Github repo with the name `<your-github-user-name>.github.io`.
2. Clone the repo locally with the `git clone` command
3. Add some simple content into an `index.html` file.
4. Push the "homepage" to your repo with `git push`
5. Hit your new page by navigating to `https://<your-github-user-name>.github.io`.  This is a great start, but your blog is not very interesting yet.

## Let's Bring in Hugo to Jazz Things Up
7. Remove that boring `index.html`: 
    - `rm index.html`
8. Create a new Hugo Site
    `hugo new site . --force`
9. Add a Theme.  Here we'll pick the "Anake" theme, but there are hundreds from which to choose.
    - Head to themes.hugo.io
    - Pick Blogs -> Anake
    - `git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke`
    - `echo theme = \"ananke\" >> config.toml`
10. Add some content:
     - `hugo new post/my-first-post.md` (what does this do)
     - Edit the content: `vim /Users/tjohander/tjohander-splunk.github.io/content/post/my-first-post.md`
11. Serve it locally
     - hugo server -D

12. Edit the config
    - Change the `baseUrl`: `https://<your-github-username>.github.io`
    - Change the `title`: Anything is fine here
    - change the `publishDir` to `./`
13. Add a Github Action:
    - `mkdir -p .github/workflows`
    - `vim .github/workflows/gh-pages.yaml`
    - Add in the workflow config
    - `git push`
14. Change Actions Permissions & Pages configuration in the Repo "Settings" UI
    - `Settings` -> `Actions` -> `General` -> `Workflow Permissions` -> `Read and Write Permissions`
    - Switch the Pages -> Branch to `gh-pages` and Save

15. The Pages build process will execute
16. Load your new blog
17. Add some new content
    - In your terminal enter `hugo new post/my-second-post.md`
    - This creates an empty `Post` template file with the title "My Second Post"
    - Edit this file just like any other Markdown file.
18. Commit your changes:
    - `git add --all`
    - `git commit -m <some helpful message about this commit>`
    - `git push`
19. Your site will be rebuilt and your new content should show up as soon as the Github action completes.


## Github Actions Workflow Config

```yaml
name: Github Pages Build and Deploy  

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  Publish:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: recursive  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
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
          publish_dir: ./
          publish_branch: gh-pages
```

## Resources For Further Reading

### Hugo Documentation
* https://gohugo.io
* https://gohugo.io/getting-started/quick-start/
* https://themes.gohugo.io/
* [An Excellent tutorial series on Youtube](https://www.youtube.com/playlist?list=PLLAZ4kZ9dFpOnyRlyS-liKL5ReHDcj4G3)

### Github Pages, Github Actions and Markdown
* https://pages.github.com/
* https://docs.github.com/en/actions/using-workflows/about-workflows
* https://docs.github.com/en/get-started/writing-on-github/
* https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

### Internal Splunk Examples of Hugo in Action
* https://leungsteve.github.io/realtime_enrichment/
* https://signalfx.github.io/observability-workshop/latest/

### A Famous Example of the "Docsy" Theme
* https://kubernetes.io/docs/home/
* https://themes.gohugo.io/themes/docsy/






