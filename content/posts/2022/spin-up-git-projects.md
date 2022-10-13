---
title: Spin up Git Projects in Cloud within Seconds with Gitpod
date: 2022-10-12
image: /assets/images/2022/gitpod.png
---

If you are looking to spin up your git projects from Github, Gitlab, and Bitbucket in seconds without installing it locally then this post might be helpful. We will spin up an Express app and a server is up in seconds with the help of Gitpod.

## About Gitpod
 [Gitpod.io](https://gitpod.io) is a tool that can provide fully initialized, perfectly set-up developer environments for any kind of software project hosted in GitHub, Gitlab, and Bitbucket right in your browser.

## Features of Gitpod

- Spin up projects in seconds in the cloud
- Remote development without friction
- Works on my machine - and yours
- Multi track development with ease
- Share running workspaces for pair programming
- Code anywhere, on any device (having browser)

## Running your project in Gitpod

### 1. Prefixing URL

To run any project in gitpod, you need to prefix with `gitpod.io/#`, It's as simple as that. For example, if you need to run a GitHub repo `https://github.com/geshan/nodejs-sqlite` in gitpod you simply prefix the URL. The prefixed URL for the above repo would be `gitpod.io/#https://github.com/geshan/nodejs-sqlite`. It will bootstrap all the prerequisites, install all the dependencies and you are ready to use the app. The workspace in Gitpod is similar to that of Visual Studio Code. Attached screenshot of the workspace in Gitpod.


![Gitpod Repo](/assets/images/2022/gitpod_repository.png)

### 2. Installing Browser Extension

Gitpod has browser extension for [Chrome](https://chrome.google.com/webstore/detail/gitpod-always-ready-to-co/dodmmooeoklaejobgleioelladacbeki) and [Firefox](https://addons.mozilla.org/en-US/firefox/addon/gitpod/) that you can install in your browser which will add a button in each Github repository page. You can click the Gitpod button right of Code button and you can start running your project on Gitpod

![Gitpod Repo](/assets/images/2022/gitpod_browser_ext.png)

### 3. Adding Open in GitPod button in your Project

You can also add `Open in Gitpod` button in your project markdown just like below with this markdown: 
`[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#<your-repository-url>)`

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#<your-repository-url>)

here, your-repository-url can be something like: https://github.com/gitpod-io/website or any of your Repository URLs.

## Conclusion

We learned how to spin up fresh, automated dev environments for each task, in the cloud, in seconds improving our productivity. It removes the hassle to clone the repo in your machine, installing dependencies, and running your project.


