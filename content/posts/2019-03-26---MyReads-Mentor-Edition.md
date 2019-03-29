---
title: Ryan's Reads
date: "2019-03-26T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/myreads-mentor-edition/"
category: "Projects"
tags:
    - "MyReads"
    - "Project"
description: "Spoiler alert: I will show my final version of MyReads"
---

### TL;DR

-   This post will talk about how to organize your React component to make it more readable and maintainable
-   You will see the usage of React Hooks
-   However, you cannot use React Hooks in this project 😂
-   Learning React is a continuous journey and you need to be ready to accept new concepts and patterns

### Anti-Pattern

In my previous post, I think I have mentioned code smell or anti-pattern.

Anti-pattern doesn't mean it's bad code, it can work perfectly, bug-free and highly performant. And some people even uphold anti-pattern as job security.

The only downside is maintenance, in terms of React components, business logics and UI component will very likely to mix together.

So every time when you want to make some small changes, that will often lead to a giant unnecessary patchset. It's error-prone and also time consuming.

Even if you are the original author, you won't want to read your code again after one month.

### Ryan's Reads

Luckily, with `React Hook` as well as some tricks of module `import`/`export`, your code can look very logical and organized.

```javascript
import React from "react";
import { Route } from "react-router-dom";
import { HomeView, SearchView } from "./view";
import { useBooksApi } from "./effects";
import "./App.css";

export default () => {
    const {
        books,
        searchResult,
        handleShelfUpdate,
        handleSearchTerm,
        resetSearchResult
    } = useBooksApi();

    const homeViewProps = {
        books,
        resetSearchResult,
        handleShelfUpdate
    };
    const searchViewProps = {
        searchResult,
        handleSearchTerm,
        handleShelfUpdate
    };

    return (
        <div className="app">
            <Route
                path="/search"
                render={() => <SearchView {...searchViewProps} />}
            />

            <Route
                exact
                path="/"
                render={() => <HomeView {...homeViewProps} />}
            />
        </div>
    );
};
```

That's it. In the above snippet, `useBooksApi()` abstracts the `books` and `searchResult` states from the `App.js` component as well as a few function handler calls with a custom hook.

By moving all the logics to another file, it not only removes distractions from the `App.js`, but also makes it a lot easier to write unit tests for interacting with the Books API.

Imagine all the handler calls were still in the component class, how can you make sure certain API is being made by triggering some action.

Well, you can do that by using `enzyme`, but wouldn't it be much easier if all the handler calls are already exposed, and all you need to do is to mock the dependencies and test it?

---

The above example also includes some usages of spread operator, destructuring as well as import/export. You can learn more about those in my knowledge webinar videos and slides as well as Udacity extra-curriculum materials.

Thoughts and question? Please feel free to leave comments. And I will try my best to address or amend the content.
