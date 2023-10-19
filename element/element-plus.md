# 引入

## 安装

```shell
npm install element-plus --save
```

## 完整引入 在 src/main.js 中添加

```javascript
// main.js
import { createApp } from 'vue'
import ElementPlus from 'element-plus' // 1
import 'element-plus/dist/index.css' // 2
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus) // 3
app.mount('#app')
```

# 使用组件

**element-plus 组件官方文档**

[https://element-plus.gitee.io/zh-CN/component/button.htmlelement-plus.gitee.io](https://element-plus.gitee.io/zh-CN/component/button.html)

1. 选择需要的组件，

2. 查看源代码，

3. 复制源代码各部分到对应 `.vue` 文件的对应位置