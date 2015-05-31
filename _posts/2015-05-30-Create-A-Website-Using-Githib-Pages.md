---
layout: post
description: How to create a website using github pages 
title: Creating a Webpage using Github Pages
---

One of many hidden gems that Github provides to its users is the ability to host personal websites. The webiste you are on at the moment is being hosted at github, and with the power of git behind it updating and managing a site is very simple and straightforward.

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

here one would replace `mywebsite` with an appropriate name of choice. Jekyll will then produce a directory with this name and in it a set of directories which we will look into in a bit. Let us now navigate to this directory and do the following:

```bash
jekyll serve
```

here we are instructing jekyll to locally serve the website, the output of the command will yield:

![Jekyll Serve myWebsite](/CSUEB-Big-Data/assets/create-site-jekyll-serve.png)

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

#### How to write Markdown

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

To create a snippet of code we can either do text enclosed by single ` or we can do block of code with text enclosed by triple grave. Morever we can do syntax highlighting by specifying the language after the first set of graves.

To view more markdown documentation please visit [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

### 4. Set Up Github

Once we have the site ready to go locally it is time to push it to github for hosting and for others to view. 

#### 1. Create A Repository

We do this the old fasion way at github 

![Create Repo](/CSUEB-Big-Data/assets/create-site-create-repo.png)

#### 2. Link Repo to Jekyll Folders
Next we want to link this repository to the folder jekyll created for us, once again we do this they way we usually would. (Follow githubs directions)

#### 3. Create `gh-pages` branch

Now we want to create a new branch in the repository called `gh-pages` we do this by going to the overview of the repo and clicking branch 
We look first hoover over:
![new branch](/CSUEB-Big-Data/assets/create-site-new-branch3.png)

![new branch](/CSUEB-Big-Data/assets/create-site-new-branch.png)

Enter gh-pages

![new branch](/CSUEB-Big-Data/assets/create-site-new-branch2.png)

we want to do the same in the terminal so lets us do:

```bash 
git checkout -b gh-pages

# to see al branches
git branch

#push 
git push origin gh-pages
```

Lastly we want to set `gh-pages` as the default branch, we do this from the github website

Look for the following and click the bottom link for settings

![new branch](/CSUEB-Big-Data/assets/create-site-default-branch2.png)

And set default branch to `gh-pages`

![new branch](/CSUEB-Big-Data/assets/create-site-default-branch3.png)

Having done this we can now delete the master branch since we do not need it any longer. So we look for the following in the repo overview and click on branches.

![new branch](/CSUEB-Big-Data/assets/create-site-default-branch.png)

Now we simply delete the master branch 

![new branch](/CSUEB-Big-Data/assets/create-site-delete-master.png)

### 5. Finalizing

Now we can go back to the settings of the repo on github and we should see the following:

![new branch](/CSUEB-Big-Data/assets/create-site-final.png)

clicking it will take you to your new website.

Congragulations!
