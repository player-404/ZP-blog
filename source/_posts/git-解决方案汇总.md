---
title: git 解决方案汇总
date: 2025-02-25 10:02:25
tags:
    - 前端
    - 后端
    - git
categories:
    - [前端，git]
index_img: https://img.lzphl.top/blog/articleImg/git.jpg
banner_img: https://img.lzphl.top/blog/bg/bg-25-1.jpg
link: /posts/git-resolve.html
excerpt: git 问题汇总
---

### 使用 ssh 连接超时

在配置好 ssh 密钥之后，使用 ssh 连接 github 时，提示超时，解决方法如下：

修改 ~/.ssh/config 文件，添加如下内容：

```bash
Host github.com
  Hostname ssh.github.com
  User git
  Port 443
```

修改完之后，重新连接 github，即可解决超时问题。
