---
title: "Just for fun: The LOL ID"
datePublished: Thu Feb 14 2019 11:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckroxseo50pemfws1h4zee5pp
slug: just-for-fun-the-lol-id-89f1839c01f5
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409537732/DwNA81sQe.png
tags: javascript, web-development

---


What type of unique identifier should you use for your next data-backed project? Incremental integers? UUIDs? A URL-friendly short ID? I present: the LOL ID.

```
'lxxxxxxxxl'
  .replace(/x/g, () => Math.random() > 0.5 ? 'l' : 'o');
```

%[https://codepen.io/travishorn/pen/BOwPjb]

This code generates a random ID that starts and ends with “l” and contain eight random “o”s or “l”s.

Some random LOL IDs generated with this code:

* lloollooll

* lolollloll

* lololloool

* loolololll

* loloooolol

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409535237/oalyDJZgZ.png)

### Disclaimer

Don’t actually use this code for anything, ever. The IDs are not guaranteed to be unique by a looong shot.