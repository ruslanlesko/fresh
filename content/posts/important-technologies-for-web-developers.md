---
title: "Important technologies for web developers"
date: 2021-11-22T20:05:00+02:00
description: "A structured list of useful articles for web developers"
tags: [Web]
---
Sometimes it can be hard to find a structured set of articles about important concepts and technologies in web development. And sometimes people just don't know what to search for when they want to have a deeper understanding of the web in general. My teammate recently asked me for some good articles about web development and I struggled to provide an answer immediately. That is why I decided to compile a decent (but not exhaustive) list of important technologies (with corresponding articles) for understanding web development better. I am a back-end software engineer and I am not trying to cover all of the web in this article. However, I think that these concepts will make every developer way more familiar with the way web applications work.

## Pre-requisites

This article is not for beginners. I assume you already have some experience in programming and intermediate knowledge of client-server architecture for web applications.

## Types of web pages (dynamic, static, SPA)

Web pages that are rendered in the browser can be delivered in different ways. For example, this blog is a static website, generated using Hugo: a popular static site generation tool. I rarely post something new here, so this approach fits me perfectly fine. I regenerate the site only when I need to add a new post. It can be safely cached by browsers. It also does not require a running server, because all content is static (technically, there is a server somewhere, but many cloud providers have automated solutions for static site hosting).

But many sites are not read-only and act like applications. For this case, you may consider a different model.

Some of the mature enterprise software was built using JSPs (Java Server Pages), which is an example of a server-side rendering approach. In this case, the back-end application is also responsible for building an HTML page depending on the context and user data.

Modern web apps have strictly divided front-end and back-end solutions that communicate to each other using REST API. And a web page is usually delivered in form of SPA (Single-Page Application). The benefit of this approach is clear: there is no coupling between front-end and back-end codebases.

#### Articles
- https://medium.com/codex/web-design-patterns-ssr-ssg-and-spa-fadad7673dfe
- https://academind.com/tutorials/dynamic-vs-static-vs-spa

## Load Balancing

Every scalable application has a load balancer. This concept implies having a single URL that resolves to different servers depending on the load. Load balancing is invisible for the client: it should not distinguish a load-balanced app from the one which is hosted on a single server.

Load balancing can be achieved by various tools, depending on the scale and type of your infrastructure (AWS, Nginx routing, Kubernetes, etc.).

#### Article
- https://www.ibm.com/cloud/learn/load-balancing

## SSL and HTTPS

Do you see a lock icon nearby the URL for this page? It means this connection is secure and my blog supports HTTPS. Even if your web application works only in a read-only mode, most browsers will mark it as a dangerous one unless you'll add support for HTTPS. And this is fair, looking at the fact that nowadays it is easy to obtain free SSL certificates ([Let's Encrypt](https://letsencrypt.org)) and automate the process of their regeneration ([Certbot](https://certbot.eff.org)).

But how do SSL and HTTPS work? And what level of security can you expect with it? There are no mysteries and many good articles can help you to understand that. I like the series of short posts by Cloudflare about SSL, SSL certificates, HTTPS, TLS handshake, and other more detailed posts available on these pages.

#### Articles
- https://www.cloudflare.com/learning/ssl/what-is-ssl
- https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate
- https://www.cloudflare.com/learning/ssl/what-is-https
- https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake

## SSL Termination

I hope now you are convinced that your apps have to support HTTPS, but where to handle it? A good practice is to decouple SSL handling from the actual application. In other words, your app should be serving plain HTTP traffic. But there would be some proxy in front of it, which is exposed to the outside world as an HTTPS server. It allows using existing solutions (for example, Nginx or Apache server) as a proxy that supports SSL and keeps your app simple. You will also be able to run the application locally on plain HTTP since you only need SSL in deployed environments.

This concept is called SSL Termination (you handle HTTPS traffic on the edge of your infrastructure and proxy plain HTTP requests to the application in a private network). Usually, SSL termination happens on the load balancer.

#### Article
- https://raghumb.gitbooks.io/a-guide-to-software-architecture/content/infrastructure_concepts/ssl_termination_pass_thru.html

## Cross-Origin Resource Sharing (CORS)

CORS rules are enforced by every modern browser. That's why you cannot skip it during the server-side implementation. CORS is controlled mainly by the `Access-Control-Allow-Origin` header and `OPTIONS` HTTP method. I like the following article by Mozilla, which explains CORS concept in details.

#### Article
- https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

## HTTP Caching

Caching in general is a complex thing. And the current approach used by browsers is very complicated and non-obvious. For example, contrary to CORS, there are a lot more headers involved: `Cache-Control`, `ETag`, `Last-Modified`, `If-None-Match`, and others. But proper caching can improve the user experience of your web app a lot. To understand it better, I recommend reading this article from Mozilla on HTTP caching.

#### Article
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching

## JSON Web Token (JWT)

JWT is an industry standard for transmitting a digitally signed set of information. Primarily used for authorization purposes. The main feature of JWT is the ability to verify the digital signature to be sure that the token was issued by the trusted issuer and that contents of the token were not modified during transmission. There are many libraries available for different programming languages to work with JWT tokens.

#### Article
- https://jwt.io/introduction

## OAuth 2 

Probably, the most complex topic on this list. Have you ever seen buttons like "Login with Google" or "Login with GitHub" on sites that are not Google or GitHub? This is OAuth2: an authorization framework that enables 3rd party login by providing limited scope of user data (previously authorized by user) to the client application. Please visit this page by Digital Ocean for a detailed explanation of the OAuth2 flow.

#### Article
- https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2

## What's Next?

If you have studied and understood all of the articles above, you have become a more senior web developer. Congrats! However, as I mentioned in the beginning, this list is not a complete set of web technologies. It would be impossible to enumerate all the various things which are used in a modern web. This is rather a good "starter pack". And web development in general is a journey, during which you'll discover more concepts and new approaches.

If you want to gain more advanced hands-on experience, I recommend playing with Nginx: a powerful HTTP server and proxy. Start by [installing](https://www.javatpoint.com/installing-nginx-on-mac) and setting up Nginx to [host a simple static website](https://www.javatpoint.com/nginx-serving-static-content) on your machine. And then [set up a reverse proxy](https://linuxiac.com/reverse-proxy-with-nginx) to some of your web apps. It will give you more understanding of how HTTP servers and reverse proxies work.