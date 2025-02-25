---
title: uniapp开发问题方案汇总
date: 2025-02-25 10:36:58
tags:
    - 前端
    - uniapp
categories:
    - [uniapp]
index_img: https://img.lzphl.top/blog/articleImg/uniapp.jpeg
banner_img: https://img.lzphl.top/blog/bg/bg-25-1.jpg
link: /posts/git-resolve.html
excerpt: git 问题汇总
---

### 状态栏高度

#### 使用 css 变量 `--status-bar-height`

var(--status-bar-height) 此变量在微信小程序环境为固定 25px，在 App 里为手机实际状态栏高度。
在 uniapp 里，使用 css 变量 `--status-bar-height` 获取状态栏高度。

```html
<template>
    <!-- HBuilderX 2.6.3+ 新增 page-meta, 详情：https://uniapp.dcloud.io/component/page-meta -->
    <page-meta>
        <navigation-bar />
    </page-meta>
    <view>
        <view class="status_bar">
            <!-- 这里是状态栏 -->
        </view>
        <view>状态栏下的文字</view>
    </view>
</template>
<style>
    .status_bar {
        height: var(--status-bar-height);
        width: 100%;
    }
</style>
```

#### nvue 下获取状态栏高度

nvue 不支持 css 变量， 该页面下获取状态栏高度，使用 `uni.getSystemInfoSync().statusBarHeight`

#### Android 与 IOS 状态栏高度适配

-   iOS 的状态栏高度是固定的，通常为 44px（iPhone 14 及以上机型）或 20px（旧机型）。
-   Android 的状态栏高度不固定，通常为 24px 或 48px（取决于设备和系统版本）。

根据不同的平台动态获取状态栏高度：

-   使用 `uni.getSystemInfoSync().platform` 判断平台
-   使用 `uni.getSystemInfoSync()`获取状态栏高度

```html
<template>
    <view :style="{ paddingTop: totalHeight + 'px' }">
        <!-- 页面内容 -->
    </view>
</template>

<script>
    export default {
        data() {
            return {
                totalHeight: 0,
            };
        },
        onLoad() {
            const systemInfo = uni.getSystemInfoSync();
            const statusBarHeight = systemInfo.statusBarHeight || 0;
            const platform = systemInfo.platform;

            let extraHeight = 0;
            if (platform === "ios") {
                extraHeight = 44; // iOS 导航栏高度
            } else if (platform === "android") {
                extraHeight = 48; // Android 导航栏高度
            }

            this.totalHeight = statusBarHeight + extraHeight;
        },
    };
</script>
```
