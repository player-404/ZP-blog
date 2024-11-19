---
title: Fragment使用
date: 2024-11-19 22:00:20
tags: - 前端
    - react
categories: - [前端, react]
index_img: https://img.lzphl.top/blog/articleImg/vercel.webp
link: /posts/react-fragment.html
excerpt: react 中的 fragment 简单介绍
---

react 中的 `Fragment` 与 Vue 中的 `template` 类似，它不会渲染成元素，仅仅用来包裹多个标签，在 react 中只能返回一个元素，如果我们想返回多个元素，又不想影响布局时，可以使用 Fragment

### 作用

用来返回多个元素，可以简写为 `<></>`

### 使用

使用 `fragment`

```react
import { Fragment } from "react";
export function Fragments() {
    return (
        <Fragment>
            <h1>Fragment</h1>
            <p>这是一个段落a</p>
        </Fragment>
    );
}

```

使用 `<></>`

```react
export function Word({ person, words }) {
    return (
        <>
            {person}
            <p>说</p>
            {words}
        </>
    );
}

```

### Fragment 渲染列表

当使用 Fragment 渲染列表时，需要显示的使用，并传入 key

```react
export function Lists() {
    const words = ["自由不是你想做什么就做什么", "而是你想做什么就能不做什么"];
    return words.map((item, index) => (
        <Fragment key={index}>
            <h2>
                {item}
                {index == 0 ? "，" : "。"}
            </h2>
        </Fragment>
    ));
}

```

### 注意事项

-   如果你要传递 `key` 给一个 `<Fragment>`，你不能使用 `<>...</>`，你必须从 `'react'` 中导入 `Fragment` 且表示为`<Fragment key={yourKey}>...</Fragment>`。
-   `Fragment` 作用很大，它与将元素包裹在一个 DOM 容器中不同，使用 `Fragment` 对元素进行组合后不会影响布局和样式。
