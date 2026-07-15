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

## 解构

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

## 展开运算符

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

## 数组方法

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

# 一、先理解数组方法的核心思想

以前：

```
const arr=[1,2,3]


for(let i=0;i<arr.length;i++){

    console.log(arr[i])

}
```

命令式：

> 告诉计算机一步一步怎么做。

ES6：

```
arr.map(item=>{

})
```

声明式：

> 告诉计算机我要什么结果。

现代前端更喜欢声明式。

------

# 二、map（最重要）

## 作用：

> 遍历数组，并返回一个新的数组。

语法：

```
arr.map((item,index,array)=>{

    return 新值

})
```

三个参数：

| 参数  | 含义     |
| ----- | -------- |
| item  | 当前元素 |
| index | 索引     |
| array | 原数组   |

------

# 面试重点：

## map不会修改原数组

```
const arr=[1,2,3]


const newArr=arr.map(
item=>item*2
)


console.log(arr)
```

结果：

```
[1,2,3]
```

------

# 工作场景1：接口数据转换

后端：

```
const users=[
{
id:1,
username:"Tom"
},

{
id:2,
username:"Jack"
}

]
```

页面需要：

```
[
{
label:"Tom",
value:1
}
]
```

使用：

```
const options=users.map(user=>({

label:user.username,

value:user.id

}))
```

结果：

```
[
{
label:"Tom",
value:1
},
{
label:"Jack",
value:2
}
]
```

这里结合了：

```
map
+
对象展开思想
+
箭头函数
```

------

# 三、filter（过滤）

## 作用：

> 返回满足条件的新数组。

语法：

```
arr.filter(item=>{

return 条件

})
```

------

例子：

找出大于18的人。

```
const users=[
{
name:"Tom",
age:20
},

{
name:"Jack",
age:15
}

]


const result=users.filter(user=>{

return user.age>=18

})
```

结果：

```
[
{
name:"Tom",
age:20
}
]
```

------

## filter执行规则

返回：

```
true
```

保留。

返回：

```
false
```

删除。

------

例如：

```
[1,2,3,4].filter(
item=>item>2
)
```

执行：

```
1 false 删除

2 false 删除

3 true 保留

4 true 保留
```

结果：

```
[3,4]
```

------

# 工作场景：

搜索功能：

输入：

```
Tom
```

过滤用户：

```
const result=users.filter(user=>{

return user.name.includes("Tom")

})
```

------

# 四、find（查找）

## 作用：

> 找到第一个满足条件的元素。

注意：

返回：

一个元素。

不是数组。

------

例子：

```
const users=[
{
id:1,
name:"Tom"
},

{
id:2,
name:"Jack"
}

]


const user=users.find(
item=>item.id===2
)
```

结果：

```
{
id:2,
name:"Jack"
}
```

------

如果找不到：

返回：

```
undefined
```

------

## find 和 filter区别

filter:

```
[
{id:1},
{id:2}
]
```

返回数组。

find:

```
{id:1}
```

返回对象。

面试必问：

> 如果只需要找到一个用户，用什么？

答案：

find。

------

# 五、some（是否存在）

作用：

> 判断数组中有没有一个满足条件。

返回：

boolean。

------

例子：

判断有没有管理员。

```
const users=[
{
name:"Tom",
role:"user"
},

{
name:"Jack",
role:"admin"
}

]


const hasAdmin=users.some(user=>{

return user.role==="admin"

})
```

结果：

```
true
```

------

理解：

some:

```
有没有一个？
```

类似：

数学：

```
存在一个
```

------

# 六、every（全部满足）

作用：

> 判断所有元素是否满足条件。

返回：

boolean。

------

例子：

判断所有用户年龄是否超过18。

```
const result=users.every(user=>{

return user.age>=18

})
```

如果：

所有人：

true

返回：

```
true
```

只要一个：

false

返回：

```
false
```

------

# some 和 every 对比

| 方法  | 意思           | 返回       |
| ----- | -------------- | ---------- |
| some  | 有没有一个满足 | true/false |
| every | 是不是全部满足 | true/false |

记忆：

some：

```
部分
```

every：

```
全部
```

------

# 七、reduce（最难，也是面试重点）

reduce：

> 将数组累积成一个结果。

语法：

```
arr.reduce((prev,item)=>{

return 新prev

},初始值)
```

两个核心：

```
prev
累计结果

item
当前元素
```



------

# reduce真实场景：购物车总价

数据：

