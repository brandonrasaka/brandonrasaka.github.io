---
layout: post
title: "Creating A Website with GitHub Pages and Jekyll"
permalink: /:categories/:year/:month/:day/:title/
---

If you want to create a website similar to this one, here is a guide for how to do it. With GitHub, you can create and host a website for free! Though you don't have to use Jekyll, I have found it to be best option and doesn't require a ton of HTML and CSS. Instead, Jekyll uses YAML and Markdown. Don't worry if you don't know either one, I don't either, not really! I know just enough to get by in creating my website, then I look up anything I don't know. (Google-Fu is a necessary skill to have for any developer!)

## Set up your GitHub Repository

1. Create GitHub account. My username is just my first and last name: _brandonrasaka_

1. Create new repo and name it `[username].github.io`

    > For example, mine is `brandonrasaka.github.io`

1. Download and install [GitHub Desktop](https://desktop.github.com/)

1. When prompted, sign in to GitHub

1. When it finishes installing, you will see a list of Your repositories, including the one you just made

1. Select it, then click the Clone button. This creates a new folder for storing all the files in the repo. Any changes you make in any file in the folder you can then “push” to the repo on GitHub

![Select repo from GitHub Desktop start screen](/assets/images/github-pages/github-desktop-start.png)

## Install Jekyll

1. Follow Jekyll’s official guides [here](https://jekyllrb.com/docs/installation/) to install Jekyll and its dependencies

1. If on Windows, open PowerShell or Command Prompt and navigate to your GitHub repo directory

1. Bundle the site’s contents and make it available on a local server

    <pre><code><span style="color:rgba(255, 255, 255, 0.5)">></span> bundle exec jekyll serve</code></pre>

## Get the *Hacker* Theme and Start Customizing

1. Download *Hacker* theme from [https://github.com/pages-themes/hacker](https://github.com/pages-themes/hacker)

    > You can obviously pick your own theme and don't have to use this one. There are a number of sources for downloading Jekyll themes, which you can Google-Fu for. Also, the [Jekyll documentation site](https://jekyllrb.com/docs/themes/) has a list of theme sources as well.

1. Extract the contents of the download to your repo folder

1. Navigate your web browser to `http://localhost:4000` or `127.0.0.1:4000`. You should see the default home page of the hacker theme.

1. Now it’s time to make changes to suit your needs. For this tutorial, I’ll be giving you the steps that I took to get my site how I like it, so make sure to tailor yours as needed. We’re going to be writing and editing files, so you’ll want a good text editor. Personally, I prefer to use [VS Code](https://code.visualstudio.com/download), but [Atom](https://atom.io/) or [Notepad++](https://notepad-plus-plus.org/downloads/) work great too.

1. From the root directory, open the `_config.yml` file. Edit its contents like shown below, but obviously change the information to match your needs, then save the file.

    ```yaml
    title: Brandon_Rasaka
    description: Get to know me and check out some of the work I've done.
    show_downloads: true
    theme: jekyll-theme-hacker
    name: Brandon_Rasaka
    ```

    > There are loads of more options you can specify in this file, but this is all I have for my simple site, and it's the basics you'll need for yours.

1. If you refresh your browser page, you should already see some changes. For example, the top line on the page (as well as the name of the site in the browser tab) will match the `title` value, and the second line will match the `description` value.

1. Now let's customize your homepage. Find the `index.md` file and copy it, renaming it `template.md`.

    > You don't actually have to copy it, but I find it helpful to keep a local copy of the style guide.

1. In `index.md`, notice the first few lines are surrounded by thre dashes `---`. This is the front matter for your page. Every page in your site will contain front matter. My site's homepage (index.md) has the following front matter:

    ```yaml
    ---
    layout: default
    title: Home
    nav_order: 1
    ---
    ```
    
    Let me explain what each of these means.

    * Layout: 