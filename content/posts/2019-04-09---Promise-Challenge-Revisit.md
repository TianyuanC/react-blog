---
title: The Promise Challenge
date: "2019-04-14T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/es6-promise-challenge/"
category: "ES6"
tags:
    - "ES6"
description: "Ryan has once created a puzzle and even himself cannot solve... Well, during the live demo. And let's see what actually happened 👀"
---

### TL;DR

-   Promise batching, chaining and mix of both
-   A reflection of why the demo is failing

### Problem Setup

First of all let's recap what is the ES6 challenge.

```javascript
const delay = (t, name) => () =>
    new Promise(resolve => {
        setTimeout(() => {
            console.log(`done: task ${name}`);
            resolve();
        }, t);
    });
const task1 = delay(1000, "one");
const task2 = delay(500, "two");
const task3 = delay(600, "three");
```

We have defined three tasks above, to be more specific, `task1` will take 1 second to run and after 1 second it will print `done: task one` and similar settings apply for `task2` and `task3` as well.

Those three tasks will not execute until you invoke them explicitly

```javascript
// individual
task1();

// chaining
task2().then(() => task3());
```

Good, and next, our challenge is to execute the three tasks one by one sequentially, and your input is
`const tasks = [task1, task2, task3];`

Feel free to try it out [here](https://tianyuanc.github.io/knowledge-652-4/#11) yourself first.

### Root Cause Analysis (RCA)

Let's first look at my solution during the live demo

```javascript
const execute = () => {
    tasks.reduce((acc, currTask) => {
        return acc.then(() => {
            currTask();
        });
    }, Promise.resolve());
};
```

Unfortunately, it will print `two, three, one` instead of `one, two, three`

Can you spot the error now?  
Don't scroll down if you want to figure it out yourself.

DA DA DA, the solution

```javascript
const execute = () => {
    tasks.reduce(
        (acc, currTask) => acc.then(() => currTask()),
        Promise.resolve()
    );
};
```

So the difference lies in between

#### Case 1

```javascript
acc.then(() => {
    currTask();
});
```

#### Case 2

```javascript
acc.then(() => currTask());
```

For `Case 1` the next task will not wait for the completion of the current task and instead it will launch right away. You can think of `Case 1` as:

#### Case 1-1

```javascript
acc.then(() => {
    // execute the task
    currTask();
    // resolve the promise and
    // get the next task right away without the wait
    return;
});
```

---

As we can see, the position of `return` matters a lot.  
And hope you guys are not making the same mistake I did in the future.

Somehow I feel greatful to be able to re-visit this problem and trying to find the root cause.

Also I shouldn't have blamed the live coding, but instead, I should always trying to find the underlying implementation flaws.

Let me know what you think, so that I know who to improve, cheers!
