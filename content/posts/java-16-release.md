---
title: "Java 16 release: non-LTS, incremental upgrade"
date: 2021-03-20T19:02:00+03:00
description: "Some interesting things were introduced, but I recommend waiting for Java 17 LTS version"
tags: [Java]
---
Java 16 just got released. It is not a long-term support (LTS) version, so I don't recommend upgrading your applications to this version, given LTS Java 17 will be available in late 2021. But I recommend at least getting familiar with the changelog since Java 17 will contain them as well.

Most users won't notice various internal improvements, so I've picked the following most interesting changes:

- Introduced Stream.toList() method which will simplify collecting the stream to list operation, which is very common and currently done via Stream.collect(Collectors.toList()) construction. 

- Added jpackage tool for packaging java apps into native installation images (msi/exe for Windows, deb/rpm for Linux and pkg/dmg for macOS). This will make Java desktop frameworks more popular since the installation part will be the same as for native apps. Also I expect the rise of new Java desktop technologies such as Skija: https://github.com/JetBrains/skija

- Added support for unix-domain socket channels. This will allow better local communication since it is faster than using TCP/IP stack for local data transfers.

If you are interested in the full Java 16 changelog, please take a look at this page: https://jdk.java.net/16/release-notes
