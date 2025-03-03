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

### deploy.sh

`deploy.sh` 是一个常见的 Shell 脚本文件，通常用于自动化部署项目。它包含一系列命令，用于将代码从开发环境部署到生产环境。通过运行 `deploy.sh`，你可以自动化完成构建、测试、上传、重启服务等操作，减少手动操作的错误和时间成本。

---

### `deploy.sh` 的作用

1. **自动化部署流程**：
    - 将代码从本地或代码仓库部署到服务器。
    - 执行构建、测试、上传、重启等操作。
2. **减少人为错误**：
    - 通过脚本自动化操作，避免手动操作中的遗漏或错误。
3. **提高效率**：
    - 一键完成复杂的部署流程，节省时间。

---

### `deploy.sh` 的典型内容

以下是一个简单的 `deploy.sh` 示例，用于部署一个基于 Vite 的前端项目：

bash

复制

```
#!/bin/bash

# 1. 停止脚本执行，如果任何命令失败
set -e

# 2. 切换到项目目录
cd /path/to/your/project

# 3. 从 Git 拉取最新代码
echo "Pulling latest code from Git..."
git pull origin main

# 4. 安装依赖
echo "Installing dependencies..."
npm install

# 5. 构建项目
echo "Building project..."
npm run build

# 6. 将构建产物复制到服务器目录
echo "Copying files to server directory..."
rsync -avz --delete dist/ /var/www/your-project/

# 7. 重启服务器（如果需要）
echo "Restarting server..."
sudo systemctl restart nginx

echo "Deployment completed successfully!"
```

---

### 如何使用 `deploy.sh`

#### 1. **创建 `deploy.sh` 文件**

在项目根目录下创建一个名为 `deploy.sh` 的文件，并将上述内容粘贴进去。

#### 2. **修改脚本内容**

根据你的项目需求修改脚本内容，例如：

-   修改项目路径（`/path/to/your/project`）。
-   修改 Git 分支（`main`）。
-   修改构建命令（`npm run build`）。
-   修改服务器目录（`/var/www/your-project/`）。

#### 3. **赋予执行权限**

在终端中运行以下命令，赋予 `deploy.sh` 执行权限：

bash

复制

```
chmod +x deploy.sh
```

#### 4. **运行脚本**

在终端中运行以下命令，执行部署脚本：

bash

复制

```
./deploy.sh
```

---

### `deploy.sh` 的详细步骤解析

#### 1. **`set -e`**

-   作用：如果任何命令执行失败，脚本会立即停止。
-   目的：避免在错误状态下继续执行后续命令。

#### 2. **`cd /path/to/your/project`**

-   作用：切换到项目目录。
-   目的：确保后续命令在正确的目录下执行。

#### 3. **`git pull origin main`**

-   作用：从 Git 仓库拉取最新代码。
-   目的：确保部署的是最新代码。

#### 4. **`npm install`**

-   作用：安装项目依赖。
-   目的：确保所有依赖包都已安装。

#### 5. **`npm run build`**

-   作用：构建项目。
-   目的：生成生产环境的静态文件。

#### 6. **`rsync -avz --delete dist/ /var/www/your-project/`**

-   作用：将构建产物（`dist/` 目录）同步到服务器目录。
-   参数说明：
    -   `-a`：归档模式，保留文件属性和权限。
    -   `-v`：显示详细输出。
    -   `-z`：压缩传输。
    -   `--delete`：删除目标目录中多余的文件。

#### 7. **`sudo systemctl restart nginx`**

-   作用：重启 Nginx 服务器。
-   目的：使新部署的代码生效。

---

### 实际例子

#### 1. **部署一个 Vite 项目**

假设你有一个 Vite 项目，部署到服务器的 `/var/www/vite-project/` 目录。

1. **`deploy.sh` 内容**：

    bash

    复制

    ```
    #!/bin/bash
    set -e

    cd /home/user/vite-project
    git pull origin main
    npm install
    npm run build
    rsync -avz --delete dist/ /var/www/vite-project/
    sudo systemctl restart nginx

    echo "Vite project deployed successfully!"
    ```

2. **运行脚本**：

    bash

    复制

    ```
    ./deploy.sh
    ```

3. **结果**：

    - 最新代码从 Git 拉取。
    - 项目依赖安装完成。
    - 项目构建完成。
    - 构建产物同步到服务器目录。
    - Nginx 服务器重启，新代码生效。

---

#### 2. **部署一个 Node.js 后端项目**

假设你有一个 Node.js 后端项目，部署到服务器的 `/var/www/node-app/` 目录。

1. **`deploy.sh` 内容**：

    bash

    复制

    ```bash
    #!/bin/bash
    set -e

    cd /home/user/node-app
    git pull origin main
    npm install
    npm run build
    rsync -avz --delete dist/ /var/www/node-app/
    pm2 restart app

    echo "Node.js app deployed successfully!"
    ```

2. **运行脚本**：

    bash

    复制

    ```bash
    ./deploy.sh
    ```

3. **结果**：

    - 最新代码从 Git 拉取。
    - 项目依赖安装完成。
    - 项目构建完成。
    - 构建产物同步到服务器目录。
    - PM2 重启应用，新代码生效。

---

### 注意事项

1. **权限问题**：
    - 如果脚本中涉及需要管理员权限的操作（如 `sudo`），确保运行脚本的用户有足够的权限。
2. **环境变量**：
    - 如果脚本中需要使用敏感信息（如 API 密钥），建议通过环境变量传递，而不是直接写在脚本中。
3. **测试脚本**：
    - 在正式使用前，先在测试环境中运行脚本，确保没有错误。
4. **日志记录**：
    - 可以在脚本中添加日志记录功能，方便排查问题。

---

### 总结

`deploy.sh` 是一个用于自动化部署的 Shell 脚本，通过它可以：

1. 拉取最新代码。
2. 安装依赖。
3. 构建项目。
4. 同步文件到服务器。
5. 重启服务。

使用 `deploy.sh` 可以大大提高部署效率，减少人为错误。根据你的项目需求，灵活调整脚本内容即可！
