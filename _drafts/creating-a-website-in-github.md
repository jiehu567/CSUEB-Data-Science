---
layout: post
description: How to create a website using github pages 
title: Creating a Webpage using Github Pages
---

One of many hidden gems that Github provides to its users is the ability to host personal websites. The webiste you are on at the moment is being hosted at github, and with the power of git behind it updating and managing a site is veru simple and straightforward.

In this article we will go over creating such a site using github pages as well as a Ruby gem called jekyll. Let us begin.

### 1. Prerequesites

First things first lets take care of some prereqs.

#### Ruby

First we need to install Ruby to do so we do the following:

```bash
sudo apt-get install ruby-full
```

#### Jekyll

Next we install jekyll, a static website builder.

```bash
sudo gem install jekyll
```
Lastly we need to install a javascript runtime on the system there a number to choose from but I will go with Nodejs

```bash
sudo apt-get install nodejs
```

### 2. Create a Website Locally

With all the prereqs covered we can create a website hosted locally to do this we use jekyll.

```bash
jekyll new myWebsite
```

here one would replace `mywebsite` with an appropriate name of choice. Jekyll will then produce a directory with this name and in it a set of directories which we will look into in a bit. Let us now navigate to this directiry and do the following:

```bash
jekyll serve
```

here we are instructing jekyll to locally serve the website, the output of the command will yield:

![jekyll serve image](/ERGZ/assets/create-site-1.png)

if we now look at the server address, this is a clickable link, doing so will take us the locally served version of the website we just created. Whenever we make changes to any files in the directory these will be regenerated on this locally served version of the website.

### 3. Configuring the Site

Now that we have the website up and running on our local machine we can start editing some files and creating posts.

#### Create Your First Post

If you navigate to the directory called `myWebsite` that was created by jekyll you will find a folder titled `_posts`. All of the posts you want to create must go here, moreover they must be titled as `year-month-day-title-of-post.md`, replacing appropriately, and where the extension `.md` stands for markdown, the language we will be writting all of the posts in.

Lets create a post called `Welcome` to do this you can use the text editor of your choice and create a new file in the `_posts` directory titled `2015-05-28-Welcome.md`.

Every post you create must have the yaml header, a piece in the posts that tells jekyll how to format the page. It will look like this:

```
---
layout: post
title: Welcome
---
```

here you are telling jekyll that the format of the markdown is of a post, and has the title `Welcome`.

#### How write Markdown

Creating a webpage using markdown is very simple. There are few basic things that one must know, all else is simple search away, we now present some basics.

**1. Headings**

In markdown we use `#` to indicate the level of heading, so a single `#` is equivalent to `<h1></h1>` in html.

`# Heding 1`

# Heading 1

`## Heading 2`

## Heading 2

`### Heading 3`

### Heading 3

`#### Heading 4`

#### Heading 4

To emphasize text we use `**text**` to bold text, and single `_text_` for italics.

To create a snippet of code we can either do text enclosed vy single ` or we can do block of code with text enclosed by triple grave.

To view more markdown documentation please visit [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

