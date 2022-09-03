---
toc: true
layout: post
description: Serving fastpages locally using docker.
categories: [markdown]
title: Docker server
author: Ellen Xu
---

It's often useful to test pages locally before deploying, as it can save time and be helpful for testing quick changes/debugging. In this blog we'll learn how to deploy fastpages locally using Docker.

## Overview of steps

1. Download Docker desktop
2. (For Windows) Set up Docker with WSL
3. Run `make server` in repo
4. Navigate to localhost link (ex. http://127.0.0.1:4000/{repo name}) to view your blog

### 1. Installation

Docker is a lightweight method to build, deploy, run, update and manage containers. Download Docker desktop from following links:

Windows: https://docs.docker.com/desktop/install/windows-install/
Mac: https://docs.docker.com/desktop/install/mac-install/.

Now, each time you want to use Docker, you can start Docker desktop to get it up and running. Make sure you do this before building locally!

### 2. (For Windows) Set up Docker with WSL

For Windows, to set up Docker with WSL use https://docs.docker.com/desktop/windows/wsl/.

### (Optional) Download Docker extention in VSCode

VSCode has an extension too make it easier to manage docker containers and images directly in your IDE. To download, go to the Extensions tab of VSCode (left bar, or Ctrl+Shift+X), search "Docker" and download the extension from Microsoft. You should now see a docker icon on the left bar, which you can click on to see your Docker connections.

### 3. Run `make server` in repo

In the local repository, make sure you are cd'ed to the base/root directory (i.e. where the Makefile is). For me that was the ap-csa-fastpages directory.

![image](https://user-images.githubusercontent.com/56745453/186964001-45e37d26-45b0-484d-bac6-b85b67cb2ffb.png)

The Makefile contains common aliased commands for building and serving your fastpages locally. If you look inside the file or run `make` in terminal, you can see what docker commands it is running.

![image](https://user-images.githubusercontent.com/56745453/186964281-4c238041-0e9e-4319-affa-5d0aebe084b3.png)

Specifically, we are interested in the `make server` command, which will build the container the first time and start the Jekyll server. If you remember from AP CSP, Jekyll is what powers Github pages behind the scenes. You can build your GitHub Pages site locally using Jekyll to preview and test changes to your site.

#### Special steps for Windows

The first time, you might get this issue:

![image](https://user-images.githubusercontent.com/56745453/186963057-bb16c926-33f5-41cb-abe1-65886678f477.png)

If that's the case, make sure this setting is toggled on in Docker desktop settings:

![image](https://user-images.githubusercontent.com/56745453/186963251-602a4073-caab-40ca-8441-55be64d9c7f7.png)

It should ask you to reload and if you run the command again it should work now.

### 4. View blog

After running, make look for link to run Server. For me, this was http://127.0.0.1:4000/{repo name}:
> Server address: http://127.0.0.1:4000/

To stop server and build again after making edits:
> Server running... press ctrl-c to stop.

Repeat make server if you have made updates to your blog and want to serve locally again.

![image](https://user-images.githubusercontent.com/56745453/186968485-a2d02d10-d53a-4b88-b6b1-bbcc2f69d1cc.png)

#### Add to .gitignore

A side effect of building locally is that it converts all the .ipynb and .docx files to .md. This means that some files will have duplicates after building -- one in .ipynb or .docx, another in .md form.

To avoid duplicates when pushing to github, add the files to your .gitignore (courtesy of Mr. M for pointing this out):

```
# Ignore from local build

images/copied_from_nb/  # images folder
_posts/2022-06-14-Curriculum-Map-David--2021-2022.md    # example of markdown files from build
_posts/2022-07-06-PBL-FE-js_tutorial.md
_posts/2022-08-15-TP100-anatomy.md
_posts/2022-08-15-TT000-windowsinstall.md
_posts/2022-08-22-TP110-primitives.md
_posts/2022-08-22-TT110-anthony_and_sahil.md
_posts/2022-08-22-TT110-bash_tutorial.md
_posts/2022-08-29-TP120-using_java_objects.md
_posts/2022-09-05-TP130-boolean_ifs.md
_posts/2022-09-05-TT130-rapidapi.md
```

### Resources

https://github.com/fastai/fastpages/blob/master/_fastpages_docs/DEVELOPMENT.md
