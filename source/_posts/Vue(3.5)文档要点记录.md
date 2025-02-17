---
title: Vue(3.5)文档要点记录
date: 2025-02-13 22:13:15
tags:
    - 前端
    - Vue
categories:
    - [前端, Vue]
index_img: https://img.lzphl.top/blog/articleImg/VUE.JS.jpeg
excerpt: 文档阅读记录
link: /posts/VueDoc3.5.html
---

## Vue 3.5 要点记录

### 计算属性

#### 计算属性会将结果进行缓存：

计算属性会将结果进行缓存，只有在依赖的数据更新时才会重新计算

#### 计算属性可以获取到上一个结果(3.4 更新)

计算属性接收一个参数，该参数为上一个计算的结果；

使用访问器则在 get 函数中接收该参数

#### 计算属性不应有副作用

计算属性不应更改其他依赖的值或更改 dom，只返回计算的结果

### 侦听器

#### watch 与 watchEffect

-   watchEffect 会自动监听依赖的值并且会立即执行，watch 需要这只 `{immediate: tue}`
-   watch 监听的是明确的单个值（单个对象）， watchEffect 可以监听多个值

-   对于监听对象中的某个或多个属性，使用 watchEffect 是较好的，避免在 watch 中监听整个对象

#### 清理副作用（3.5 新增）

当在侦听器中执行异步任务，当异步任务未完成，而依赖发生改变，会触发 **onWatcherCleanup** 钩子，如果此时想停止执行异步任务，可以在 **onWatcherCleanup** 取消执行

```js
import { watch, onWatcherCleanup } from "vue";

watch(id, (newId) => {
    const controller = new AbortController();

    fetch(`/api/${newId}`, { signal: controller.signal }).then(() => {
        // 回调逻辑
    });

    onWatcherCleanup(() => {
        // 终止过期请求
        controller.abort();
    });
});
```

#### 侦听器执行时机

侦听器实在 父组件更新之后，当前组件更新之前执行，改变执行时机可以使用 **flush** 设置项

-   **post**：在当前组件更新之后执行

```js
watch(source, callback, {
    flush: "post",
});
```

-   **sync**：更新时执行（同步执行）

```js
watch(source, callback, {
    flush: "sync",
});
```

#### 停止侦听器

侦听器本身返回一个函数，该函数 **unwatch** 可以停止侦听器的执行

### 模板引用

#### 访问模板引用 （useTemplateRef 3.5 新增）

新增辅助函数 **useTemplateRef** 辅助函数 访问 dom 实例

```vue
<script setup>
import {useTemplateRef, onMounted} from "vue";
const btnRef = useTemplateRef("btn");
onMounted(() => {
    console.log("btn 实例", btnRef.value);
});
<template>
    <button  ref="btn">change me</button>
</template>
```

`v-for` 中使用返回的时元素实例的数组

3.5 版本之前访问 v-for 中的模板引用：

```vue
<script setup>
import { ref, onMounted } from "vue";

const list = ref([
    /* ... */
]);

const itemRefs = ref([]);

onMounted(() => console.log(itemRefs.value));
</script>

<template>
    <ul>
        <li v-for="item in list" ref="itemRefs">
            {{ item }}
        </li>
    </ul>
</template>
```

### props

#### props 解构(3.5)

可以使用对象结构的方式使用 props

```js
const { name } = defineProps(["name"]);
```

### 事件

emit 可以接收一个函数返回布尔值，用来验证参数是否合法，但是即使不合法也将触发自定义事件并传递参数，只是谁给出一个 warn

```js
<script setup>
const emit = defineEmits({
  // 没有校验
  click: null,

  // 校验 submit 事件
  submit: ({ email, password }) => {
    if (email && password) {
      return true
    } else {
      console.warn('Invalid submit event payload!')
      return false
    }
  }
})

function submitForm(email, password) {
  emit('submit', { email, password })
}
</script>
```

### 组件

#### 组件名格式

组件名使用 PascalCase 格式



### 组件的 v-model

子组件使用 defineModel即可在父组件使用 v-model绑定数据

```vue
//子组件
<script setup>
const model = defineModel('model')
</script>

<template>
	<input v-model="model" />
</template>
  
//父组件
<components v-model="test" />

```

上面的写法等于：

```vue
<script>
	const props = defineProps('modelValue');
  const emit = defineEmits('update:modelValue')
</script>
```



### 透传Attribute

#### 访问透传attribute

使用 useAttrs 即可访问透传Attribute

```vue
<script setup>
  import {useAttrs} from 'vue'
const arrts = useAttrs();
</script>
```



### 插槽

#### 作用于插槽

在插槽中绑定的属性，在组件中可以访问到

```vue
<template>
	<div>
    <slot name="test" :text="123"></slot>
  </div>
</template>

//使用
<component>
<template #test="scope">
	<div>
    {{scope.test}} //123
  </div>
</template>
</component>
```

