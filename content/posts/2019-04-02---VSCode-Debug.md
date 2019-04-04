---
title: Debug with VSCode
date: "2019-04-02T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/debug-with-vscode/"
category: "Tools"
tags:
    - "Tools"
description: "Tired of console.log and setting breakpoints on minified code? Try debugging your code with your favorite editor."
---

### TL;DW (watch)

-   Install VSCode extension Debugger for Chrome
-   Create your debug configurations
-   Step into/over/out
-   Gain insights through call stacks

### Show Me How

If it's your first time creating a debug session, you will need to select `Add Configuration...` and then pick Chrome as your environment.

For MyReads project in specific, because it's running on port 3000, you will need to modify your `settings.json` as follows, and then it will be saved under `.vscode` folder, and you can consider opt it out of your source control through `.gitignore`

```javascript
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

That's it. Enjoy debugging 🙈

---

Here are the breakdowns of the VSCode debugging demo

-   Install Debugger for Chrome _@19:28_
-   Set breakpoints _@20:29_
-   Step into and step over _@20:50_
-   Debug console _@21:22_
-   Call stacks _@22:04_

`youtube:https://www.youtube.com/embed/1JAeDlbk6b4?start=1143`

All comments are welcomed, so that I know how to improve next time 🙏
