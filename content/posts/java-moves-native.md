---
title: "Java moves to native. Now with Project Leyden"
date: 2020-05-21T21:44:04+03:00
description: "Project Leyden is one more attempt to run Java natively"
tags: [Java]
---
[Project Leyden Aims to Improve Java Startup Time](https://www.infoq.com/news/2020/05/java-leyden/?useSponsorshipSuggestions=true&itm_source=presentations_about_java&itm_medium=link&itm_campaign=java)

Mark Reinhold, Java architect, recently proposed to start work on a new project to improve Java startup time and performance: Project Leyden. As we know, the best way to do that is to make Java apps run natively. GraalVM offers similar capabilities, but it is more about interoperability with other programming languages. I think the main goal of Project Leyden will be the ability to compile the application for a specific operating system, using its native libraries thus improving startup and runtime. 

It is an obvious movement from Java. And it will be especially useful for deploying Java apps as Docker images.
