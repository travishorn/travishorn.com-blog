---
title: "Https & Ssl/tls"
datePublished: Tue Aug 05 2014 22:54:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrni79ii0c01ems1dfxj2utb
slug: https-ssl-tls-26fd0d1a6cb7
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409898372/1YpU-B9-J.png
tags: ssl, https, security, tls

---


Before we get started on today’s blog post I’d like to clear up some terminology. When you connect to a web service using HTTPS (Hypertext Transfer Protocol Secure), your browser and server are using a protocol called TLS (Transport Layer Security) in the background. TLS is the successor to SSL. Basically, the named changed between versions.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409897224/eic3Bm-5J.png)

## The web is getting more secure

More and more sites are enabling HTTPS by default. Right now I have a few tabs open:

* Gmail

* Google Calendar

* Google+

* GitHub

* Twitter

I’m connected to all of them through HTTPS. But why do all these popular sites use “always-on” HTTPS? It all comes down to one fact: It’s more secure. OK, but why does the entire session need to use TLS? Wasn’t the web just fine the way it was, only using a secure connection when passing authentication and other sensitive information back and forth?

## The problem

Well, the problem is that when you’re transferring information back and forth to a web service, any device in the same network can view your traffic. I’m oversimplifying here, but that’s generally what can happen. So imagine you and a bunch of people (some of whom might be strangers) all sharing a network. Think of a public place like a hotel, coffee shop, or restaurant. You’re browsing away on your favorite social media site, while everyone else can view that traffic.

The problem of people “sniffing” others’ traffic started to become a problem, and most major sites fixed it by enabling TLS by default. TLS encrypts information using a form of public-key encryption. I will probably go over the details of public key encryption in a later blog post, possibly getting into the details of the mathematics behind why it works. All the average user needs to know is that their data is secure when connected over HTTPS — or at least as secure for all practical purposes.

## Disadvantages

So why haven’t all sites been using always-on TLS this whole time? Performance is the main issue. Check out this [diagram showing packet flows between a client and server using HTTPS](http://msdn.microsoft.com/en-us/library/dd303381.aspx) from Microsoft Developer Network. You can see how complex each request and response can be. Keep in mind the web is stateless, so this packet flow has to reoccur for every request.

If you are doing anything with sensitive data, including authentication, the answer is probably yes. You will have to balance the cost — both in price and performance — to see if it’s worth it to you and your users. Nowadays, users will be looking for that lock icon and/or green address bar, indicating that their connection is secure. If you plan on doing anything serious with your site, you’ll probably want to look for a certificate provider. I like this [pluralsight blog post on the top SSL certificate providers](http://blog.pluralsight.com/top-reliable-ssl-certificates).

Perhaps in a future post, I’ll cover setting up certificates on various platforms.

Stay secure.

*Lock image licensed by Paomedia under [CC BY 3.0](http://creativecommons.org/licenses/by/3.0/)*

*Originally published on my old blog on August 5, 2014.*