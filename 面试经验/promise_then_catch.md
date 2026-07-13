## 面试重点

### 1. Promise状态为什么不可逆？

回答：

> Promise状态只能从pending变为fulfilled或者rejected，一旦改变不能再次修改，这是为了保证异步结果的一致性。

------

### 2. Promise.then为什么可以链式调用？

回答：

> 因为then方法返回一个新的Promise，因此可以继续调用then。

------

### 3. async await和Promise关系？

回答：

> async await是Promise的语法糖，本质还是基于Promise实现，让异步代码更接近同步写法。

------

### 4. 微任务宏任务

必问：

```
setTimeout(()=>{},0)

Promise.resolve().then(()=>{})
```

谁先执行？

答案：

Promise。

因为：

```
微任务 > 宏任务
```

------

### 5. axios为什么封装？

回答：

> 封装axios可以统一管理请求配置、请求拦截、响应处理、错误处理，提高代码复用性和维护性。

### 6.如果then里面返回Promise呢？

这是面试重点。

例如：

```
function getUser(){

    return new Promise(resolve=>{

        setTimeout(()=>{

            resolve("Tom");

        },1000)

    })

}


getUser()
.then(name=>{

    console.log(name);

});
```

1秒后：

输出：

```
Tom
```

------

再看：

```
getUser()
.then(name=>{

    return getUserInfo(name);

})
.then(info=>{

    console.log(info);

});
```

流程：

```
getUser()

↓

Promise

↓

then

↓

返回新的Promise

↓

下一个then等待它完成
```

这就是链式调用。

## 1. resolve和reject有什么区别？

回答：

> resolve表示Promise成功完成，会将结果传递给then；reject表示Promise失败，会将错误原因传递给catch。

------

## 2. 为什么then可以链式调用？

回答：

> 因为then方法会返回一个新的Promise，后续then接收的是这个新的Promise的结果。

------

## 3. resolve之后还能reject吗？

例如：

```
new Promise((resolve,reject)=>{

    resolve("成功");

    reject("失败");

});
```

结果：

成功。

原因：

> Promise状态一旦从pending变成fulfilled/rejected后不可改变。

------

## 4. then里面return普通值和return Promise有什么区别？

普通值：

```
return 100;
```

会包装成：

```
Promise.resolve(100)
```

返回Promise：

```
return new Promise()
```

后面的then会等待它完成。

## Promise中的异常是怎么产生的

可以回答：

> Promise中的异常既可以由开发者主动通过throw或者reject产生，也可以由JavaScript运行时自动产生，例如类型错误、引用错误等。如果异常发生在Promise链中，Promise会自动将状态变为rejected，并由catch进行捕获处理。

### 1. async 和 Promise 的关系？

> **async/await 是 Promise 的语法糖，本质还是基于 Promise 实现。**

------

### 2. await 后面可以跟什么？

> **任何 Promise，或者能被包装成 Promise 的值。**

------

### 3. 为什么 axios 可以 await？

> **因为 axios 的请求方法（如 `axios.get()`）返回 Promise。**

------

### 4. 为什么要封装 axios？

标准回答：

- 统一 `baseURL`
- 统一超时时间
- 统一添加 Token
- 统一错误处理
- 统一响应数据格式
- 提高代码复用性，便于维护

> Token 是什么？

回答：

> Token 是服务器签发给客户端的一段身份凭证，用于标识当前登录用户。客户端会在后续请求中携带 Token，服务器通过验证 Token 来确认用户身份。

------

### 面试官：

> Authorization 是什么？

回答：

> Authorization 是 HTTP 请求头中的一个字段，用于携带认证信息。在前后端分离项目中，通常将 Token 放在 Authorization 请求头中发送给服务器。

------

### 面试官：

> 为什么 axios 要在请求拦截器里加 Token？

回答：

> 因为项目中几乎所有需要登录权限的接口都要携带 Token。将添加 Token 的逻辑统一放到请求拦截器中，可以避免重复代码，保证每个请求都自动携带认证信息，便于维护。

### Q1：为什么封装axios？

> 统一管理请求配置、token添加、错误处理和响应格式，提高代码复用性和维护性。

------

### Q2：axios为什么可以使用async/await？

> 因为axios请求方法返回Promise，而await可以等待Promise完成。

------

### Q3：请求拦截器有什么作用？

> 在请求发送之前统一修改请求，例如添加token、设置请求头、显示loading。

------

### Q4：响应拦截器有什么作用？

> 对服务器返回结果统一处理，例如格式化数据、处理错误状态、登录过期。

### 高频面试问题

> 为什么说 axios 拦截器本质是 Promise 链？

你可以回答：

> axios 内部将请求拦截器、实际请求发送函数、响应拦截器按照顺序组成 Promise 链。请求拦截器通过 return config 将修改后的配置传递给后续请求，响应拦截器通过 return response 修改最终返回值。由于 Promise 链具有值传递和错误冒泡机制，所以拦截器可以实现请求前处理、响应后处理以及统一错误处理。

## 1. 为什么封装request？

回答：

> 为了统一管理项目中的HTTP请求，包括baseURL配置、token携带、请求错误处理和响应数据格式化，提高代码复用性和维护性。

------

## 2. token为什么放请求拦截器？

回答：

> 因为token是所有接口共享的认证信息，将其放入请求拦截器可以保证每次请求自动携带，避免业务代码重复处理。

------

## 3. 401为什么在响应拦截器处理？

回答：

> 401属于服务器返回的认证失败状态，响应拦截器可以统一捕获并处理，例如清除token、跳转登录页。

------

## 4. 为什么return Promise.reject(error)？

回答：

> 因为axios内部基于Promise链，reject可以让错误继续向下传递，使调用方能够通过try/catch或者catch捕获错误。

# 校招面试高频问题

## 1. 登录流程是什么？

回答：

> 用户输入账号密码，前端调用登录接口，后端验证成功后返回token，前端保存token，之后通过请求拦截器携带token访问需要权限的接口。

------

## 2. 为什么token存在localStorage？

回答：

> 因为localStorage可以持久化保存数据，页面刷新后仍然存在，可以保证用户登录状态。

（补充：实际生产项目也可能使用 HttpOnly Cookie，提高安全性）

------

## 3. 为什么请求拦截器添加token？

回答：

> 因为所有需要认证的接口都需要token，将逻辑统一放在请求拦截器可以避免重复代码。

------

## 4. token过期怎么办？

回答：

> 后端返回401状态码，前端响应拦截器捕获后清除token，并跳转登录页面。

# 面试官让你解释 request.js，你这样说：

> 我会基于 axios.create 创建项目自己的请求实例，然后通过请求拦截器统一添加 token，通过响应拦截器统一处理返回数据和错误状态，例如401登录过期。由于 axios 返回 Promise，所以业务层可以直接使用 async/await 调用接口。