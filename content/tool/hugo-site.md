---
title: Hugo Site
date: 2017-11-04T22:12:19-04:00
description: ""
categories: [tool]
keywords: []
menu:
  docs:
    parent: "tool"
    weight: 10
weight: 10	#rem
aliases: []
toc: false
draft: false
---

## Introduction

As you know that you can use gh-pages branch to create document site for each repo, 
there is also a way that you can create a site from your organization and deploy your
site to github.com. Furthermore, you can use hugo to create a static website and deploy
it anywhere you want.  

The following will describe the process with an example site www.networknt.com

## Installation

For hugo installation, please read this [hugo document][].

## Instructions

The detail steps are documented at [hugo][] site. I am just trying to apply the step to 

https://www.networknt.com


### Create a document repo

This repo will contain Hugo's content and other source files. 

We are using light-doc as repo name in networknt organization

### Create a site repo

This repo contains the fully rendered version of your Hugo website. In our case, it 
is called www.networknt.com

### Create docs

Clone both repos to your local and create a document hugo site in light-doc

```
cd light-doc
hugo new site docs
```

This will create a docs folder in light-doc directory. For me I have copied the docs
folder to light-4j as a staring point.

Make sure that you test your content with "hugo serve" and open browser to http://localhost:1313

Once you are happy with the result, kill the server and remove the generated
public folder under docs.

```
cd light-doc/docs
rm -rf public
```

### Create a submodule

```
cd light-doc/docs
git submodule add -b master git@github.com:networknt/networknt.github.io.git public
```
This creates a git submodule. Now when you run the hugo command to build your site to 
public, the created public directory will have a different remote origin (i.e. hosted 
GitHub repository). You can automate some of these steps with the following script.

### Build site

```
cd light-doc/docs
hugo
```

This will build the site and put rendered site into public folder.

### Deployment script

You’re almost done. You can also add a deploy.sh script to automate the preceding steps for you. You can also make it executable with chmod +x deploy.sh.

The following are the contents of the deploy.sh script:

```
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..
```

### Use a custom domain

If you’d like to use a custom domain for your GitHub Pages site, create a file static/CNAME. Your custom domain name should be the only contents inside CNAME. Since it’s inside static, the published site will contain the CNAME file at the root of the published site, which is a requirements of GitHub Pages.

Refer to the [official documentation for custom domains](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) for further information.

[hugo document]: /tool/hugo-docs/
[hugo]: https://gohugo.io/hosting-and-deployment/hosting-on-github/