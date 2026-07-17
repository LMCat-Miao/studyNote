# 第0步：检查开发环境

Vue项目需要：

1. Node.js
2. npm（Node自带）
3. VS Code（推荐）

------

# 第1步：检查 Node.js 是否安装

打开 Windows 终端：

方法：

按：

```
Win + R
```

输入：

```
cmd
```

回车。

输入：

```
node -v
```

如果看到：

例如：

```
v22.15.0
```

说明安装成功。

再检查：

```
npm -v
```

例如：

```
10.9.2
```

说明npm正常。

------

# 第2步：选择项目存放位置

例如：

我建议：

```
D:\code
```

创建：

```
D:\code
```

进入：

```
cd D:\code
```

解释：

```
cd
```

= change directory

进入某个文件夹。

例如：

```
cd Desktop
```

进入桌面。

------

# 第3步：创建Vue项目

执行：

```
npm create vite@latest
```

你会看到：

```
Need to install the following packages:
create-vite
Ok? (y)
```

输入：

```
y
```

------

然后出现：

```
Project name:
```

输入：

例如：

```
vue-admin-system
```

这就是你的项目名字。

------

接下来：

选择框架：

```
Select a framework:
```

选择：

```
Vue
```

键盘：

上下键选择

Enter确认。

------

然后：

选择语言：

```
Select a variant:
```

选择：

```
JavaScript
```

（现在不要选TypeScript，我们后面补）

------

最终类似：

```
✔ Project name: vue-admin-system
✔ Select framework: Vue
✔ Select variant: JavaScript
```

------

# 第4步：进入项目

现在：

```
cd vue-admin-system
```

你的路径：

变成：

```
D:\code\vue-admin-system
```

------

# 第5步：安装依赖

执行：

```
npm install
```

作用是什么？

你创建项目以后：

package.json里面：

记录了依赖。

例如：

```
{
"dependencies":{

"vue":"xxx"

}
}
```

但是电脑没有真正下载。

npm install：

就是：

根据package.json下载所有依赖。

下载完成：

出现：

```
node_modules
```

这个文件夹。

------

# 第6步：启动Vue项目

执行：

```
npm run dev
```

你会看到：

```
VITE v6.x.x

Local:

http://localhost:5173/
```

复制：

```
http://localhost:5173/
```

浏览器打开。

看到：

Vue欢迎页面。

说明成功。

------

# 第7步：用VS Code打开项目

如果安装了VS Code：

终端输入：

```
code .
```

作用：

当前文件夹使用VS Code打开。

------

如果没有：

打开VS Code：

点击：

```
File
 ↓
Open Folder
 ↓
选择

D:\code\vue-admin-system
```

------

# 第8步：认识创建后的目录

打开：

```
vue-admin-system
```

你会看到：

```
node_modules
public
src
.gitignore
index.html
package.json
vite.config.js
```

重点看src：

```
src
|
├── assets
|
├── components
|
├── App.vue
|
├── main.js
|
└── style.css
```

------

# 第9步：清理默认代码

我们做后台项目，需要干净环境。

## 删除：

```
src/assets
```

删除：

```
src/components/HelloWorld.vue
```

------

打开：

```
src/App.vue
```

全部删除。

改成：

```
<template>

  <h1>
    Vue Admin System
  </h1>

</template>


<script setup>


</script>


<style scoped>


</style>
```

保存。

浏览器：

应该显示：

```
Vue Admin System
```

------

# 第10步：安装后台项目需要的依赖

我们的后台系统需要：

## Element Plus

后台UI组件库：

按钮

表格

表单

安装：

```
npm install element-plus
```

------

## Vue Router

路由：

页面跳转。

安装：

```
npm install vue-router
```

------

## Pinia

状态管理。

安装：

```
npm install pinia
```

------

## Axios

请求接口。

安装：

```
npm install axios
```

------

最终你的项目技术：

```
Vue3

+
Vite

+
Vue Router

+
Pinia

+
Axios

+
Element Plus
```

这就是企业Vue后台项目常见组合。

------

# 第11步：验证安装成功

打开：

```
package.json
```

应该看到：

```
"dependencies": {

"axios": "...",

"element-plus": "...",

"pinia": "...",

"vue": "...",

"vue-router": "..."

}
```

------

# 第12步：理解整个创建流程（面试）

你刚才实际上做了：

```
安装Node环境

↓

npm create vite

↓

生成Vue工程

↓

npm install

↓

安装依赖

↓

npm run dev

↓

Vite启动服务器

↓

浏览器访问项目
```

------

# 你今天创建完成后的项目：

```
vue-admin-system

src

├── api
│
├── components
│
├── router
│
├── stores
│
├── utils
│
├── views
│
├── App.vue
│
└── main.js
```