---
title: How to use Redux Dev Tool
date: "2019-04-07T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/how-to-use-redux-dev-tool/"
category: "Tools"
tags:
    - "Tools"
description: "Having fun with that React Dev Tool? Good news for you, we have an even better one for redux. Its name is, unsurprisingly, Redux Dev Tool 🙃"
---

### TL;DR

-   Install the Redux Dev Tool extension
-   Configure the dev tool in your code
-   Play around with the dev tool with the Tetris app

### Installation and Setup

You can follow the following [guide](http://extension.remotedev.io/#1-for-chrome) to download the extension for Chrome. There are also options for Firefox, Electron and others.

In terms of the setup on your project, I recommend using the [redux-devtools-extension](http://extension.remotedev.io/#13-use-redux-devtools-extension-package-from-npm) because that will make your code base cleaner.

Also if you prefer opting out of the redux dev tool in production environment, you can leverage  
`import { composeWithDevTools } from 'redux-devtools-extension/logOnlyInProduction';`.

### Time for Tetris 👏

When was the last time you played [Tetris](https://chvin.github.io/react-tetris/?lan=en)?

Now you can play it again, and maybe learn some redux from it.

The following video will cover several insights we can get by using the redux devtool

-   Jump among actions and skip them _@50:27_
-   Visualize your state _@50:42_
-   Action replay _@51:05_

`youtube:https://www.youtube.com/embed/SSLrz5RsmnE?start=2852`
