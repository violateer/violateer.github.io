---
title: 获取当前事件源DOM
index_img: /img/Vue.png
excerpt: Vue中获取当前事件源的方法
---

## 获取当前事件源DOM

今天用Vue3写程序时遇到了一个问题，原代码如下：

```vue
<template>
	<div id="toggleCodeButton" @click="toggleCode">
        显示代码
    </div>
</template>

<script lang="ts">
import { onMounted } from 'vue'
export default {
    setup() {
        const toggleCodeButton = <HTMLDivElement>document.querySelector('#toggleCodeButton')
        const toggleCode = onMounted(() => {
            console.log(toggleCodeButton)
        })
    }
}
</script>
```

预期的结果时打印出`toggleCodeButton`这个DOM元素，但是结果返回的是`null`，

后来发现了问题所在：我虽然log是在挂载完成后，但是获取DOM却是在挂载之前，当然不会有结果，只要将获取DOM元素也放到`onMounted()`钩子中即可。

代码如下：

```vue
setup() { 
        const toggleCode = onMounted(() => {
		const toggleCodeButton = <HTMLDivElement>document.querySelector('#toggleCodeButton')
            console.log(toggleCodeButton)
        })
    }
```



本文想要记录的是在查找解决办法的过程中发现的另一种方法，可以获取触发当前事件的DOM，即在调用的方法中传入`$event`参数，再用`e.target`获取DOM

代码修改如下：

```vue
<template>
	<div id="toggleCodeButton" @click="toggleCode($event)">
        显示代码
    </div>
</template>

<script lang="ts">
export default {
    setup() {
        const toggleCode = (e) => {
            console.log(e.target)
        }
    }
}
</script>
```