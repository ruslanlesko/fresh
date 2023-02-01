---
title: "Making News Easier: The Launch of Digsetwave"
date: 2023-02-01T10:30:42+01:00
description: "Releasing Digestwave: my open-source news aggregator"
tags: [Projects]
---
https://digestwave.com

More than a year has passed since [my Digestwave announcement post](/posts/announcing-digestwave/). And today, I am happy to report that the initial version is ready to be used.

[Digestwave](https://digestwave.com) is a news aggregator which provides a clean web interface for reading news from many sites without ads and in a unified UI.

![Digestwave screenshot](/digestwave_screenshot.png)

### Content available

Digestwave has two so-called "editions": international and Ukrainian. This was done intentionally to be able to filter out Ukrainian websites for people who don't understand them or don't interested. The international edition contains content in English that is not location-specific. You can change edition in the top right corner.

Topics across editions are the following: technology, finance, football (only in the ukraininan edition), and programming (only in the international edition).

Please bear in mind that extracting clean content from bloated website HTML is not trivial. That is why sometimes you may encounter weird paragraphs.

### Technology

Web UI is written in vanilla-JS without any libraries or frameworks. Hopefully, this will enable fast interactions even on slow hardware.

Androind and iOS clients are in plans, hovewer I cannot yet provide any specific or approximate dates.

As my other projects, Digestwave is open source. You can find it's souce code on my GitHub: https://github.com/ruslanlesko/digestwave/.
