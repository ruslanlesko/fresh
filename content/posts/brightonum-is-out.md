---
title: "BrightonUM project release"
date: 2020-05-28T18:50:50+03:00
description: "I have created a lightweight user management system"
tags: [Projects]
cover:
    image: "/brightonum_logo.jpg"
    alt: "BrightonUM logo"
---
https://github.com/ruslanlesko/brightonum

I have released BrightonUM as an [open-source project on GitHub](https://github.com/ruslanlesko/brightonum). It is a simple authentication and user management system, which was primarily developed for my other project - [AirPicHub](https://airpichub.leskor.com/). 

Its main feature is that it is lightweight and simple. BrightonUM uses signed JWT access tokens for proving user authentication and does not support the OAuth2 protocol. I have created it specifically for such small projects which don't require sophisticated features like OAuth2 support. Pet projects could also benefit from BrightonUM.

The code is written in Go and Dockerized. The project is released under MIT license, so you can try it in your own projects.
