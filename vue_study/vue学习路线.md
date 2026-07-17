# Day1：Vue3入门 + 项目初始化

目标：

会创建Vue项目，理解Vue运行流程。

## 上午学习

### 1. Vue项目结构

掌握：

```
src

├── api
├── assets
├── components
├── views
├── router
├── stores
├── utils
├── App.vue
└── main.js
```

理解：

为什么这样拆？

面试：

> Vue项目目录如何设计？

------

### 2. Composition API

重点：

setup

```
<script setup>


</script>
```

理解：

Vue3为什么推荐组合式API。

------

### 3. ref

必须掌握：

```
const count=ref(0)

count.value++
```

理解：

ref为什么需要.value。

------

### 4. reactive

```
const user=reactive({

name:"Tom"

})
```

理解：

Proxy代理。

------

## 下午项目

创建后台项目：

技术栈：

```
Vue3
Vite
Vue Router
Pinia
Axios
Element Plus
```

完成：

基础目录。

创建：

```
views

Login.vue

Home.vue


components

Layout.vue
```

------

晚上复习JS：

10题：

- let/const
- 类型转换
- == ===
- 闭包

------

# Day2：响应式 + 组件通信 + Layout

目标：

完成后台基本框架。

## 上午

重点：

Vue响应式原理

必须理解：

```
数据

↓

Proxy

↓

track依赖收集

↓

trigger更新

↓

重新渲染
```

面试：

Vue3响应式原理。

------

组件通信：

必须掌握：

### 父→子

props

### 子→父

emit

### 跨组件

Pinia

------

## 下午项目

完成：

Layout布局

结构：

```
Layout

 |
 |-- Header

 |
 |-- Sidebar

 |
 |-- Main
```

实现：

左侧菜单

顶部导航

内容区域

------

晚上：

JS：

作用域

this

闭包

------

# Day3：Vue Router + 登录流程

目标：

完成登录到首页跳转。

## 上午

Router核心：

学习：

router

route

history

children

meta

例如：

```
{
path:"/home",
component:Home,
meta:{
title:"首页"
}
}
```

------

路由守卫：

重点：

```
router.beforeEach()
```

流程：

```
访问页面

↓

检查token

↓

有token

↓

进入

↓

无token

↓

登录页
```

------

## 下午项目

完成：

登录页面

包含：

账号密码

登录按钮

模拟接口

流程：

```
Login

↓

request

↓

保存token

↓

router.push('/home')
```

------

晚上：

JS：

Promise

async await

------

# Day4：Axios封装 + 请求流程

这个非常重要。

## 上午

理解：

企业请求流程：

```
组件

↓

api/user.js

↓

request.js

↓

axios

↓

server
```

------

封装：

request.js

实现：

## 请求拦截器

添加token

## 响应拦截器

统一错误处理

处理：

401

------

## 下午项目

完成：

```
utils

request.js


api

user.js
login.js
```

接入登录。

------

晚上：

Promise：

手写：

Promise.all

事件循环

------

# Day5：Pinia状态管理

目标：

完成用户状态管理。

## 上午

学习：

为什么不用props？

Pinia结构：

```
store

 |
 user

 |
 permission
```

掌握：

state

action

getter

------

## 下午项目

创建：

```
stores

user.js
```

保存：

```
token

username

role
```

实现：

退出登录

清除状态。

------

晚上：

Vue八股：

ref/reactive区别

computed/watch

------

# Day6：computed + watch + 生命周期

目标：

补齐Vue核心。

## 上午

computed：

重点：

缓存机制

回答：

为什么computed性能更好？

------

watch：

应用：

监听数据变化

发送请求

------

生命周期：

重点：

mounted

unmounted

------

## 下午项目

完善首页：

Dashboard

增加：

统计卡片

用户信息展示

------

晚上：

JS：

原型链

new

instanceof

------

# Day7：动态菜单 + 权限控制

重点！！！

这是项目亮点。

## 上午

学习：

权限设计：

后端返回：

```
{
role:"admin",
menus:[
"user",
"role"
]
}
```

前端根据权限生成菜单。

------

动态路由：

```
router.addRoute()
```

------

## 下午项目

实现：

管理员：

```
首页

用户管理

权限管理
```

普通用户：

```
首页
```

------

晚上：

Vue八股：

Router原理

导航守卫

------

# Day8：404 + 项目完善

## 上午

学习：

路由匹配：

```
/:pathMatch(.*)
```

404页面设计。

------

## 下午项目

完成：

404页面

菜单高亮

面包屑

页面切换动画

------

晚上：

JS：

数组方法

map

filter

reduce

------

# Day9：项目优化 + 面试包装

目标：

把项目变成简历项目。

## 上午

学习：

Vue性能优化：

- v-if/v-show
- keep-alive
- lazy loading
- 组件拆分

------

## 下午

优化项目：

增加：

loading

错误提示

请求状态管理

代码整理。

------

晚上：

准备项目介绍：

回答：

> 介绍一下你的Vue项目

------

# Day10：模拟面试 + 查漏补缺

上午：

重新整理：

Vue知识地图

必须会：

```
Vue3

 |
 Composition API

 |
 响应式

 |
 Router

 |
 Pinia

 |
 Axios

 |
 权限
```

------

下午：

模拟：

20道Vue面试题

例如：

1. Vue3为什么用Proxy？
2. ref和reactive区别？
3. computed为什么缓存？
4. watch什么时候用？
5. Vue生命周期？
6. nextTick原理？
7. Router守卫？
8. Pinia作用？
9. Axios拦截器？
10. 权限怎么实现？

------

晚上：

总结项目。

------

# 每天JS复习固定安排（不要停）

每天：

30-60分钟

顺序：

| 天   | JS复习      |
| ---- | ----------- |
| 1    | 变量+类型   |
| 2    | 作用域+闭包 |
| 3    | this        |
| 4    | Promise     |
| 5    | 事件循环    |
| 6    | 原型链      |
| 7    | new         |
| 8    | ES6         |
| 9    | 数组方法    |
| 10   | 手写题      |

------

# 10天完成后的能力

你可以：

✅ 独立搭Vue后台项目
 ✅ 写登录流程
 ✅ 封装Axios
 ✅ 管理Token
 ✅ 使用Pinia
 ✅ 写动态路由
 ✅ 做权限控制
 ✅ 解释Vue核心原理

对于校招来说，这比花20天学Vue所有API更加有效。

下一步建议：

直接开始 **Day1，我会按照真实公司项目方式带你搭建Vue后台系统，每写一行代码解释：为什么存在、不写会怎样、面试怎么回答。**