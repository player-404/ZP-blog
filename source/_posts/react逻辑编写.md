---
title: react逻辑编写
date: 2024-11-06 22:49:54
tags:
    - 前端
    - React
categories:
    - [前端, React]
link: /posts/reactCode.html
index_img: https://img.lzphl.top/blog/articleImg/react.jpeg
banner_img: https://img.lzphl.top/blog/img/bg8.jpg
---

### 条件渲染

react 的写法和 js 写法相近，只不过是将值换成元素，这点与 vue 大不相同，vue 将这些 js 语法封装成一个个指令，同样条件渲染与 Vue 的写法也是有区别的，下面是几种条件渲染的写法:

#### 使用 if / else 进行条件渲染

```jsx
// 使用if else 条件渲染
export function ItemList(props) {
    let item;
    if (props.status) {
        item = <del>{props.name}</del>;
    } else {
        item = <span>{props.name}</span>;
    }
    return (
        <div className="item">
            <ul>
                <li>{item}</li>
            </ul>
        </div>
    );
}
```

当 `status` 为 `true` 我们为 `item` 设置 del 元素，当为 false 时，我们为 item 设置 span 元素，此时 item 是一个变量，它的值是元素，对于变量在元素中的使用，我们使用`{ }` 包裹

#### 使用三目运算符进行条件渲染

编写 react 更贴近于我们平时的 js 代码编写，**元素我们可以赋值给变量，并在元素中使用该变量**，因此，我们可以使用 三目 运算符进行条件渲染

```jsx
// 使用三目运算符
function ItemList2(props) {
    let item = props.status ? <del>{props.name}</del> : <span>{props.name}</span>;

    return (
        <div className="item">
            <ul>
                <li>{item}</li>
            </ul>
        </div>
    );
}
```

#### 使用 && 运算符

当条件为 true 时再渲染这个元素可以使用 `&&` 运算符

```jsx
// 使用 && 运算符
function ItemList3(props) {
    return (
        <div>
            <ul>
                <li>{props.status && <del>{props.name}</del>}</li>
            </ul>
        </div>
    );
}
```

```ad-note
`{}` 不光可以编写逻辑、变量，也可以直接编写元素，参照如上代码,嵌套标签变量仍需包裹一层 `{}`
```

### 列表渲染

列表渲染，我们可以使用 循环 将要渲染的元素放入一个数组，再直接使用该数组就可以了，你可以使用 map 方法，它可以直接返回一个数组，简单快捷，同样你也可以使用 for 循环、forEatch，**方法有很多，react 编写相对自由，根据你的需求和想法来决定，不要过于局限**

#### 使用 map 渲染列表

```jsx
export function Card() {
    const list = [
        { name: "HTML", status: true },
        { name: "CSS", status: true },
        { name: "JavaScript", status: false },
    ];

    const listItems = list.map((data, index) =>  <ItemList name={data.name} status={data.status} key={index} />;
    );



    return (
        <div className="card">
            <div style={{ margin: "30px" }}>{listItems}</div>
        </div>
    );
}
```

#### 使用 forEach 渲染列表

```jsx
export function Article() {
    const content = ["鹅，鹅，鹅", "曲项向天歌", "白毛浮绿水", "红掌拨清波"];
    let i = [];
    content.forEach((item, index) => {
        i.push(
            <li key={index} className="item">
                {item + " "}
                {index % 2 === 0 ? "," : "。"}
            </li>
        );
    });

    return (
        <div className="content">
            <Header title="咏鹅" subtitle="骆宾王" />
            <ul>{i}</ul>
        </div>
    );
}

function Header(props) {
    return (
        <div className="header">
            <div className="title">{props.title}</div>
            <div className="subtitle">{props.subtitle}</div>
        </div>
    );
}
```

#### 使用 for 循环渲染列表

```jsx
export function Card() {
    const test = [
        { name: "Vue", status: true },
        { name: "React", status: false },
    ];

    const listItem3 = [];

    for (let i = 0; i < test.length; i++) {
        listItem3.push(<ItemList3 name={test[i].name} status={test[i].status} key={i} />);
    }

    return (
        <div className="card">
            <div style={{ margin: "30px" }}>{listItem3}</div>
        </div>
    );
}
jsx;
```
