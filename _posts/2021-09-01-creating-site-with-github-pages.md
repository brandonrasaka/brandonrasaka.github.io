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

1. In `index.md`, notice the first few lines are surrounded by thre dashes `---`. This is the front matter for your page. Every page in your site will contain front matter. My site's home page (index.md) has the following front matter:

    ```yaml
    ---
    layout: default
    title: Home
    nav_order: 1
    ---
    ```
    
    Let me explain what each of these means.

    * Layout: There are different layout templates, usually multiple styles with each Jekyll theme. The Hacker theme has only two layouts that are nearly identical: default and post. The template for these is stored in the `_layouts` directory as HTML files, but there are several variables and scripts that use the _Liquid_ language to call pages and variable from elsewhere in the site. For example, the `default.html` layout has this line: 

        ```liquid
        {{ "{% include header.html" }}%}
        ```
    
        This tells it to insert the contents of `_includes/header.html` at that point, which itself is more HTML. So you can nest HTML within HTML using these variables.

    * Title: This is pretty self-explanatory. It's the title of the page. But more importantly, it's also a variable that can be called elsewhere. For example, my `_includes/header.html` file contains the following loop:

        ```liquid
        <nav class="main-nav">
        <ul>
                {{ "{% assign navigation_pages = site.html_pages | sort: 'nav_order'" }} %}
                {{ "{% for p in navigation_pages" }} %}
                    {{ "{% if p.nav_order" }} %}
                        <li><a href="{{ "{{ p.url " }}}}" {{ "{% if p.url == page.url" }} %} class="active"{{ "{% endif" }} %}>{{ "{{ p.title" }} }}</a></li>
                    {{ "{% endif" }} %}
                {{ "{% endfor" }} %}
            </ul>
        </nav>
        ```

        I'm not going to explain every element in this bit of code, but notice the `{{ "{{ p.title " }}}}` in the `<li>` line. That's part of an If-THEN statement in a FOR loop that is calling each Markdown (.md) page `p` in the root directory (known as navigation pages) and using their `title` variables as the name of each link in the navigation bar in the header of every page.

    * nav_order: I'll be honest, I don't remember if this was part of the default front matter when I downloaded the theme, or if it's something I added later. But this tells the above loop in which order to display the links for each navigation page. So my home page (index.md) has `nav_order: 1`, my _About Me_ page (about.md) has `nav_order: 2`, and so on.

1. Below the front matter you can use Markdown language to fill in your home page with the content you want, following the theme.md page as a guide. The beauty of using Jekyll is that you don't have to write HTML for every single page. The more HTML, CSS, Javascript, Liquid, Markdown, and YAML you know, the more creative you can get. But with a pre-made theme you can get to work with very little extra effort if your knowledge of these languages isn't up to snuff. I'm still pretty new to all of these languages, and yet I built this website!

    I _did_ tweak my header quite a bit, so I had to tinker with the HTML and CSS quite a bit to get it how I liked it, so my header won't be quite the same as yours even if you're using the Hacker theme.

1. At this point you can make as many navigation pages as you like. Just give them a `.md` extension and store them in the repo's root directory. Make sure to edit the front matter appropriately and you're good to go!

## Start Blogging

Okay, so now you've got navigation pages, it's time to start blogging. Now, I don't use my website as a blog, per se, but I use the blogging functionality. As you can see, I use it to post my projects and tutorials. But Jekyll has some great functionality for blogging.

1. To write your first blog post, make a new Markdown file in the `_posts` subdirectory, giving it a name with the date for a suffix in the format `yyyy-mm-dd`. For example, the filename for this page is `2021-09-01-creating-site-with-github-pages.md`.

1. Give the page some front matter. Again, using this page as an example, the front matter is

    ```yaml
    ---
    layout: post
    title: "Creating A Website with GitHub Pages and Jekyll"
    permalink: /:year/:month/:day/:title/
    ---
    ```

    The layout and title variables are the same as before, but now there's a new one: permalink. This tells the site what the URL for this page (after the domain name) should be, which you can verify by checking the URL in your browser right now.

1. Just like with the navigation pages, here's where you can enter your post's content.