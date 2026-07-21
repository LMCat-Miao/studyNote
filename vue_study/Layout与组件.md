# 一、什么是后台 Layout？

先看一个真实后台页面：

比如：

```
--------------------------------
|          Header              |
--------------------------------
|      |                       |
|      |                       |
| Side |       Main            |
| bar  |       内容区域         |
|      |                       |
|      |                       |
--------------------------------
```

三个部分：

## Header

顶部区域：

放：

- 用户头像
- 退出登录
- 系统名称

------

## Sidebar

左侧菜单：

例如：

```
首页

用户管理

权限管理

商品管理

订单管理
```

------

## Main

内容区域：

显示不同页面。

例如：

点击：

用户管理

Main里面显示：

```
用户列表
```

点击：

商品管理

Main里面显示：

```
商品列表
```

------

所以：

Layout本质：

> 一个固定的页面框架，负责承载变化的业务页面。

------

# 二、为什么不全部写在App.vue？

初学者可能这样写：

App.vue：

```
<template>

<header>
顶部
</header>


<aside>
菜单
</aside>


<main>
内容
</main>


</template>
```

小项目：

可以。

但是企业项目：

会爆炸。

原因：

App.vue职责太多。

------

正确：

拆分：

```
App.vue

   |
   |
 Layout.vue

   |
   |
 ----------------

 Header.vue

 Sidebar.vue

 Main.vue
```

------

这就是：

组件化。

------

# 三、Vue组件是什么？

你可以把组件理解成：

> 一个可以重复使用的小型Vue页面。

比如：

Header：

它就是一个组件。

文件：

```
components/Header.vue
```

里面：

```
<template>

<header>
后台管理系统
</header>

</template>
```

然后其他地方：

可以使用。

------

类似JavaScript函数：

你以前学JS：

```
function add(){

}
```

写一次。

调用：

```
add()
```

组件：

也是一样。

写一次：

```
Header.vue
```

使用：

```
<Header/>
```

------

# 四、开始搭建Layout目录

现在你的src：

创建：

```
src

├── components

│     ├── Header.vue
│     └── Sidebar.vue
│
├── layouts
│
│     └── Layout.vue
│
└── views

      └── Home.vue
```

为什么新增layouts？

因为：

Layout不是普通组件。

它是：

页面骨架。

所以企业喜欢单独：

```
layouts
```

------

# 五、创建Layout.vue

文件：

```
src/layouts/Layout.vue
```

写：

```
<template>

<div class="layout">


    <Header />


    <div class="container">


        <Sidebar />


        <main>
            页面内容
        </main>


    </div>


</div>


</template>


<script setup>


import Header from '@/components/Header.vue'

import Sidebar from '@/components/Sidebar.vue'


</script>
```

------

现在解释。

## 这一句：

```
import Header from '@/components/Header.vue'
```

什么意思？

引入组件。

类似：

JS：

```
import axios from "axios"
```

------

## 为什么可以写 @？

正常：

```
../../components/Header.vue
```

很麻烦。

所以Vite配置了：

```
@
```

代表：

```
src
```

所以：

```
@/components
```

等于：

```
src/components
```

------

# 六、创建Header组件

文件：

```
components/Header.vue
```

代码：

```
<template>

<header>

    Vue Admin System

</header>


</template>


<style scoped>


header{

height:60px;

background:#333;

color:white;

display:flex;

align-items:center;

padding-left:20px;

}


</style>
```

------

# 七、创建Sidebar组件

文件：

```
components/Sidebar.vue
```

代码：

```
<template>


<div class="sidebar">


<ul>


<li>
首页
</li>


<li>
用户管理
</li>


<li>
权限管理
</li>


</ul>


</div>


</template>



<style scoped>


.sidebar{


width:200px;

height:500px;

background:#eee;


}


li{

padding:20px;

}


</style>
```

------

# 八、现在修改App.vue

因为：

App负责入口。

让它显示Layout。

App.vue：

```
<template>


<Layout/>


</template>


<script setup>


import Layout from './layouts/Layout.vue'


</script>
```

------

现在浏览器：

应该看到：

```
------------------

Vue Admin System


首页

用户管理

权限管理


页面内容


------------------
```

恭喜。

你已经完成：

企业后台Layout第一版。

------

# 九、现在进入重点：组件通信

为什么需要组件通信？

因为：

组件之间需要传递数据。

例如：

Header显示用户名。

用户名在哪里？

登录之后：

可能存在：

Pinia。

但是现在先理解组件。

------

Vue组件关系：

## 父组件

包含别人。

例如：

Layout：

```
Layout

 |
 |
Header
```

Layout是父。

Header是子。

------

## 子组件

被别人使用。

Header：

就是子。

------

# 十、父传子：props

场景：

Layout告诉Header：

当前用户名。

Layout：

```
const username="Tom"
```

Header：

显示。

------

父组件：

```
<Header username="Tom"/>
```

这里：

传递数据。

------

子组件：

Header：

```
<script setup>


defineProps({

username:String


})


</script>
```

使用：

```
<template>


<h1>

{{username}}

</h1>


</template>
```

------

流程：

```
Layout

  |
  | username="Tom"
  ↓

Header

显示Tom
```

------

# 十一、为什么需要props？

因为：

组件之间不能直接访问变量。

比如：

Layout：

```
const username="Tom"
```

Header：

不能：

```
console.log(username)
```

为什么？

因为：

两个组件：

两个独立作用域。

就像JS函数：

```
function A(){

let a=10

}


function B(){

console.log(a)

}
```

访问不到。

所以需要：

props。

------

# 十二、子传父：emit

反过来。

例如：

Sidebar点击菜单。

告诉Layout：

“用户点击了首页”。

子：

Sidebar

发送：

事件。

------

子组件：

```
<script setup>


const emit=defineEmits([

"change"

])


function clickMenu(){

emit("change","home")

}


</script>
```

------

父组件：

```
<Sidebar
@change="handleChange"
/>
```

接收：

```
function handleChange(page){

console.log(page)

}
```

------

流程：

```
Sidebar

点击

↓

emit("change")

↓

Layout接收

↓

执行函数
```

------

# 十三、组件通信总结（面试重点）

Vue组件通信：

## 父传子

props

```
父
 |
 |
子
```

------

## 子传父

emit

```
子

 |
 |
父
```

------

## 兄弟组件

通过：

共同父组件

例如：

Header

Sidebar

不能直接通信。

应该：

Header

↓

Layout

↓

Sidebar

------

## 全局通信

Pinia。

例如：

用户信息。

```
任何组件

↓

Pinia

↓

任何组件
```

------

# 十四、今天你应该形成的知识链

不要背代码。

理解：

```
后台系统

↓

页面固定部分

↓

Layout

↓

拆组件

↓

组件需要数据

↓

组件通信

↓

props / emit

↓

复杂数据

↓

Pinia
```