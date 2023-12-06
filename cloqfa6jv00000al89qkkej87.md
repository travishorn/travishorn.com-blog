---
title: "Reverse Proxying with nginx"
datePublished: Thu Nov 09 2023 00:00:11 GMT+0000 (Coordinated Universal Time)
cuid: cloqfa6jv00000al89qkkej87
slug: reverse-proxying-with-nginx
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689277404769/90908587-8061-41de-926c-b842a2b389fa.png
tags: optimization, nginx, configuration, reverse-proxy, web-traffic

---

Reverse proxying with nginx is a powerful way to manage and distribute web traffic. In this comprehensive guide, we’ll walk you through the installation process and configuration of nginx as a reverse proxy server. We’ll also cover some next steps you can take to optimize your setup.

## The Why

Why would a system administrator choose to run their Node.js web applications behind a reverse proxy like nginx? After all, they could just set the app to listen on port 80 or 443 and allow users to connect directly. Here are a few good reasons:

* If your application runs on multiple back-end servers, you can leverage a reverse proxy to balance the load between them
    
* HTTPS/SSL is more easily managed and typically runs faster when delegated to a dedicated reverse proxy
    
* Using a reverse proxy can add more versatility to your server. It can be configured for many scenarios, can host multiple apps on the same port, and enables more robust security features
    

## Prerequisites

This guide assumes you already have a web app up and running on the server. It also assumes that the app is listening on port 3000, although you can easily adapt the guide for any port number.

## Install nginx

```javascript
sudo apt update
sudo apt install -y nginx
```

You should immediately be able to visit [http://localhost](http://localhost) and see the "Welcome to nginx!" page.

## **Configure nginx**

Edit the nginx configuration file at `/etc/nginx/sites-available/default`.

Replace the `location /` block with the following:

```javascript
location / {
  proxy_pass http://localhost:3000;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection 'upgrade';
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;
}
```

Save the file.

Test the new nginx configuration

```javascript
sudo nginx -t
```

If successful, restart the nginx service to apply the changes.

```javascript
sudo systemctl restart nginx
```

In your web browser, visit http://\[your server's IP address/hostname\]. You should see your Node.js app.

## Next Steps

* All requests to the web application should go through port 80 (or 443 if you set up HTTPS/SSL later). However, the application is still listening on port 3000. Use a firewall to block external requests to port 3000. [I have a full guide on using nftables to set firewall rules this way](https://travishorn.com/firewall-configuration-with-nftables).
    
* Web sites and applications should be served over HTTPS with SSL. You can configure nginx to handle this. I have a [guide on setting up nginx with HTTPS/SSL](https://travishorn.com/configuring-httpstls-on-nginx-a-complete-guide-for-securing-web-traffic), as well.
    

Congratulations! You now have a fully functional reverse proxy server using nginx. With this setup, you can manage and distribute web traffic with ease. If you have any questions or comments, feel free to leave them below.

Cover photo by [Valentin Vlasov](https://unsplash.com/@aga4ar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/x6C5we9lYik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).