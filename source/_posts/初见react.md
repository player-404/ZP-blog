---
title: 初见react
date: 2024-11-04 22:20:16
tags:
    - 前端
    - React
categories:
    - [前端, React]
link: /posts/reactStart.html
index_img: https://img.lzphl.top/blog/articleImg/react.jpeg
banner_img: https://img.lzphl.top/blog/img/bg22.jpg
excerpt: 初识react
---

## 1. 创建 react 项目

### create-react-app 创建

这个方式创建 react 已经不被官方推荐，这里仅作为练习使用

```shell
npx create-react-app
```

### vite 创建

推荐创建项目的方式

## 2.使用

引入 react 并渲染到页面

```js
import React from "react";

import ReactDOM from "react-dom/client";

import "./index.css";

import App from "./App";

import reportWebVitals from "./reportWebVitals";

const root = ReactDOM.createRoot(document.getElementById("root")); //  创建html的根节点
root.render(
    // render 渲染页面
    <React.StrictMode>
                <App />   {" "}
    </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function

// to log results (for example: reportWebVitals(console.log))

// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals

reportWebVitals();
```

## 3.react 构成

一个 react 项目有多个**页面**构成，一个页面由多个**组件**构成，组件中包含各自的数据与逻辑，react 中组件是基石

### 创建组件

react 组件名称首字母大写，其次组件只能返回一个根元素，这点与 vue2 是一样的

```js
// 组件名称首字母大写 返回的组件只能包含一个根元素

function Pizza() {
    return (
        <div>
            <h1>this is a pizza menu</h1> <p>this is a pizza content</p>     
        </div>
    );
}

function App() {
    return (
        <div className="App">
            <Pizza /> // 使用组件      
        </div>
    );
}
```

### JSX

**JSX**是 javascript 扩展，每个组件返回的内容就是 jsx，特点就是元素、数据与逻辑是写可以直接写在一块的

## 启用 react 严格模式

处于严格模式时，react 会编译两次，查找 bug，同时会提示是否使用了过时的语法

```js
import React from "react";
import ReactDOM from "react-dom/client";

function App() {
    return <h1>hello React</h1>;
}

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
    <React.StrictMode>
        {" "}
        // 启用 react 严格模式
        <App />
    </React.StrictMode>
);
```
