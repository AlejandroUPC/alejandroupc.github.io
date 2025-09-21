# Starting a blog

_In this, more than ever, fast changing times in tech I decided to set up a blog. I am not sure what will come out of this other than me writing down stuff and trying to explain what I learn._



## Introduction

After looking around seems like github pages might be the best resource, like its simplicity and I dont like this kind of monetized-paywalls like medium and so on.

The list of resources I used so far is:

- [Creating an Engineering Blog with GitHub Pages](https://medium.com/cardano/creating-an-engineering-blog-with-github-pages-4e9bf423c1d0).
- [Creating and Hosting a Personal Site on GitHub](https://jmcglone.com/guides/github-pages/).
- [Build A Blog With Jekyll And Github Pages](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)

This assumes you are qutie familiar already with tools like Git, Github and can understand some basic web development cocnepts using HTML/CSS/Javascript.

## Git & Github

Git is an OSS tool to keep track of the state of a file (VCS, Version Control System) and understand all of its transformations over time, making it easy to jump back and forth between those (think of those university projects that kept getting created `project.doc`, then `project_v2.doc`, `project_v2_final.doc`, but you dont need to create multiple files and can jump back and forth somewhat easily).
It is a very powerful tool, it can really do a lot of things and I probably only use around 5% of its commands but you can find more information in [Git's page](https://git-scm.com/).

Github is a service that (among other things) offers you to host those projects online, so makes it easy to collaborate with other people online (happens a lot in software), there are similar platforms also such as Gitlab.

Although all of those Git + whatever might be confusing, the distinction between the actual technology(Git) and the platforms (Github, Gitlab) offering its services its important, and you need to beceome somewhat familiar with it over your career.


## Github pages

Github pages are a way to create a free website (or subdomain) in their page, its quite tech oriented (given that Github is a tech company) and easy to set up. My personal thoughts on why its great:

- Free.
- You will get familiar with git.
- Simple

## Creating the blog

The first step is quite easy, assuming you have a github account just create a new repository with `yourusername.github.io`, visibility public and I recommend marking the option add README.

After creating it update a file `index.html` on the root folder of the repository (hint, if you don't know git at this point would be great if you pause and learn how to pull a repository locally, add files and upload(push) them, or the least recommended option is to use the UI).

The file could be as simple as:

```html
<!DOCTYPE html>
<html>
  <head>Hello World</head>
  <h1>Welcome</h1>
</html>
```

Now going to `yourusername.github.io` should show a very simple ugly page welcoming you.
Here is where you can get some insipration going on and feel free to use HTML, CSS to make it look fancier (see [Additional resources](#additional-resources)), we will just be covering some very basics below.

## Getting fancy

We will be adding some Cascading Style Sheet (CSS) which can be understood as a way to define styles for HTML documents, modifying fonts, size, color of HTML elements.

In order to avoid having all of our files lying around we will be creating a specific folder for storing our css files and create our first file under `css/main.css`, which will look like:

```ccs
body {
	background-color: "#F0F8FF";
}

h1 {
	font-family: 'Comic Sans MS';
}
```

Now after having this file uploaded (pushed) to your repository, if you wait a little and refresh your page things should look slightly different. You can really get some cool stuff done with just HTML/CSS so I'd recommend you playing a little bit around before if you are not familiar yet.


## Jekyll

Jekyll is a new tech for me, so we will be learning together. 
It seems to be a way to go from plainetxt to some cool websites and blogs, so we are just going to set up some structure in our repository (folder where our website conents are) and it will automatically pick it up and save us some time.
It fits very well with Github pages simplicty where we can just upload the files directly and no need of a database.

One of the advantages is using templates (layouts) so you can save some time avoiding some repetitive tasks, the folder structure is defined [here](https://jekyllrb.com/docs/structure/), I will kinda speed run this but the link to `Creating and Hosting a Personal Site on GitHub` in the [introduction](#introduction) explains it with more details.

An important file is the `._config.yml`, I recommend checking the official docs [here](https://jekyllrb.com/docs/configuration/) and also its [defaults values](https://jekyllrb.com/docs/configuration/default/), as you explore the tool and feel you are missing something go and explore if its already implemented.


```yml
lsi: true
safe: true
gist:
  noscript: false
kramdown:
  math_engine: mathjax
  syntax_highlighter: rouge

```

The next step is creating the `_layouts` folder, where we will create a `default.html` which will be added on every post, so it makes sense for us to define a header and a footer (HTML tags).

The cool thing of the layouts is you can reference then them anywhere from another html file, so we can do the following on our `index.html` file:

```html
---
layout: default
title: This is the first attempt
---
<div>
  <h1>Welcome</h1>
</div>
```
Notice at the top of the file? Yhose are called `Front-matter` in Jekyll and this means every file starting with those characters will be processed by the framework, and basically we are just calling it to use our `layouts/_default` to craete a new HTML page where the content will be replaced in the `{{ content }}`.

## The blog itself

Jekyll has a lot out of the box features to create a blog and allows for customization but the tutorials that we followed will cover the basics to creat a post, a page to index our posts, creating permalinks for our posts and finally RRS feed.

To creat a post layout we go back to `layouts` and create a `post.html` file, something as simple as:


```html
---
layout: default
---
<h1> {{ page.title }} </h1>
<p class="meta">Written the {{ page.date | date_to_string }}</p>

<div class="post">
	{{ content }}
</div>
```

We need to create a directory posts where we will store our posts `_posts/` and now its important we are very strict with the name of the files under and follow the convention as `YYYY-MM-DD-title-of-post.md`, so we can do something like `2025-09-21-first-post.md`with the content:

```markdown
---
layout: post
title: "We try our first post"
date: 2025-21-09
---

This is where we try our first post following mostly jmcglone's tutorial
```

Notice some of the elements between `---`, like title or date, those should be familiar with our `post.html` template.


## Tags

Git, Github, Jekyll, HTML, CCS, Javascript

### Additional resources

- [Git documentation]()
- HTML
- [CSS Mozilla Docs](https://developer.mozilla.org/es/docs/Web/CSS).
- [Jekyll] (https://jekyllrb.com/)
- [Docker] ()
