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

## 2.引入 react

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
                        <h1>this is a pizza menu</h1>            <p>this is a pizza content</p>     
             {" "}
        </div>
    );
}

function App() {
    return (
        <div className="App">
                        <Pizza /> // 使用组件        {" "}
        </div>
    );
}
```

### JSX

**JSX**是 javascript 扩展，每个组件返回的内容就是 jsx，特点就是元素、数据与逻辑是写可以直接写在一块的

## 4. 样式

### 内联样式

react 的内联样式写在 `{ }` 中，传入对象，不可直接写字符串，`{}` 也可再 jsx 中写逻辑，与 Vue 中的 `{{ }}` 十分相似，区别在于少了一个括号

```jsx
function Pizza() {
    return (
        <div>
                        <p className="text">this is a pizza content</p>       {" "}
        </div>
    );
}
```

### 外部样式

react 同样可以使用外部样式，首先导入外部的样式，再在标签上添加样式，**react 中添加样式使用 className，不可使用 class**，class 在 jsx 中是关键字

```jsx
// 组件名称首字母大写 返回的组件只能包含一个根元素
import "./App.css";
function Pizza() {
    return (
        <div>
                        <p className="text">this is a pizza content</p>       {" "}
        </div>
    );
}
```

## 5.react 数据

### state

在 react 中组件内使用到的数据使用`state`创建，外部数据接收一个`props`参数
**外部的数据在组件内不应被更改，应为只读的，需要更改应使用 state**

### 父组件向子组件传递数据

这类数据的传递与 Vue 是类似的，**单向数据流**，子组件不能更改父组件传递的数据，这点在上面也提到了，传递的方式也是与 Vue 类似的，在子组件上绑定数据，之后接收一个 props 参数在组件中直接使用

```jsx
// 父组件
function App() {
    return (
        <div className="App">
                        <Pizza title="锄禾日当午111" />       {" "}
        </div>
    );
}
// 子组件
function Pizza(props) {
    return (
        <div>
            <h1 style={{ color: "red", fontSize: "390px" }}>{props.title}</h1>
            <p className="text">this is a pizza content</p>
        </div>
    );
}
```

### 6.列表渲染

当需要渲染一个列表时，可以使用 js 中的数组的方法（map）等，返回一个标签数组，然后再组件中将返回的标签数组直接使用:

```JSX
export function Info(props) {
  const lists = [
    {
      bg: "pink",
      text: "精通电脑开机",
    },
    {
      bg: "blue",
      text: "精通软件安装",
    },
    {
      bg: "green",
      text: "精通软件卸载",
    },
  ];
  const tagList = lists.map((item, index) => (
    <Tag key={index} bg={item.bg} text={item.text} />
  ));
  return (
    <div className="info-con">
      <div className="head-title">{props.title}</div>
      <p>{props.desc}</p>
      <div className="bot">{tagList}</div>
    </div>
  );
}
```

### 7.综合案例

这个案例集合了 列表渲染，数据传递，可以查看源码结合上面的知识点理解

<iframe src="https://codesandbox.io/embed/qkkhdl?view=editor+%2B+preview&module=%2Fsrc%2Fmycard.js"
     style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;"
     title="卡片"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

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

```

```
