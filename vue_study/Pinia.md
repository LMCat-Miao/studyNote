# 一、为什么需要 Pinia？

先思考一个问题。

假设你的后台系统：

```
App.vue

 ├── Layout.vue

      ├── Header.vue
      │
      ├── Sidebar.vue
      │
      └── Main.vue
```

现在登录用户信息在哪里？

比如：

```
{
 username:"admin",
 avatar:"xxx.jpg",
 role:"admin",
 permissions:[
    "user:add",
    "user:delete"
 ]
}
```

Header需要：

```
显示用户名
显示头像
```

Sidebar需要：

```
根据权限显示菜单
```

Main需要：

```
判断页面权限
```

那么问题来了：

这些组件之间怎么传？

------

## 方法1：props传递

App.vue

↓

Layout.vue

↓

Header.vue

比如：

```
<Header :user="user"/>
```

继续传：

```
<UserInfo :user="user"/>
```

问题：

层级少还可以。

但是企业项目：

```
App

 Layout

  Header

   UserAvatar

    Dropdown

     MenuItem
```

传5层。

这叫：

## Props 地狱

------

## 方法2：事件传递

子组件：

```
emit("updateUser")
```

父组件接收：

```
@updateUser="xxx"
```

但是：

跨组件通信非常麻烦。

------

所以我们需要：

# 一个所有组件都能访问的公共仓库

这就是：

# Store（状态仓库）

结构：

```
          Store

        /   |    \

 Header Sidebar Main
```

所有组件：

直接访问Store。

------

# 二、Pinia是什么？

一句话：

> Pinia 是 Vue 官方推荐的状态管理工具，用来管理跨组件共享的数据。

以前：

Vue2:

```
Vuex
```

Vue3:

```
Pinia
```

------

# 三、理解 Pinia 的核心思想

一个 Store：

包含三个东西：

```
Store

 |
 |
 ├── state
 |
 ├── getters
 |
 └── actions
```

对应：

| Pinia   | 类比     |
| ------- | -------- |
| state   | 数据     |
| getters | 计算属性 |
| actions | 方法     |

------

# 四、第一个Pinia Store

项目结构：

```
src

 ├── stores

      └── user.js
```

安装：

```
npm install pinia
```

main.js:

```
import { createApp } from 'vue'
import { createPinia } from 'pinia'


import App from './App.vue'


const app=createApp(App)


app.use(createPinia())


app.mount('#app')
```

意思：

给整个Vue应用安装Pinia。

------

# 五、创建用户Store

user.js

```
import {defineStore} from "pinia"


export const useUserStore=defineStore(
"user",
{

state(){

return {

 username:"",

 token:"",

 roles:[],

 permissions:[]

}

},


actions:{


login(userInfo){


this.username=userInfo.username

this.token=userInfo.token


}


},


getters:{


isLogin(state){

return !!state.token

}


}


})
```

------

现在解释每一部分。

------

# 1. state

类似：

Vue组件里面：

```
data(){

return{

}

}
```

例如：

```
state(){

return{

username:"Tom"

}

}
```

就是存数据。

------

# 2. actions

存放：

修改数据的方法。

比如登录：

```
login(){

this.token="xxx"

}
```

为什么不能直接：

```
store.token="xxx"
```

当然可以。

但是企业喜欢：

```
组件

↓

action

↓

修改state
```

原因：

统一管理业务逻辑。

例如：

登录：

```
login()

  |
  |
调用接口

  |
  |
保存token

  |
  |
获取用户信息

  |
  |
保存权限
```

这些都放action。

------

# 3. getters

类似：

computed。

例如：

state:

```
token:"abc"
```

getter:

```
isLogin(){

return !!this.token

}
```

使用：

```
store.isLogin
```

返回：

```
true
```

------

# 六、组件如何使用Store？

例如Header.vue

```
<script setup>


import {useUserStore} from "@/stores/user"


const userStore=useUserStore()


</script>



<template>


<h1>

{{userStore.username}}

</h1>


</template>
```

流程：

```
Header

 ↓

useUserStore()

 ↓

读取store

 ↓

显示用户名
```

------

# 七、真实项目：登录流程

现在结合你的Day3登录。

完整流程：

```
用户输入账号密码

        ↓

Login.vue

        ↓

调用request.js

        ↓

后端返回token

        ↓

保存Pinia

        ↓

跳转Home
```

代码：

Login.vue

```
const userStore=useUserStore()


async function login(){


const res=await loginApi(form)


userStore.login(res.data)


router.push("/")


}
```

------

# 八、为什么token要放Pinia？

因为：

很多地方需要：

```
request.js

Header

权限判断

退出登录
```

如果放：

Login.vue

别人访问不到。

所以：

```
token

userInfo

permission
```

都是全局状态。

------

# 九、但是Pinia刷新会消失？

重点！！！

Pinia默认：

```
内存保存
```

刷新浏览器：

```
F5

↓

重新加载

↓

state丢失
```

所以：

需要持久化。

------

安装：

```
npm install pinia-plugin-persistedstate
```

main.js:

```
import persist from "pinia-plugin-persistedstate"


app.use(createPinia().use(persist))
```

store:

```
export const useUserStore=defineStore(
"user",
{

state(){

return{

token:""

}

},


persist:true


})
```

现在：

token会保存到：

```
localStorage
```

------

# 十、权限菜单设计（企业重点）

后台系统：

不同用户看到不同菜单。

例如：

管理员：

```
首页

用户管理

权限管理

商品管理
```

普通用户：

```
首页

个人中心
```

怎么实现？

------

## 数据结构设计

后端返回：

```
{

username:"admin",

menus:[


{
title:"用户管理",

path:"/user",

permission:"user:list"


}


]

}
```

------

Pinia保存：

```
menus:[]
```

------

Sidebar:

```
<div
v-for="menu in userStore.menus"
>


{{menu.title}}


</div>
```

结果：

自动生成菜单。

------

# 十一、权限控制核心思想

权限分三层：

## 第一层：路由权限

例如：

用户不能访问：

```
/admin/user
```

router:

```
beforeEach((to)=>{


if(!hasPermission(to)){

return "/403"

}


})
```

------

## 第二层：菜单权限

控制：

看不看得到。

例如：

```
<UserManage v-if="permission.includes('user:add')"/>
```

------

## 第三层：按钮权限

比如：

删除按钮。

```
<button
v-if="hasPermission('user:delete')"
>

删除

</button>
```

------

# 十二、企业项目完整结构

你的项目未来应该类似：

```
src


├── api

    user.js


├── stores

    user.js


├── router

    index.js


├── layouts

    Layout.vue


├── components

    Sidebar.vue


├── utils

    permission.js
```