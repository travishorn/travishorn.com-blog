---
title: "Firewall Configuration with nftables"
datePublished: Wed Nov 22 2023 12:00:12 GMT+0000 (Coordinated Universal Time)
cuid: clp9pq6yh001b09l74wih6q6d
slug: firewall-configuration-with-nftables
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689277636192/9f4c1a31-816e-4a9f-a5de-85e39fac03a7.png
tags: security, debian, networking, firewall, nftables

---

Firewalls are an essential part of network security, and nftables is a powerful tool for configuring them. In this article, we’ll explore how to configure nftables. We’ll cover everything from enabling nftables to adding new rules and allowing common ports. This guide has everything you need to get started.

There is no shortage of firewall configuration guides online for Linux. But most of them use the older (albeit more widespread) iptables or the front-end firewalld that uses some other firewall software behind the scenes. I am using a Debian Linux server which comes with nftables installed by default. Rather than installing extra tools, I wanted to use what the official distribution supported.

Here is the very basic nftables configuration I have been successful with. It is located at `/etc/nftables.conf`

```javascript
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
        chain input {
                type filter hook input priority filter;

                # Allow loopback (local connections)
                iifname lo accept

                # Allow established/related
                ct state established,related accept

                # Allow incoming pings
                ip protocol icmp limit rate 1/second accept

                # Allow SSH and HTTP
                tcp dport {ssh,http} accept

                # Drop everything else
                drop
        }
        chain forward {
                type filter hook forward priority filter;

                # Disallow forwarding
                drop
        }
        chain output {
                type filter hook output priority filter;

                # Allow all outgoing traffic
                accept
        }
}
```

Notice how I am disallowing all incoming traffic except pings, SSH and HTTP.

Enable the nftables service so it starts when the machine starts.

```javascript
sudo systemctl enable nftables
```

Start the nftables service now

```javascript
sudo systemctl start nftables
```

If the service is already running and you just want to apply changes you recently made to the configuration, just restart the service.

```javascript
sudo systemctl restart nftables
```

## **Add a New Firewall Rule**

Any time you want to allow traffic for a new service on a specific port, you must add a new firewall rule.

Edit the nftables configuration file located at `/etc/nftables.conf`

Find the line that looks like this:

```javascript
tcp dport {ssh,http} accept
```

Add the new port into the comma-separated list inside curly braces. For example, if you want to add a rule that allows port 3306, the line will look like this:

```javascript
tcp dport {ssh,http,3306} accept
```

Some ports have aliases (like `ssh` and `http`) that nftables recognizes.

Restart nftables to apply the new rules.

```javascript
sudo systemctl restart nftables
```

## **More Rules**

Here are some common ports on which you may want to enable incoming traffic:

* SSH, port `22`, alias `ssh`
    
* HTTP, port `80`, alias `http`
    
* HTTPS, port `443`, alias `https`
    
* MariaDB, port `3306`, alias `mysql`
    

Configuring a firewall can be a daunting task, but using this guide as a starting point, it doesn’t have to be. We’ve covered everything you need to know to get started with nftables. From enabling it to adding new rules and allowing common ports, you now have the knowledge to configure your firewall with confidence. So if it comes installed in your Linux distribution, why not give it a try before you decide to install additional tools? If you're like me, it may be all you need.

Cover photo by [Don Kaveen](https://unsplash.com/@donkaveen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/F0CTHqaZth0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).