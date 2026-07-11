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