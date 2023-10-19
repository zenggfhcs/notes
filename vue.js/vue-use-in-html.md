# 引入 Vue.js

```html
<!-- 这是最新版本，适用于学习 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js">
</script>
<!-- 生产环境推荐用这个 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14"></script>
```

# 编写 Vue 核心对象

```html
<script>
	new Vue({
  		el: "#app",
  		data: {
  			message: "Hello Vue!"
  		},
  		methods: {
  			// 方法
  		},
  		// 生命周期函数
  		// 挂载完成函数
  		mounted: () => {
  		}
	})
</script>
```

# 编写视图


```html
<div id="app"> <input type="text" v-model="message">
	{{ message }}
</div>
```

`div.id` 要与 `Vue 对象` 的 `el 属性` 内容一致（# 代表 id选择器）

# 常用指令

![img](..\images\vue\com-vue-commands.webp)