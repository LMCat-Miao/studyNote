# 第一阶段：解构赋值（必须熟练）

## 1. 为什么需要解构？

以前：

```
const user = {
    name:"Tom",
    age:18,
    job:"frontend"
}


const name = user.name;
const age = user.age;
```

问题：

代码重复。

ES6：

```
const {name, age} = user;

console.log(name);
console.log(age);
```

本质：

> 从对象或者数组里面快速取值。

------

# 一、对象解构

## 基础

```
const person = {
    username:"Tom",
    age:20
}


const {username, age} = person;


console.log(username);
console.log(age);
```

等价于：

```
const username = person.username;
const age = person.age;
```

------

## 面试重点：变量改名

比如：

```
const user={
    name:"Tom"
}


const {
    name:userName
}=user;


console.log(userName);
```

输出：

```
Tom
```

注意：

不是创建两个变量：

错误理解：

```
name
userName
```

实际上：

```
userName = user.name
```

------

## 设置默认值

```
const user={
    name:"Tom"
}


const {
    age=18
}=user;


console.log(age);
```

输出：

```
18
```

但是：

```
const user={
    age:null
}

const {
    age=18
}=user;


console.log(age)
```

输出：

```
null
```

原因：

默认值只针对：

```
undefined
```

------

# 二、数组解构

以前：

```
const arr=[10,20,30];


const a=arr[0];
const b=arr[1];
```

ES6：

```
const [a,b,c]=arr;


console.log(a);
console.log(b);
```

输出：

```
10
20
```

------

## 跳过元素

```
const arr=[1,2,3];


const [a,,c]=arr;


console.log(a,c);
```

输出：

```
1 3
```

------

# 工作中哪里用？

非常多：

## 1. Vue3 setup

你以后一定会看到：

```
const {
 data,
 loading,
 error
}=useRequest()
```

比如：

```
const {
 data:userInfo
}=await getUserInfo()
```

------

## 2. 函数参数

以前：

```
function createUser(options){

console.log(options.name)

}
```

现在：

```
function createUser({
 name,
 age
}){

console.log(name)

}


createUser({
 name:"Tom",
 age:20
})
```

这是企业代码高频写法。

------

# 第二阶段：展开运算符 ...

这是 ES6 最重要特性之一。

符号：

```
...
```

叫：

spread operator

展开运算符。

------

# 1. 数组展开

```
const arr1=[1,2,3];


const arr2=[4,5,6];


const arr3=[
...arr1,
...arr2
];


console.log(arr3);
```

结果：

```
[1,2,3,4,5,6]
```

以前：

```
arr1.concat(arr2)
```

------

# 2. 数组复制

```
const arr=[1,2,3];


const newArr=[
...arr
];
```

注意：

这是浅复制。

例如：

```
const obj={
name:"Tom"
}


const newObj={
...obj
}
```

复制的是：

```
第一层属性
```

里面对象仍然共享。

这个和你之前学的：

> 引用数据类型

直接关联。

------

# 3. 对象展开

非常重要。

例如：

修改用户信息：

```
const user={
name:"Tom",
age:18
}


const newUser={
...user,
age:20
}


console.log(newUser)
```

结果：

```
{
name:"Tom",
age:20
}
```

为什么？

后面的覆盖前面的：

```
{
...user,
age:20
}
```

执行：

第一步：

```
name:"Tom",
age:18
```

第二步：

```
age:20
```

所以：

```
后面的属性覆盖前面的
```

------

# 工作场景

React：

```
setUser({
 ...user,
 name:"Jack"
})
```

Vue：

修改响应式对象：

```
state={
 ...state,
 loading:false
}
```

------

# 第三阶段：Map / Set

这两个面试经常问。

------

# Set

理解：

> 没有重复元素的集合。

创建：

```
const set=new Set();


set.add(1);
set.add(2);
set.add(2);


console.log(set)
```

结果：

```
{1,2}
```

自动去重。

------

## 数组去重

经典面试：

```
const arr=[
1,2,2,3,3
];


const result=[
...new Set(arr)
]


console.log(result)
```

输出：

```
[1,2,3]
```

------

# Map

理解：

