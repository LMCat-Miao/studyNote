# 一、为什么需要axios？

先回忆：

你之前写：

```
function getDashboardData(){

    userCount.value=120

}
```

这是假的。

真实项目：

数据来自服务器。

例如：

用户列表：

```
Vue

↓

请求

↓

后端接口

↓

数据库

↓

返回数据

↓

页面展示
```

这个过程：

叫：

## HTTP请求

------

JavaScript原生：

可以使用：

```
fetch()
```

例如：

```
fetch(
"/api/user"
)
.then(res=>res.json())
.then(data=>{

})
```

但是企业项目很少直接用。

原因：

fetch：

- 没有统一错误处理
- token不好管理
- 请求配置麻烦

所以：

使用axios。

------

# 二、axios是什么？

一句话：

> axios是一个发送HTTP请求的工具。

例如：

获取用户：

```
axios.get("/user")
```

发送登录：

```
axios.post(
"/login",
{
username,
password
}
)
```

------

但是：

项目里面不会这样写：

Login.vue：

```
axios.post()
```

Home.vue：

```
axios.get()
```

User.vue：

```
axios.get()
```

为什么？

------

假设公司接口全部变化：

以前：

```
http://old-api.com
```

换：

```
http://new-api.com
```

如果你写了：

100个页面：

修改100次。

所以：

需要封装。

------

# 三、为什么需要request.js？

真实项目结构：

```
页面

↓

api

↓

request.js

↓

axios

↓

服务器
```

例如：

Login.vue：

只负责：

“我要登录”

```
login(form)
```

api/user.js：

负责：

“登录接口地址”

```
login(){

return request.post("/login")

}
```

request.js：

负责：

“所有请求规则”

比如：

- baseURL
- token
- 错误处理

------

# 四、创建request.js

现在你的项目：

src下面创建：

```
src

├── utils

    └── request.js
```

------

打开：

```
src/utils/request.js
```

写：

```
import axios from "axios"


const request = axios.create({

    baseURL:"http://localhost:3000",

    timeout:5000

})


export default request
```

------

现在解释。

## axios.create()

什么意思？

原本：

```
axios.get()
```

这是全局axios。

我们创建自己的：

```
request
```

以后：

项目全部使用：

```
request.get()
```

------

## baseURL

例如：

接口：

```
/user/list
```

自动拼接：

```
http://localhost:3000/user/list
```

------

## timeout

超过：

5秒

请求失败。

避免：

用户一直等待。

------

# 五、现在加入请求拦截器

问题：

登录成功后：

服务器给：

token

例如：

```
abc123
```

以后：

请求：

```
/user/list
```

必须带：

```
Authorization: abc123
```

怎么办？

每个接口手写：

```
headers:{
token
}
```

太麻烦。

所以：

请求拦截器。

------

在request.js加入：

```
request.interceptors.request.use(

config=>{


config.headers.Authorization =`Bearer ${token}`


if(token){

config.headers.Authorization=

}


return config


},


error=>{


return Promise.reject(error)

}

)
```

------

逐行解释。

## 进入请求前：

```
request.interceptors.request.use()
```

意思：

所有请求发送之前：

先经过这里。

流程：

```
request.get()

↓

请求拦截器

↓

服务器
```

------

这里：

```
const token=
localStorage.getItem("token")
```

取登录保存的token。

------

如果存在：

```
config.headers.Authorization =`Bearer ${token}`
```

自动添加。

------

以后：

你写：

```
request.get("/user")
```

实际发送：

```
GET /user

headers:

token: abc123
```

------

# 六、响应拦截器

请求回来：

也需要统一处理。

例如：

服务器返回：

成功：

```
{
code:200,
data:[]
}
```

失败：

```
{
code:401,
message:"token过期"
}
```

每个页面判断：

很麻烦。

所以：

响应拦截器。

加入：

```
request.interceptors.response.use(

response=>{


return response.data


},


error=>{


return Promise.reject(error)


}

)
```

------

流程：

```
服务器返回

↓

响应拦截器

↓

页面
```

------

为什么：

```
return response.data
```

因为：

axios默认：

```
response={
data:{},
status:200,
headers:{}
}
```

我们一般只需要：

data。

所以简化。

------

# 七、现在你的request.js完整版本

```
import axios from "axios"



const request = axios.create({

baseURL:"http://localhost:3000",

timeout:5000

})





// 请求拦截器

request.interceptors.request.use(

config=>{


const token =
localStorage.getItem("token")


if(token){

config.headers.Authorization =`Bearer ${token}`

}


return config


},


error=>{


return Promise.reject(error)

}

)






// 响应拦截器

request.interceptors.response.use(

response=>{


return response.data


},


error=>{


return Promise.reject(error)


}

)



export default request
```

------

# 八、现在把你的知识串起来

你之前学：

Promise：

```
axios.get()
返回Promise
```

async/await：

```
const res =
await request.get()
```

Vue：

```
ref
```

页面更新。

完整链路：

```
Home.vue

↓

onMounted()

↓

await request.get()

↓

axios

↓

请求拦截器

↓

服务器

↓

响应拦截器

↓

修改ref

↓

页面更新
```

这就是企业Vue开发。

# 九、为什么需要 Bearer？

你可能会问：

为什么不是：

```
Authorization: abc123
```

而是：

```
Authorization: Bearer abc123
```

原因：

Authorization支持不同认证方式。

例如：

## Basic认证

用户名密码：

```
Authorization: Basic xxxxx
```

------

## Bearer认证

Token：

```
Authorization: Bearer xxxxx
```

------

## OAuth

也是：

```
Authorization: Bearer token
```

所以：

Bearer意思：

> 持有这个token的人，就是认证用户。

## 为什么axios请求拦截器里添加Authorization？

可以这样回答：

> 因为项目中的接口通常需要身份认证，登录成功后后端返回token。通过axios请求拦截器，可以在每次请求发送前自动将token添加到Authorization请求头中，避免每个接口重复手动传递，同时方便统一管理。