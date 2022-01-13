---
layout: post
title: A blog with Jekyll
author: Milan de Ruiter
tags: Jekyll 
---

For a couple of months I was thinking about creating my own blog. Not to become famous in the iOS industry, but more to have a place where I can collect my ideas and learnings.
In my work I write a lot, think about documentation, 'how-to' articles or even proposals. Some of the documents are really work and project related, but some are general about iOS development.

My New Year's Resolution for 2022 is to actually get started with blogging, but how do you start a blog? After some google searches I learned about [Jekyll](https://jekyllrb.com). 

Jekyll is a static site generator written in Ruby by Tom Preston Werner, GitHub's co-founder. Jekyll can convert `Markdown` files into static web pages. 

## Starting a new blog

Starting a blog with Jekyll for me was so easy that I wanted to write an article about. It can be that you are also thinking about blogging, but that you don't want to do all the extra work of building your own blog. 
Yes, you can also start a blog on sites like Medium, but I've noticed that more people are moving to their own blogs. 

To start a new blog with Jekyll, you need to install the Jekyll. As it is a Ruby tool it can easily be installed via:
```
gem install jekyll
```

Once Jekyll is installed, you can create a new blog via:
```
jekyll new blog
```
This command will create a new directory. This directory contains everything you will need for you to get started.
In above example the directory is called blog. 
```
cd blog
```

Now that we have created our blog and that we are in the right directory, we can run it run our blog locally:
```
jekyll serve
```

By default it will start the static page on localhost on port *4000*. But this can be changed just like the title, description and images via the `_config.yml` file.

## Templates

Luckily, there are plenty of Jekyll templates available on GitHub. You can find a lot of templates [here](https://github.com/topics/jekyll-theme). Some templates requires you to do some manual work, include dependencies or move files to certain folders, but others work right away when you press the 'Use this Template' button in Github! As you can see in the footer of the website, this website is using the [Tale](https://github.com/chesterhow/tale/) template. 
