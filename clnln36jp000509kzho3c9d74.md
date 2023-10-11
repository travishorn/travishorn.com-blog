---
title: "How to Manage Node.js Processes with PM2"
datePublished: Wed Oct 11 2023 11:00:09 GMT+0000 (Coordinated Universal Time)
cuid: clnln36jp000509kzho3c9d74
slug: how-to-manage-nodejs-processes-with-pm2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689276743834/8daae0e9-090c-41c9-bf66-d9b55678c1b4.png
tags: nodejs, pm2, process-management, server-administration, system-startup

---

Node.js is a popular runtime environment for building scalable and high-performance applications. However, managing Node.js processes can be challenging, especially when you have multiple processes running on the same machine. PM2 is a process manager for Node.js applications that makes it easy to manage and monitor your Node.js processes. In this article, we’ll walk through the process of installing PM2, creating a configuration file, launching the processes, and setting them to start on system startup.

## **Setup**

Install [PM2](https://pm2.keymetrics.io/) globally using npm.

```bash
sudo npm install -g pm2
```

Create the file `~/ecosystem.config.js`. Many of the configuration options in this file will depend on your setup. Here are quite a few examples:

```javascript
module.exports = {
  apps: [
    {
      // Example for a basic Node.js app
      name: "[app name]",
      cwd: "/path/to/app",
      script: "node",
      args: "index.js",
      max_restarts: 5,
      log_date_format: "YYYY-MM-DDTHH:mmZ",
    },
    {
      // Example for a Node.js app that you normally launch with `npm start`
      name: "[app name]",
      cwd: "/path/to/app",
      script: "npm",
      args: "start",
      max_restarts: 5,
      log_date_format: "YYYY-MM-DDTHH:mmZ",
    },
    {
      // Example for a Next.js app
      // This one takes advantage of cluster mode to spawn a new instance
      // for every CPU core
      name: "[app name]",
      cwd: "/path/to/app",
      script: "node_modules/next/dist/bin/next",
      args: "start",
      instances: "max",
      exec_mode: "cluster",
      max_restarts: 5,
      log_date_format: "YYYY-MM-DDTHH:mmZ",
    },
    {
      // Example for a SvelteKit app
      name: "[app name]",
      cwd: "/path/to/app",
      script: "node -r dotenv/config build",
      max_restarts: 5,
      log_date_format: "YYYY-MM-DDTHH:mmZ",
    },
  ],
};
```

Make sure you replace `[app name]` and `/path/to/app` with the appropriate values. I use an absolute path to be extra safe.

Also notice that you can place more than one object in the `apps` array if you want to manage more than one Node.js app.

Start the process(es) using PM2.

```bash
pm2 start ~/ecosystem.config.js
```

Open a web browser and navigate to http://\[your server's IP address/hostname\]:3000. You should see your app. If your app listens on a port other than 3000, just use the appropriate port number instead.

Save the currently running configuration.

```bash
pm2 save
```

View the startup script command.

```bash
pm2 startup
```

That command will output a command that looks something like this:

```bash
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u myuser --hp /home/myuser
```

Copy the command.

Paste it into your terminal and press **Enter** to run it.

PM2 will now automatically start the saved Node.js processes at system startup. Try restarting the machine to test it.

If you make changes to the code, the process for updating the app is...

1. Push changes to the repository
    
2. Log into the server
    
3. Pull changes to the cloned (running) app
    
4. Install missing dependencies
    
5. Rebuild the app
    
6. Reload the pm2 processes
    

You can automate steps 2-6 with continuous integration, which I will go over in a future article.

PM2 is a powerful tool for managing Node.js processes that can help you streamline your development workflow and improve the performance of your applications. By following the steps outlined in this article, you should now have a good understanding of how to install and use PM2 to manage your Node.js processes. Whether you’re working on a small project or a large-scale application, PM2 can help you keep your Node.js processes running smoothly and efficiently.

Cover photo by [Omid Armin](https://unsplash.com/@omidarmin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-grassy-field-with-a-bird-in-the-middle-of-it-lzZrtrLEOR0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).