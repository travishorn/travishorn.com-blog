---
title: "Configuring HTTPS/TLS on nginx: A Complete Guide for Securing Web Traffic"
datePublished: Wed Dec 06 2023 12:00:12 GMT+0000 (Coordinated Universal Time)
cuid: clptpw4gp000408ia5p363oi2
slug: configuring-httpstls-on-nginx-a-complete-guide-for-securing-web-traffic
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689621816795/a0a0049f-5b80-43fb-ae45-aed1387738ce.png
tags: ssl, https, nginx, tls, websecurity

---

Today, online security is paramount. Configuring HTTPS/TLS/SSL on your web server is crucial to protect sensitive data and establish trust with your users. By implementing a robust certificate management system, you ensure that all communication between your server and clients is encrypted, preventing unauthorized access and data breaches. In this comprehensive guide, I'll walk you through the process of configuring your nginx server using certbot and Let's Encrypt. I'll also provide an alternative in which you can generate a self-signed certificate.

## **Prerequisites**

Before you can enable TLS on your server, you must first have a registered domain name. One of the main functions of an TLS certificate is to verify that the server sending HTTPS traffic owns the domain name (or, "is who they say they are"). You can purchase a domain name from registrars like [Namecheap](https://www.namecheap.com/), [GoDaddy](https://www.godaddy.com/), [Bluehost](https://www.bluehost.com/), and many, many others.

However, if you do not have a registered domain name, you can still enable TLS on your server with a self-signed certificate. This isn't a perfect solution, though, because all modern browsers will warn the user that the site they're visiting is using a certificate that they signed themselves. They will force the user to read the warning before continuing as a security measure.

If you want to use a self-signed certificate, there is a section below that guides you through the process which you can skip to.

## **Prepare your nginx Configuration**

Edit `/etc/nginx/sites-available/default`.

Find the line that starts with `server_name` and replace the `_` character with the domain name the server will use. You can configure multiple domains by separating them with a space.

```bash
server_name example.com www.example.com
```

Save and close the file.

Verify the nginx configuration.

```bash
sudo nginx -t
```

If all is well, reload nginx.

```bash
sudo systemctl reload nginx
```

## **Use Certbot to enable HTTPS**

Use Certbot if your server is public-facing and you are ready to install a real certificate signed by [Let's Encrypt](https://letsencrypt.org/) (a Certificate Authority which issues certificates for free). Skip to the next section if you just want to use a self-signed certificate.

Install `certbot` and `python3-certbot-nginx`.

```javascript
sudo apt update
sudo apt install -y certbot python3-certbot-nginx
```

Generate the TLS certificate.

```javascript
sudo certbot --nginx -d example.com -d www.example.com
```

You will be asked some questions, including your email address. Answer the questions to continue.

Certbot automatically generates the certificate and configures nginx for you. It also sets a systemd timer that runs periodically that will automatically renew your certificate before it expires.

With the certificate installed, you should be able to visit your domain using `https://` and see your web app. You can also visit it via `http://` and you should be automatically redirected to the `https://` URL.

> Make sure you have allowed incoming traffic on port 443 in your firewall's configuration.

## **Self-Signed Certificate**

If you aren't ready to use a real certificate signed by an authority, you can use a self-signed certificate instead. Visitors to the site will receive a warning about the invalid certificate, but they can bypass it, making this a good option for testing and development.

Make a directory to store the certificate files.

```javascript
sudo mkdir /etc/nginx/tls
```

Change into that directory.

```javascript
cd /etc/nginx/tls
```

### **Generate a Certificate**

If you already have a certificate you want to use, you can skip this step.

```javascript
sudo openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt
```

You must answer a few questions about the certificate being generated. Most important is the common name. If you're using a custom domain name to reference this server, use that. For example `myserver.dev`. Otherwise, just use something like `localhost`.

Two new files, `localhost.crt` and `localhost.key` are generated at `/etc/nginx/tls`.

### **Self-Signed Configuration**

Edit `/etc/nginx/sites-available/default`.

Above the existing `server {}` block, add another, new `server {}` block like so:

```javascript
server {
    listen 80;
    return 301 https://$host$request_uri;
}
```

In the lower, already-existing `server {}` block, remove the first two lines (they both start with `listen`). Replace them with the following:

```javascript
server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/tls/localhost.crt;
    ssl_certificate_key /etc/nginx/tls/localhost.key;

    # ...
}
```

> Note that the configuration file uses the term "SSL" for backward compatibility. Rest assured what we're enabling here is the more updated, more common, and more secure TLS.

Save and close the file.

Test to make sure the nginx configuration is valid.

```javascript
sudo nginx -t
```

If no errors appear, restart the nginx service.

```javascript
sudo systemctl restart nginx
```

With the certificate installed, you should be able to visit your domain using `https://` and see your web app. You can also visit it via `http://` and you should be automatically redirected to the `https://` URL.

> Make sure you have allowed incoming traffic on port 443 in your firewall's configuration.

Since you're using a self-signed certificate, your browser will probably warn you that the certificate is invalid. You can usually bypass this by clicking **Advanced** and **Proceed** or something similar.

Securing your web traffic with HTTPS/TLS on nginx is no longer an option but a necessity. By following the steps outlined in this guide, you can safeguard your users' data and boost your website's credibility. Whether you choose to obtain certificates from Let's Encrypt, generate self-signed certificates for local testing, or leverage other certificate authorities, implementing TLS on nginx ensures the confidentiality, integrity, and authenticity of your online interactions. Don't compromise on security; take the necessary steps to protect your server and users.

Cover photo by [Woliul Hasan](https://unsplash.com/ja/@shotbywoliul?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/W4lO7MMna6w?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).