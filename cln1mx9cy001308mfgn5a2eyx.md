---
title: "Serving Next.js Apps on your Linux Server"
datePublished: Wed Sep 27 2023 11:00:09 GMT+0000 (Coordinated Universal Time)
cuid: cln1mx9cy001308mfgn5a2eyx
slug: serving-nextjs-apps-on-your-linux-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689276270869/aed04e76-31a2-4aa3-ac70-7b142b2f9761.png
tags: linux, deployment, nextjs

---

In this tutorial, we will explore the process of serving Next.js apps on a Linux server. Next.js is a powerful framework for building server-side rendered React applications. By deploying Next.js apps on a Linux server, we can harness the scalability and flexibility of the platform. This article will provide a concise and practical guide, taking you through the prerequisites, repository cloning, dependency installation, app building, and serving.

## **Prerequisites**

* Node.js is installed
    
* A [Next.js](https://nextjs.org/) app has been written and its source code is in a git repository accessible by the server
    
* You are logged in to the server as a non-root user
    
* If using a virtual machine, port 3000 is forwarded
    

## **Clone the repository**

Use `git clone` to clone the repository into the `~/apps` directory.

> The repository could be located anywhere.
> 
> If it is hosted on the same server as you're currently using, (for example, inside the `~/repos` directory), you can clone it by passing `git clone` the path to the repository.

```bash
git clone ~/repos/my-app ~/apps/my-app
```

Change into the repository.

```bash
cd ~/apps/my-app
```

Install the project's dependencies.

```bash
npm install
```

Disable Next.js telemetry.

```bash
npx next telemetry disable
```

Build the project.

```bash
npm run build
```

Serve the project.

```bash
npm start
```

By default, the app will be accessible on port 3000.

## **Test the Application Server**

Open a web browser and navigate to http://\[your server's IP address/hostname\]:3000. You should see your Next.js app.

> You can navigate to [http://localhost:3000](http://localhost:3000) as long as...
> 
> 1. you're using a virtual machine,
>     
> 2. are working from the host machine, and
>     
> 3. you have port 3000 forwarded
>     

This is a simple setup that isn't fault-tolerant or feasible to manage. I have another article on [managing your Node.js processes using a manager like PM2](https://travishorn.com/how-to-manage-nodejs-processes-with-pm2) that fixes this problem.

Deploying Next.js apps on a Linux server opens up a world of possibilities for scalable and performant web applications. By following the steps outlined in this guide, you can confidently serve your Next.js app on a Linux server, leveraging its powerful features. Armed with this knowledge, you are now equipped to deploy your Next.js app and deliver an exceptional user experience. Happy serving!

Cover photo by [Francesco Ungaro](https://unsplash.com/@francesco_ungaro?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-bird-is-sitting-on-a-rock-in-the-water-qX_HzjwEUhg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).