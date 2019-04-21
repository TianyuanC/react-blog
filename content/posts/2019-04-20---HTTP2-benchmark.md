---
title: How fast is HTTP/2
date: "2019-04-20T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/how-fast-is-http2/"
category: "Web"
tags:
    - "Web"
description: "Are you confused about HTTP/1.1, HTTP/2 and HTTPS 🤯. Yes? Let's mix them together and see what happens next lol"
---

## TL;DR

-   HTTP/2 can reduce latency by multiplexing
-   A quick benchmark between HTTP/1.1 and HTTP/2

## HTTP/2 with Node

First of all HTTP/2 needs to be enabled from the backend, and in the following example I will use `Node.js`

```javascript
const http2 = require("http2");
const fs = require("fs");

http2
    .createSecureServer({
        key: fs.readFileSync("localhost-privkey.pem"),
        cert: fs.readFileSync("localhost-cert.pem")
    })
    .on("stream", stream => {
        console.log("handling http/2");
        stream.end(Math.random());
    })
    .listen(8443);
```

You must've noticed the certificate for localhost and don't worry, here is how to generate them

```sh
openssl req -x509 -newkey rsa:2048 -nodes -sha256 -subj /CN=localhost -keyout localhost-privkey.pem -out localhost-cert.pem
```

## Setup for Benchmarking

### Server side

There are still a few more steps need to be taken.

1. CORS
2. Cache invalidating
3. Long running tasks

Ideally, we need to do those through middleware. Since this is a demo, I'm trying to use minimal code to illustrate the idea by altering the headers directly, why not 🤣.

```javascript
// create 2-second tasks with setTimeout
setTimeout(() => {
    // configure CORS through Access-Control-Allow-Origin
    // configure cache through Cache-Control and Expires
    stream.respond({
        ":status": 200,
        "Access-Control-Allow-Origin": "*",
        "Cache-Control": "private, no-cache, no-store, must-revalidate",
        Expires: "-1"
    });
    stream.end(Math.random());
}, 2000);
```

### Client side

There is nothing special for the client side, its main purpose is to

1. Ping the server like crazy
2. Take measurements

```javascript
// take quick and dirty measurements with the Console API
console.time();
// yes, let's ping it 100 times and see what happens
Promise.all(
    [...Array(100)].map(() => fetch(`https://localhost:8443/${Math.random()}`))
).then(() => {
    console.timeEnd();
});
```

### Show me the Repo

Yes, we do have a repo.  
And it also includes the HTTP/1.1 endpoint.

Please feel free to check out the [repo](https://github.com/TianyuanC/http2-demo) for more details

## Benchmarking Results

The result is amazing 🤩

Due to the default connection limit on Chrome, the client can only send 6 requests at a time. And the whole operation ends up with **34182.37ms**.

On the other hand, HTTP/2 supports multiplexing which allows requests share the same connection. And take a wild guess, what will be the result?

**2269.68ms**, that is 15 times faster.

HTTP/1.1  
![http1](https://raw.githubusercontent.com/TianyuanC/http2-demo/master/img/http1_1.png)

HTTP/2  
![http2](https://raw.githubusercontent.com/TianyuanC/http2-demo/master/img/http2_0.png)

---

## Concerns?

Everything is perfect! Or is it?

Maybe now it's time to worry about the scalability of your server.  
And also optimize your queries so they are not making unnecessary requests, which will lead to unexpected billings. That's very common in the cloud subscriptions nowadays.

## Got Feedbacks?

Hope this post is helpful. Let me know what you think so that I can improve. Cheers!
