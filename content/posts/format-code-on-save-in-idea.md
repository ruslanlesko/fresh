---
title: "Automate Java files formatting with IntelliJ IDEA"
date: 2021-09-23T19:00:00+03:00
description: "New updates from Jetbrains bring auto-formatting funtionality."
tags: [Java]
images: ["actions_on_save.png"]
---

IntelliJ IDEA added one cool feature in their [2021.2 release](https://blog.jetbrains.com/idea/2021/07/intellij-idea-2021-2/): the ability to add actions when saving files. The most useful use-case is applying automatic reformatting and optimizing the imports. 

To enable that, go to `Preferences -> Tools -> Actions on Save` and select all required actions.

![IDEA Preferences, actions on save](/actions_on_save.png "IDEA Preferences, actions on save")

Well-formatted code is a key factor in readability in large teams. Unless you are working alone on some pet project, you want consistency in code style across your codebase. With that in mind, I highly recommend this feature for all Java developers.

It is worth mentioning that this functionality is not brand new and previously could be achieved using the ["Save Actions" plugin](https://plugins.jetbrains.com/plugin/7642-save-actions/). I feel sorry for the authors, but now IDEA has it as a built-in feature.

