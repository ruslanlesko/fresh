---
title: "Trying GitHub Actions"
date: 2020-10-18T13:00:00+03:00
description: "First look at the new GitHub functionality"
tags: [GitHub]
featured_image: "/gh_actions.jpg"
images: ["gh_actions.jpg"]
---

A few months ago, GitHub rolled out their own CI solution called GitHub Actions. Since I host my PalermoPG and BrightonUM projects on GitHub, I decided to try it. 

What is CI? Continuous Integration (CI) is a development practice where developers integrate code into a shared repository frequently, preferably several times a day. It also involves verification by an automated build and tests. While automated testing is not strictly part of CI, it is typically implied.

GitHub Actions provides that automation. Although, GitHub is the last of the big source code hosting platforms that introduced a built-in CI tool. Bitbucket and Gitlab had it for years. Perhaps, acquisition by Microsoft had sped up that process.

Enabling Actions was easy and intuitive. To do that, go to the Actions section on top of your repo and click "Setup this workflow" for the suggested programming language (automatically detected based on your codebase). Next, you need to customize the build and test commands according to your project. And now you ready to go! Just do a commit to the master branch or merge a PR, and you'll see the working pipeline.

![Initial page of GitHub Actions](/gh_pipeline.jpeg "Initial page of GitHub Actions")

As an additional feature, you can add a CI badge to your readme to show the current build status. It can be done from a 'more' button on a specific workflow:

![Creating the status badge](/gh_create_badge.png "Creating the status badge")

![Build status badge](/gh_badge.png "Build status badge")

I do recommend enabling Actions if you haven't already. At least it will add visibility of the current status of your code, whether it is working or not. Also, it may help with the PR review. PR page will show how changes affect build status. 

With all said, I am happy to see that GitHub finally added pipelines. Now, this makes GitHub the best place to host your project.