```
const cart=[

{
name:"iphone",
price:8000
},

{
name:"ipad",
price:4000
}

]
```

计算总价：

```
const total=cart.reduce(
(sum,item)=>{

return sum+item.price

},0
)
```

结果：

```
12000
```

------

# reduce高级：数组转对象

后端：

```
[
{
id:1,
name:"Tom"
},
{
id:2,
name:"Jack"
}
]
```

转：

```
{
1:{
name:"Tom"
},

2:{
name:"Jack"
}
}
```

代码：

```
const obj=users.reduce(
(result,user)=>{

result[user.id]=user

return result

},{}
)
```

这个面试很喜欢问。

------

# 七个方法总结

| 方法   | 作用     | 返回     |
| ------ | -------- | -------- |
| map    | 转换     | 新数组   |
| filter | 过滤     | 新数组   |
| find   | 找一个   | 元素     |
| some   | 是否存在 | boolean  |
| every  | 是否全部 | boolean  |
| reduce | 累计     | 任意类型 |

------

# 真实项目中的组合

例如：

用户列表：

```
const users=[
{
name:"Tom",
age:20,
active:true
}
]
```

需求：

找到成年用户，然后只显示名字。

写法：

```
const names=users
.filter(user=>user.age>=18)
.map(user=>user.name)
```

结果：

```
[
"Tom"
]
```

这就是企业代码。

------

## Set Map

实现：

- 数组去重
- 简单缓存

# 一、先理解为什么需要 Set 和 Map

JavaScript 最开始只有：

## Array

数组：

```
const arr=[
"Tom",
"Jack",
"Tom"
]
```

问题：

### 1. 去重麻烦

以前：

```
const result=[];

for(let item of arr){

    if(!result.includes(item)){
        result.push(item)
    }

}
```

复杂。

------

### 2. 查找效率低

例如：

```
const users=[
{
id:1,
name:"Tom"
},
{
id:2,
name:"Jack"
}
]
```

找 id=2：

```
users.find(user=>user.id===2)
```

需要遍历。

如果数据很多：

```
10000条
100000条
```

效率下降。

所以：

ES6 引入：

```
Set
Map
```

------

# 二、Set（集合）

## 1. Set是什么？

一句话：

> Set 是一个不会存储重复值的数据结构。

例如：

```
const set=new Set();


set.add(1);
set.add(2);
set.add(2);


console.log(set);
```

结果：

```
Set(2){1,2}
```

第二个：

```
2
```

不会添加。

------

# 2. Set的基本操作

## 创建

```
const set=new Set();
```

也可以：

```
const set=new Set([
1,
2,
3
])
```

------

## 添加 add

```
set.add(4)
```

------

## 删除 delete

```
set.delete(2)
```

------

## 判断 has

```
set.has(1)
```

返回：

```
true
```

------

## 长度 size

注意：

不是：

```
length
```

而是：

```
set.size
```

------

## 清空 clear

```
set.clear()
```

------

# 三、Set实现数组去重（必须掌握）

这是面试高频。

题目：

给：

```
const arr=[
1,
2,
2,
3,
3,
4
]
```

去重。

------

## 方法一：Set

```
const result=[
    ...new Set(arr)
]
```

结果：

```
[
1,
2,
3,
4
]
```

------

# 逐步拆解

首先：

```
new Set(arr)
```

得到：

```
Set{
1,
2,
3,
4
}
```

Set自动去重。

但是：

Set不是数组。

所以：

展开：

```
...
```

转换：

```
[
1,
2,
3,
4
]
```

------

完整过程：

```
Array

 ↓

Set去重

 ↓

展开

 ↓

Array
```

------

# 面试回答

问：

> 如何实现数组去重？

回答：

```
const unique=[...new Set(arr)]
```

原理：

> 利用 Set 不允许重复元素的特点，再通过展开运算符转换回数组。

------

# 四、Set 工作场景

## 场景1：标签去重

比如：

用户标签：

```
[
"Vue",
"React",
"Vue",
"JS"
]
```

去重：

```
const tags=[
...new Set(tags)
]
```

------

## 场景2：权限去重

例如：

```
const permissions=[
"user:add",
"user:add",
"user:delete"
]
```

使用 Set：

```
const uniquePermissions=[
...new Set(permissions)
]
```

------

## 场景3：判断是否存在

例如：

```
const bannedUsers=new Set([
1001,
1002,
1003
])
```

判断：

```
bannedUsers.has(1002)
```

比：