> key-value结构，但是key可以是任何类型。

普通对象：

```
const obj={}

obj["name"]="Tom"
```

key只能：

```
字符串
symbol
```

Map：

```
const map=new Map();


map.set("name","Tom");


map.set(
 {id:1},
 "user"
)
```

对象也可以作为key。

------

# 工作场景：

缓存：

```
const cache=new Map();


cache.set(
"user/1",
data
)
```

------

# 第四阶段：Class

你之前学习过：

- 原型
- prototype
- new
- this

所以 class 很容易。

注意：

class不是新东西。

本质：

> prototype 的语法糖。

------

以前：

```
function Person(name){

this.name=name;

}


Person.prototype.say=function(){

console.log(this.name)

}
```

ES6：

```
class Person{


constructor(name){

this.name=name;

}


say(){

console.log(this.name)

}


}
```

完全一样。

------

# 继承

以前：

原型链继承。

ES6：

```
class Student extends Person{


constructor(name){

super(name)

}


study(){

console.log("study")

}


}
```

重点：

## super是什么？

调用父类 constructor。

------

面试：

问：

> class 是不是新的继承方式？

回答：

不是。

class只是：

```
prototype + constructor 的语法糖
```

------

# 第五阶段：ES Module 模块化

现代项目核心。

以前：

所有代码：

```
index.js
main.js
utils.js
```

混一起。

ES6：

拆文件。

------

## 导出

utils.js

```
export function add(a,b){

return a+b;

}
```

------

使用：

```
import {
add
}
from "./utils.js"
```

------

默认导出：

```
export default function(){

}
```

导入：

```
import xxx from "./xxx.js"
```

------

# 面试重点：

## CommonJS 和 ES Module区别

CommonJS:

Node:

```
require()
module.exports
```

ES Module:

```
import
export
```

区别：

|          | CommonJS   | ES Module |
| -------- | ---------- | --------- |
| 加载     | 运行时     | 编译时    |
| Node     | 早期       | 现代      |
| 浏览器   | 不原生支持 | 支持      |
| 静态分析 | 不方便     | 方便      |

# 第1题：接口数据

真实开发：

后端：

```
{
code:200,

data:{
 username:"Tom"
},

message:"success"

}
```

------

答案：

```
const {

code,

data:{
    username
},

message

}=response;
```

------

输出：

```
200
Tom
success
```

------

这个非常重要。

Vue项目大量出现：

例如：

```
const {
data:userList
}=await getUserList()
```

# 第2题：函数参数解构

普通：

```
function createUser(options){

console.log(options.name)

}
```

调用：

```
createUser({
name:"Tom",
age:20
})
```

------

改：

```
function createUser({
name,
age
}){

console.log(name);
console.log(age);

}
```

调用：

```
createUser({
name:"Tom",
age:20
})
```

------

面试解释：

> 函数参数解构可以直接拿到对象属性，减少options.xxx这种重复访问。



# 你的 ES6 学习计划（7天）

## Day1

解构

目标：

能写：

```
const {
data,
error
}=response
```

练习：

写10个对象解构。

------

## Day2

展开运算符

重点：

```
浅复制
对象合并
immutable思想
```

练习：

实现：

```
updateUser(user,newInfo)
```

------

## Day3

数组方法

必须掌握：

```
map

filter

reduce

find

some

every
```

这是企业代码高频。

------

## Day4

Set Map

实现：

- 数组去重
- 简单缓存

------

## Day5

Class

手写：

- Person
- Student继承

复习：

prototype

------

## Day6

模块化

自己拆：

```
utils.js

request.js

api.js

main.js
```

模拟真实项目。

------

## Day7

综合项目：

手写：

```
用户管理模块
```

包含：

```
class User

Map缓存

Set权限

module拆分

对象解构

展开更新
```

------

# 学完 ES6 后，你应该达到这个水平：

看到：

```
const {
data:userInfo={}
}=await request.get("/user")


const newUser={
...userInfo,
age:20
}


class UserService{

}
```

你应该马上知道：

- `{}` 是解构
- `...` 是浅复制
- await返回Promise结果
- class本质prototype
- request来自模块导入

这才是企业开发需要的 ES6 能力。