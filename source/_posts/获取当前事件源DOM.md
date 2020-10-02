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



```js

clickfun(e) {
// e.target 是你当前点击的元素
// e.currentTarget 是你绑定事件的元素
    #获得点击元素的前一个元素
    e.currentTarget.previousElementSibling.innerHTML
    #获得点击元素的第一个子元素
    e.currentTarget.firstElementChild
    # 获得点击元素的下一个元素
    e.currentTarget.nextElementSibling
    # 获得点击元素中id为string的元素
    e.currentTarget.getElementById("string")
    # 获得点击元素的string属性
    e.currentTarget.getAttributeNode('string')
    # 获得点击元素的父级元素
    e.currentTarget.parentElement
    # 获得点击元素的前一个元素的第一个子元素的HTML值
    e.currentTarget.previousElementSibling.firstElementChild.innerHTML
 
    }
```