```
array.includes()
```

语义更明确。

------

# 五、Map（重点）

Map 是面试重点。

------

# 1. Map是什么？

一句话：

> Map 是专门存储 key-value 键值对的数据结构。

类似对象：

```
{
key:value
}
```

但是 Map 更强。

------

# 六、Map和Object区别（面试必问）

## Object

```
const obj={}

obj.name="Tom"
```

key：

只能：

```
字符串
symbol
```

------

## Map

```
const map=new Map()


map.set(
{name:"Tom"},
"用户数据"
)
```

key：

可以是任何类型。

比如：

```
string

number

object

function

array
```

------

# 七、Map基本操作

## 创建

```
const map=new Map()
```

------

## 添加 set

```
map.set(
"name",
"Tom"
)
```

------

## 获取 get

```
map.get("name")
```

结果：

```
Tom
```

------

## 判断 has

```
map.has("name")
```

------

## 删除 delete

```
map.delete("name")
```

------

## 数量 size

```
map.size
```

------

# 八、Map实现简单缓存（重点）

现在进入工作场景。

## 为什么需要缓存？

比如：

页面：

```
用户详情页
```

第一次：

请求：

```
GET /user/100
```

服务器返回：

```
{
id:100,
name:"Tom"
}
```

如果用户再次打开：

还请求服务器？

浪费。

可以缓存：

第一次请求：

```
服务器
 ↓
数据
 ↓
Map保存
```

第二次：

```
Map
 ↓
直接返回
```

------

# 九、实现一个简单缓存

## 第一步：创建缓存

```
const cache=new Map();
```

现在：

```
cache
{}
```

------

## 第二步：请求数据

模拟：

```
function fetchUser(id){

return {
id,
name:"Tom"
}

}
```

------

## 第三步：封装缓存函数

```
function getUser(id){

    if(cache.has(id)){

        console.log("读取缓存")

        return cache.get(id)

    }


    console.log("请求服务器")


    const user=fetchUser(id)


    cache.set(id,user)


    return user

}
```

------

# 执行过程

第一次：

```
getUser(1)
```

缓存：

空：

```
{}
```

执行：

```
cache.has(1)
```

false

请求：

```
服务器
```

得到：

```
{
id:1,
name:"Tom"
}
```

保存：

```
cache.set(1,user)
```

现在：

```
Map{
1=>user
}
```

------

第二次：

```
getUser(1)
```

判断：

```
cache.has(1)
```

true

直接：

```
cache.get(1)
```

返回。

不用请求。

------

# 十、真实项目哪里用 Map？

## 1. 请求缓存

比如：

```
userCache
productCache
menuCache
```

------

## 2. 防止重复请求

例如：

用户快速点击：

```
提交按钮
提交按钮
提交按钮
```

可以：

```
const pendingRequests=new Map()
```

保存正在请求的 Promise。

------

## 3. Vue列表快速查找

例如：

原数据：

```
users=[
{id:1,name:"Tom"},
{id:2,name:"Jack"}
]
```

转换：

```
const userMap=new Map(
users.map(user=>[
user.id,
user
])
)
```

以后：

找用户：

```
userMap.get(2)
```

不用：

```
find()
```

------

# 十一、Map + reduce经典面试题

数组：

```
const users=[
{
id:1,
name:"Tom"
},
{
id:2,
name:"Jack"
}
]
```

转换对象：

```
{
1:user,
2:user
}
```

reduce：

```
users.reduce((obj,user)=>{

obj[user.id]=user

return obj

},{})
```

Map：

```
const map=new Map(
users.map(user=>[
user.id,
user
])
)
```

------

# 十二、Set和Map面试对比

|      | Set      | Map     |
| ---- | -------- | ------- |
| 存储 | 值       | 键值对  |
| 重复 | 自动去重 | key唯一 |
| 访问 | has      | get     |
| 用途 | 去重     | 缓存    |
| 类似 | 数组     | 对象    |

------

# 十三、你现在必须掌握的两个代码

## 1. 数组去重

```
const unique=[
...new Set(arr)
]
```

------

## 2. 简单缓存

```
const cache=new Map()


function getData(id){

if(cache.has(id)){
return cache.get(id)
}


const data=request(id)

cache.set(id,data)

return data

}
```

------

## Class

### 

手写：

- Person
- Student继承

复习：

prototype

------

## 模块化

自己拆：

```
utils.js

request.js

api.js

main.js
```

模拟真实项目。

------

## 综合项目：

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