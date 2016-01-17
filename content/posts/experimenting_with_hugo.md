+++
categories = [ "development"]
date = "2016-01-16T15:53:26-02:00"
tags = ["experiment", "hugo"]
title = "Experimenting with Hugo"

+++

# Experimenting with Hugo

This is my first post and I'm going to talk about how I created the site. Last time I decided to create anything like a blog it was 1999. I'm used to work with dynamic webpages with database access upon request, since 1999 websites development has changed from static files only to dynamic webpages and now it's coming back to static file again, to improve user experience by being as fast as it can possibly be. The difference from 1999's website development, now a days we have tools to dynamically create static files and that's what [Hugo](//gohugo.oi/) does. There are many other tools like Hugo, a popular choice is [Jekyllrb](http://jekyllrb.com/), I choose Hugo because it seemed simple and fast enough.

In case you want to see the original files I'm currently using for this experiment, I created a [Github repository](https://github.com/vhugo/vhugo.github.io-hugosrc).

## QuickStart
Hugo has a good [documentation](http://www.gohugo.io/overview/introduction/), in the [quickstart section](http://www.gohugo.io/overview/quickstart/) you can create a website under 2 minutes.

{{< youtube w7Ft2ymGmfc >}}

If you watch this video will know the basics to get started.

## Themes and layouts files
After the quickstart, I knew how to create site, add content and create site files, just needed to find a theme I like and start writing content. I wanted something clean and simple, so after looking at most of themes in [Theme Showcase](http://themes.gohugo.io/) I found [Cocoa](http://themes.gohugo.io/cocoa/) developed by [Nishanth Shanmugham](https://github.com/nishanths). To install it I just clone the repository into themes directory in the project root directory. It looks something like this:

```
cd project-root-dir
git clone https://github.com/nishanths/cocoa-hugo-theme.git themes/cocoa
```

Even though the theme is good, I still wanted to make some changes without touching the original source, so I added some new files into [layouts folder](https://github.com/vhugo/vhugo.github.io-hugosrc/tree/master/layouts)  which is used instead of the ones in the theme.

## Github Pages
I publish the site using Github, I just created a new repo called `vhugo.github.io` and push all files from my Hugo project's public folder. if you want to know more about this check [Github Pages](https://pages.github.com/).

## Editors for code and markdown content
As a developer I love text editors of all kind, but don't like fancy IDEs as much. For years I used [TextMate](https://macromates.com/) and when [SublimeText](http://www.sublimetext.com/) showed up and I felt the need to switch editors, been using it ever since. There's a new editor emerging from the GitHub team called [Atom](https://atom.io), still miss a lot of packages that I'm used to on SublimeText, but it looks promising. SublimeText has some packages to help edit markdown, but I found a MarkDown editor called [Writed](http://writed.io/), loved it.


