最近在写项目时碰到了一个问题，已解决，在此记录下仅作学习用。

## 需求

已经将一个`.vue`文件的代码作为字符串获取，需要封装一个函数，将代码中的`import`声明所`from`的库改为指定字符串，例如：将`import Switch from '../lib/Switch.vue'\r\n`改为`import Switch from 'vue'\r\n`

`.vue`文件的代码字符串如下：

```js
const str = '\r\n' +
    '<template>\r\n' +
    '<Switch v-model:value="init" disabled />\r\n' +
    '</template>\r\n' +
    '\r\n' +
    '<script lang="ts">\r\n' +
    "import Switch from '../lib/Switch.vue'\r\n" +
    'import {\r\n' +
    '    ref\r\n' +
    "} from 'vue'\r\n" +
    'export default {\r\n' +
    '    components: {\r\n' +
    '        Switch\r\n' +
    '    },\r\n' +
    '    setup() {\r\n' +
    '        const init = ref(false)\r\n' +
    '        return {\r\n' +
    '            init\r\n' +
    '        }\r\n' +
    '    }\r\n' +
    '}\r\n' +
    '</script>\r\n'
```

## 函数modifyImport（oldStr, updateStr）

```js
const modifyImport = (oldStr, updateStr) => {
    const arr = str.split('\r\n')
    const targetIndex = arr.findIndex(e => e.endsWith(".vue'"))
    const targetStr = arr.find(e => e.endsWith(".vue'"))
    const indexStart = targetStr.indexOf("from") + 5
    arr[targetIndex] = targetStr.replace(targetStr.slice(indexStart), updateStr)
    return arr.join('\r\n')
}
```

## 优化函数

```js
// 上面的写法只支持替换找到的第一条符合要求的`import`语句
// 改为下面的写法，支持找到所有符合要求的语句并将其替换

const modifyImport = (oldStr, updateStr) => {
    const oldStrsArr = []
    const arr = oldStr.split('\r\n')
    arr.map((e, index) => {
        if(e.endsWith(".vue'")) {
            const tempObj = {
                value: e,
                index: index,
                replaceStartIndex: e.indexOf("from") + 5
            }
            oldStrsArr.push(tempObj)
        }
    })
    oldStrsArr.map(e => {
        arr[e.index] = e.value.replace(e.value.slice(e.replaceStartIndex), updateStr)
    })
    return arr.join('\r\n')
}
```

