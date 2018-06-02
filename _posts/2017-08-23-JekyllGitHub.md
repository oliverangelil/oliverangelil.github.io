---
layout: post
title:  "Setting up Jekyll on GitHub Pages"
date:   2017-08-23 06:25:17
categories: website blog jekyll github
permalink: /posts/jekyll-github
image: "https://github.com/nherger/nherger.github.io/blob/master/photos/JekyllGitHub_banner.png?raw=true"
---

This blog is hosted on [GitHub Pages](https://pages.github.com/) and built based on the static site generator [Jekyll](https://jekyllrb.com/). Here, I give a brief overview on how this can be set up.

<!--more-->

# Setting up GitHub Pages
First of all, you need to follow [these instruction](https://pages.github.com/) to create your own GitHub Page. A new repository can be created [here](https://github.com/new). Note, that your new repo should be named *username.github.io*, where *username* is your username on GitHub.

# Find a Jekyll template
If you don't want to build a Jekyll site from scratch, you might find [this website](http://jekyllthemes.org/) with a large range of templates helpful. Have a look at their demos and GitHub repos. My website is based on the Jekyll blog theme ["EasyBook"](https://github.com/laobubu/jekyll-theme-EasyBook).

# Clone GitHub Page
Once you find a template that you'd like to work with, you can clone it to a local directory of your choice. In my case, I executed:
`git clone https://github.com/laobubu/jekyll-theme-EasyBook.git`

# Modify the _config.yaml file
After you have a local copy of the repo, you need to modify the `_config.yaml` file using your favourite editor. You'll need to change the `url` and the `base_url`. In my case, I chose: `url: "http://nherger.github.io"` and `baseurl: ""`. I then use `git add *`, `git commit -m *` and `git push` to make the changes online (replace * with the relevant code).

# Preview Jekyll pages locally
I followed [these steps](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) to preview the changes I made locally before pushing the changes to GitHub Pages. I installed Ruby from source based on [these steps](https://www.ruby-lang.org/en/documentation/installation/#building-from-source).
To run the Jekyll site locally, you then have to execute `bundle exec jekyll serve` and preview your local site in your web browser at `http://localhost:4000`.
Alternatively, have a look at [this blog post](http://kbroman.org/simple_site/pages/local_test.html) which explains the route via the *github-pages gem*.

# Setting up Google Analytics
Go to your [Google Analytics](https://analytics.google.com/analytics/web/) profile and then navigate to Admin > Property > Tracking Info > Tracking Code. You can find your Tracking ID there (should look something like UA-xxxxxxxx-x). Go to your Jekyll site's `_includes` directory and create a new file called `analytics.html`. In this file, paste the Google Analytics tracking code, which you can find on the same page as your Tracking ID. It should look something like this:
```
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-xxxxxxxx-x"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-xxxxxxxx-x');
</script>
```
Next, save the file and open your `head.html` file, which should be in the same directory. Paste the following code just before the `</head>` tag: {% include analytics.html %}. You're all set and should now be able to view the visitor traffic on your website via Google Analytics.




