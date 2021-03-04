---
title: "Reactive Java attracts more people"
date: 2020-05-06T16:46:49+03:00
description: "Benchmarks proves benefits of reactive approach in high-concurrent systems."
tags: [Java]
---
[Spring: Blocking vs non-blocking: R2DBC vs JDBC and WebFlux vs Web MVC](https://technology.amis.nl/2020/04/10/spring-blocking-vs-non-blocking-r2dbc-vs-jdbc-and-webflux-vs-web-mvc/)

This recent article on blocking versus non-blocking Java performance proves the right direction in which Java slowly moves: *reactive programming*. New R2DBC driver (reactive successor to JDBC) is at its early stage of development (lack of JPA support), but it already shows significant performance benefits of non-blocking computations.

Please keep in mind that it works only for high concurrent services. Otherwise, reactive programming brings more overhead than benefits. So don't rush to rewrite everything asynchronously, if it is already running well. I am working with RxJava and can say for sure that this will become a standard in high-concurrent applications despite its high learning curve. 

