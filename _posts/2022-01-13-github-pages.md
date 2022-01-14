---
layout: post
title: A blog with Github pages
author: Milan de Ruiter
tags: Jekyll GitHub 
---

In previous article [A blog with Jekyll](/2022-01-12/a-blog-with-jekyll) we have learned how to make a blog and run it on your local machine. But, what is a blog when it is only accessible from your machine. 

One way to make the blog accessible and put it online is to get a domain and upload your website. Another way is to make use of [Github Pages](https://pages.github.com).
As the header on the Github pages indicates, it will host your website from your github repository and automatically updates it on each change.


## Getting started

To start with Github pages, you only need to have a github account. If you do not have one already, you can make a free account via their website.
Once you have logged in to your Github account, you can make a new repository. When creating the repository, you have to have to make sure that the name of your repository is following the following structure:
```
<github-username>.github.io
```
where `<github-username>` should be replaces with your github username.

Now that the repository is created we only have to link our blog (from previous article) to our repository. 

```
git remote add origin https://github.com/<github-username>/<github-username>.github.io.git
git branch -M main
git push -u origin main
```

Once pushed you can visit your personal github page via: `<github-username>.github.com`.

<b>Note</b> that it can take some time for it to load or to refresh the contents of the page. Especially when working with large or a lot of images. 
