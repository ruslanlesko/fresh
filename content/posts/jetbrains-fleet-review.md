---
title: "JetBrains Fleet review (preview version)"
date: 2022-01-27T19:05:00+03:00
description: "What I discovered during Fleet beta testing"
tags: [Tools]
---

At the end of 2021, JetBrains announced the new application into their IDE line-up: Fleet.
However, this is not really an IDE, but just an editor (with a hidden power).
https://blog.jetbrains.com/blog/2021/11/29/welcome-to-fleet/

It is still in the preview stage, so it may change significantly by the time it will be publicly available.
And I was lucky enough to get my preview version from JetBrains.

## What is Fleet

Roughly speaking, Fleet is an attempt from JetBrains to take over VS Code (or Sublime Text) market. A very fast and lightweight editor.
It is also built on a more-or-less native technology stack, compared to VS Code which runs on top of web technologies.
The UI framework is a home-grown solution ([Skija](https://github.com/JetBrains/skija)) which runs on top of a JVM.
This allowed JetBrains to achieve extremely low typing lag.

However, just making a simple editor would not differentiate it from its competitors.
So JetBrains decided to add one killer feature for it: enabling a smart mode with autocomplete which we see in their other IDEs.
It is disabled by default and can be enabled when you want to have smart features and OK to wait for the editor to "think" the same amount of time as in IntelliJ IDEA.

## My ways of working

First, I'd like to explain how I usually work. As a Java developer, my primary tool is IntelliJ IDEA.
And VS Code is my choice for a secondary editor when I am editing some configs, non-Java scripts, or DevOps stuff (YAML files) that are outside of my primary project.
One of the nice features of VS Code is the ability to open any folder or file directly from the terminal by executing `$ code config.yml`. The editor does not open immediately, but much faster than a heavyweight IntelliJ IDEA.

## My testing setup

I have used the following setup when playing with Fleet:

* Windows 10 Pro 21H2, build 19044.1415
* Fleet 1.0.588

## Expectations vs Reality

Before I got my hands-on Fleet experience, I thought this will be the same UX as other Jetbrains IDEs. Because IDEA, CLion, PyCharm are not very different from each other. They share the same UI, shortcuts, launch configurations, etc.

In reality, Fleet has more things in common with VS Code and Sublime Text than with IDEA. For example, Fleet uses `CMD+P` for file search and actions palette, editing settings via JSON settings file (no UI for settings, just a config file as on the screenshot below). And there is no UI even for run configurations.

![Screenshoot of Fleet settings file](/fleet_settings.png "Screenshoot of Fleet settings file")

All of it gives an impression that apart from other JetBrains IDEs, this editor was built from scratch. And the downside of this is a very unpolished layout (which is totally understandable for a preview version). Margins, icons, fonts, and the size of UI elements feel too tight and hard to navigate. But maybe it is just my subjective impressions. Here is a screenshot of the editor with my blog repo opened where you can look at the UI.

![Screenshoot of Fleet in dark theme editing this post](/fleet_ui.png "Screenshoot of Fleet in dark theme editing this post")

As in the case with VS Code, it is possible to set up the terminal `$ fleet` command to open files from the command line (configurable in Jetbrains Toolbox application, "Generate shell scripts" section).

## First attempt: writing a post in markdown for this blog

The first thing I tried to do with Fleet is to write a blog post since plain text editor fits perfectly fine for this task. I have noticed that the typing lag is invisible, compared to my experience with IDEA and VS Code. It feels the same as with Sublime Text, letters appear on the screen immediately as you type them.
This is cool, but I cannot say that I lacked it during my work. I usually think slower than I can type.

The sad point for me is the lack of Markdown support. Of course, I already know its simple syntax, but basic preview functionality and highlighting are available in VS Code out of the box (if I am not mistaken).
But I am sure that this will be resolved when Fleet will add support for plug-ins.

At this point, I have also discovered that it is not possible to change the font type. But again, this is a preview version.

In all other parts, Fleet behaved as I have expected: a simple, but fast text editor.

## Second attempt: trying out smart mode in a Java project

I wanted to try smart mode. For this purpose, I opened one of my pet projects: [Digestwave (scraper application)](https://github.com/ruslanlesko/digestwave/tree/main/scraper).

After I hit the "Enable smart mode" button, it took Fleet about 2 minutes to index and build my project. Now I have a nice syntax highlighting, autocomplete, and go-to-definition feature. However, I have noticed that this editor does not understand the latest Java language features such as `Stream.toList()` and records. As a result, I have red lines highlighting errors.

![Screenshoot of Fleet highlighting errors](/fleet_smart_mode_fails.png "Screenshoot of Fleet highlighting errors")

But the build itself succeeds. Fleet correctly picked up that my project requires Java 17 and I was able to successfully launch the application by hitting a green 'play' button, just like in plain old IDEA.

![Screenshoot of Fleet running my application](/fleet_run.png "Screenshoot of Fleet running my application")

I must admit that this works much better than I could have expected from a product preview version. But I cannot use it (yet) for my professional Java projects due to the number of customizations and automation features in IDEA.

## Summary

Fleet is a promising editor which is only in a preview version now. It is far from a complete product. I cannot replace VS Code with Fleet due to the lack of plugins (to be an actual polyglot editor, such as VS Code). And the smart mode is not as smart as IDEA yet to be used in professional projects. However, I was able to launch my pet project and the typing performance (low typing lag) is very impressive.
All of that means that we have a very interesting promising editor, which can potentially replace IDE and plain text editor needs with a single application. However, I think that for large projects with complex configurations, IDEA will still be a champion.

I will wait for Fleet to add missing features (plug-in support, editor customization, better Java 17 syntax support, a more polished UI) and will consider trying it one more time. But for now, I'll stick to tools that help me do my work more productive: IntelliJ IDEA and VS Code.