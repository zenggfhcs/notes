# 安装 vue-cli

```shell
npm install -g @vue/cli
```

# 创建 vue-cli-project

在指定目录下命令行中：

**命令行创建**

```shell
vue create vue-project-name
```

**图形化创建**

```shell
vue ui
```

# 项目目录结构S

![img](..\images\vue\vue-project-strcutment.png)

## vue.config.js

保存vue配置的文件，可以配置代理、端口：

```javascript
// devServer.port 配置项目端口
devServer: {
  port: 7070
}
```

## package.json



## src



# vue 结构

## `<template>` 模板

定义模板

## `<script>` 数据

控制数据来源与行为

## `<style>` 样式

模板样式定义

# vue 路由

## 安装 vue-router

```shell
npm install vue-router@3.5.1
```

## 定义路由

在 src/router/index.js 文件中定义路由表 配置其中的 routes 变量：

```javascript
const routes = [
    {
		path: '/', // 访问路径
   		name: 'home', // 对应名称
   		component: HomeView // 访问页面
	}
	// [, {...}]*
  	,{
   		path: '/index',
	    redirect: '/' // 重定向页面
	}
]
```

## 使用 `<router-link>` 和 `<router-view>` 来完成页面路由配置

 `<router-link>` ：请求链接组件，浏览器会解析成 `<a>`

```html
<router-link to="/dept">部门管理</router-link>
```

`<router-view>` ：动态视图组件，用来渲染展示与路由路径对应的组件

```html
<!-- 添加在要跟随路由变化的位置 -->
<router-view></router-view>
```