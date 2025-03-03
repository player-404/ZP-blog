---
title: Vite指北
date: 2025-03-03 21:17:28
tags: [前端, Vite]
categories: [前端]
index_img: https://img.lzphl.top/blog/img/OIP-C.jpeg
banner_img: https://img.lzphl.top/blog/bg/bg-25-1.jpg
link: /posts/vite-guide.html
excerpt: vite 文档记录
---

## Vite

### 缓存

#### 文件缓存

Vite 会将构建的依赖缓存，当以下文件改变时 Vite 会重新构建依赖

-   `package.json`
-   lockfile: `package-lock.json`、`yarn.lock`

> Vite 会将 预构建的依赖缓存到 `node_modules/.vite`

#### 浏览器缓存

在浏览器中，Vite 构建的依赖会被强缓存，可以使用以下方法禁用缓存

-   浏览器设置禁用缓存
-   启动添加 `--force` 命令
-   重新载入页面

### 文件导入

Vite 使用 `import.meta.glob`导入文件，可以使用 `*.js` 等匹配模式

### 插件

`plugins` 接收一个数组插件

参考：

[官方插件列表](https://vitejs.cn/vite3-cn/plugins/#official-plugins)

[社区插件列表](https://github.com/vitejs/awesome-vite?tab=readme-ov-file#plugins)

### 资源的处理

#### assetsIndeclude [Array]

在 vite 中，打包时会讲 js 与 css 文件进行处理，例如压缩等，但是静态资源是不用处理的，vite 会将如图片等的静态资源复制到指定的输出目录，如果你希望某些文件不要进行打包处理，可以使用 `assetsInclude` 进行配置， 告诉 vite 将哪些文件视为静态文件，不做处理

#### assetsInlineLimit [number]

小于该大小的的资源将会转换为 base64 内联使用，这样可以减少 HTTP 请求， 设置 **0** 可以禁用该选项

#### 显示的 URL 引入

#### 引入文件路径

导入静态资源时结尾加 **?url** 会返回文件的路径链接：

```js
import workletURL from "extra-scalloped-border/worklet.js?url";
```

使用场景：

1. 当你需要获取某个文件的 URL 路径，而不是文件内容时。
2. 当文件类型未被 Vite 默认视为静态资源时。
3. 当你需要动态加载资源（如图片、音频、配置文件等）时。

#### 引入为字符串

如果你希望将一个文件的内容转换成字符串可以在结尾添加 **?raw**

```js
import shaderString from "./shader.glsl?raw";
```

使用场景：

1. 加载纯文本 txt 文件中的内容
2. 加载 html 中的文件内容，将 html 片段渲染页面上
3. 加载 css 中的内容

### public 目录

该目录下的文件会直接复制到打包项目的 **根目录** 下，打包后可以通过 **/** 直接访问

当你希望某些文件不被 vite 处理时，可以将该文件放在 public 目录下，例如

-   rotbots.txt 文件，该文件告诉搜索引擎哪些页面可以抓取，需要放在项目根目录下
-   favicon.icon 网站图标

#### 注意事项

-   public 中的文件不应被 js 通过 import 活着 require 引用
-   文件名不会被哈希处理
-   访问时使用绝对路径

#### publicDir

默认为 public 目录下的文件会直接复制到根目录，可以使用该配置项修改默认的路径

### 浏览器兼容

vite 打包后的项目默认支持现代浏览器，需要兼容旧的浏览器可以使用 **@vitejs/plugin-legacy** 插件：

```bash
npm install @vitejs/plugin-legacy --save-dev
```

配置：

```js
import legacy from "@vitejs/plugin-legacy";
import { defineConfig } from "vite";

export default defineConfig({
    plugins: [
        legacy({
            targets: ["defaults", "not IE 11"], // 指定需要支持的浏览器
        }),
    ],
});
```

也可以修改 `build.target` 构建指定版本

### 公共基础路径

使用 **base** 配置项可以配置打包后部署的路径，默认为 **/**，也可配置为 **./** ，或者明确的明确的路径 **/foo/** ，完整的 url **http://xxx.com**

如果不同的资源在不同的路径下可以使用 `experimental.renderBuiltUrl`。

```ts
experimental: {
  renderBuiltUrl(filename: string, { hostType }: { hostType: 'js' | 'css' | 'html' }) {
    if (hostType === 'js') {
      return { runtime: `window.__toCdnUrl(${JSON.stringify(filename)})` }
    } else {
      return { relative: true }
    }
  }
}

```

### 自定义构建

可以通过 `build.rollupOptions` 直接调整底层的 [Rollup 选项](https://rollupjs.org/guide/en/#big-list-of-options)：

```js
// vite.config.js
export default defineConfig({
    build: {
        rollupOptions: {
            // https://rollupjs.org/guide/en/#big-list-of-options
        },
    },
});
```

### 产物分块策略

...待研究

### 文件变化时构建

可以使用 `vite build --watch` 来启用 rollup 的监听器。或者，你可以直接通过 `build.watch` 调整底层的 [`WatcherOptions`](https://rollupjs.org/guide/en/#watch-options) 选项：

```js
// vite.config.js
export default defineConfig({
    build: {
        watch: {
            // https://rollupjs.org/guide/en/#watch-options
        },
    },
});
```
