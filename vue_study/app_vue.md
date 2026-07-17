# 第一部分：Vue项目到底怎么启动？

现在打开你的项目：

```
vue-lmm-test
```

找到：

```
index.html
```

你会看到：

```
<body>

<div id="app"></div>

<script type="module" src="/src/main.js"></script>

</body>
```

这里非常重要。

------

## 问题：

浏览器打开网页，它第一眼看到什么？

答案：

index.html

浏览器不知道：

Vue是什么。

它只认识：

HTML。

所以：

# 第一步：

浏览器加载：

```
index.html
```

里面：

```
<div id="app"></div>
```

现在页面空的。

像你买了一块空地。

------

# 第二步：main.js出现

继续看：

```
src/main.js
```

默认：

```
import { createApp } from 'vue'

import App from './App.vue'


createApp(App).mount('#app')
```

我们逐行分析。

------

# 第一行

```
import { createApp } from 'vue'
```

什么意思？

从Vue这个库里面：

拿出来一个函数：

```
createApp
```

它的作用：

创建Vue应用。

类似：

你学JS的时候：

```
new Object()
```

创建对象。

这里：

```
createApp()
```

创建Vue应用。

------

# 第二行

```
import App from './App.vue'
```

什么意思？

导入：

根组件。

什么叫根组件？

Vue项目：

实际上是一棵树。

比如：

```
             App.vue

                 |
        -----------------

        Header       Main

                       |

                    UserList
```

所有组件：

最终都挂在：

App.vue下面。

所以：

App.vue就是：

Vue应用的大门。

------

# 第三行（重点）

```
createApp(App)
```

执行之后：

Vue内部发生：

创建一个Vue应用实例。

类似：

```
const app = createApp(App)
```

现在：

app里面有：

- Vue能力
- 组件系统
- 响应式系统
- 插件系统

------

所以更推荐写：

```
const app=createApp(App)
```

为什么？

因为后面需要：

```
app.use()
```

比如：

Router：

```
app.use(router)
```

Pinia：

```
app.use(pinia)
```

------

所以企业项目：

main.js一般写：

```
import {createApp} from "vue"

import App from "./App.vue"


const app=createApp(App)


app.mount("#app")
```

------

# 第四步：mount是什么？

继续：

```
app.mount('#app')
```

mount：

翻译：

挂载。

什么意思？

找到：

index.html

这里：

```
<div id="app"></div>
```

然后：

把App.vue放进去。

原来：

```
<div id="app">

</div>
```

变成：

```
<div id="app">

<h1>
Vue Admin System
</h1>

</div>
```

------

所以完整流程：

```
浏览器打开


↓

index.html


↓

找到main.js


↓

createApp(App)


↓

加载App.vue


↓

mount("#app")


↓

页面显示
```

###### Vue项目入口在哪里？

错误回答：

> App.vue

为什么错？

因为：

App.vue只是根组件。

真正入口：

main.js。

正确回答：

> Vue3项目通常从main.js作为入口，通过createApp创建应用实例，然后挂载根组件App.vue。

# 第二部分：App.vue到底是什么？

打开：

```
src/App.vue
```

Vue文件叫：

单文件组件：

SFC

Single File Component

结构：

```
<script setup>

</script>


<template>

</template>


<style>

</style>
```

------

# 为什么一个文件包含三个东西？

传统开发：

HTML：

一个文件

CSS：

一个文件

JS：

一个文件

Vue认为：

一个功能应该放一起。

比如：

按钮：

```
Button.vue


HTML

JS

CSS
```

------

# template是什么？

写页面。

例如：

```
<template>

<h1>
首页
</h1>

</template>
```

最终：

变成HTML。

------

# script setup是什么？

写JavaScript逻辑。

例如：

```
<script setup>

const name="Tom"

</script>
```

------

# style是什么？

CSS。

```
<style scoped>

h1{

color:red

}

</style>
```

------

# scoped是什么意思？

重点。

假设：

A组件：

```
h1{

color:red

}
```

B组件：

也有：

```
<h1>
标题
</h1>
```

那么B也会红。

因为CSS全局。

------

加：

```
<style scoped>
```

意思：

只影响当前组件。

企业Vue项目大量使用。

------

# 第三部分：为什么Vue需要组件？

回忆你的后台项目。

如果全部写：

App.vue

最后：

可能：

5000行。

所以拆：

```
App.vue


Layout.vue

Header.vue

Sidebar.vue

Home.vue

User.vue
```

------

例如后台：

页面结构：

```
Layout


|
|---- Header

|
|---- Sidebar

|
|---- Content
```

每个都是组件。

------

为什么这样设计？

因为：

复用。

例如：

Header：

后台每个页面都有。

不用复制代码。

------

# 第四部分：进入Composition API

现在你理解了：

Vue负责：

管理页面。

但是：

Vue怎么知道数据变了？

例如：

```
let count=0

count++
```

Vue不知道。

所以需要：

响应式数据。

------

# ref出现

普通JS：

```
let count=0
```

Vue：

不知道。

改：

```
const count=ref(0)
```

Vue知道。

------

写一个例子：

创建：

```
src/views/Home.vue
```

代码：

```
<template>

<h1>
{{count}}
</h1>


<button @click="add">

增加

</button>


</template>



<script setup>

import {ref} from "vue"


const count=ref(0)


function add(){

count.value++

}


</script>
```

------

运行：

点击按钮。

发生：

```
点击按钮

↓

count.value++

↓

Vue发现数据变化

↓

重新渲染页面

↓

数字变化
```

------

# 为什么必须.value?

因为：

ref不是普通变量。

实际上：

```
const count=ref(0)
```

等于：

```
const count={

value:0

}
```

所以：

修改：

```
count.value++
```

------

# reactive出现

问题：

对象怎么办？

例如：

用户信息：

```
const user={

name:"Tom",

age:20

}
```

如果：

每个字段：

ref：

```
name=ref()

age=ref()
```

很麻烦。

所以：

reactive。

```
const user=reactive({

name:"Tom",

age:20

})
```

修改：

```
user.age++
```