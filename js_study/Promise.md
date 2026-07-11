# 一、为什么需要 Promise？

## 1. JavaScript 为什么需要异步？

先回忆：

JavaScript 是：

> 单线程语言

什么意思？

一次只能执行一个任务。

例如：

```
console.log("开始");

console.log("结束");
```

执行：

```
开始

结束
```

正常。

------

但是如果：

```
console.log("开始");

请求服务器数据（需要3秒）;

console.log("结束");
```

如果 JS 等服务器：

```
开始

等待3秒

数据回来

结束
```

页面会卡死。

所以浏览器设计：

> 耗时任务交给其他线程处理，JS继续执行。

例如：

- 网络请求
- 定时器
- 文件读取
- 用户事件

这就是异步。

------

# 二、最原始的异步：回调函数 callback

例如：

请求数据：

```
getData(function(data){

    console.log(data);

});
```

意思：

> 数据回来以后，执行这个函数。

这个函数叫：

callback（回调函数）

------

## 一个请求没问题

例如：

```
login(function(user){

    getUserInfo(user,function(info){

        console.log(info);

    })

})
```

还能接受。

------

但是现实项目：

登录：

↓

获取用户信息：

↓

获取权限：

↓

获取菜单：

↓

获取列表：

于是：

```
login(function(user){

    getUserInfo(user,function(info){

        getPermission(info,function(permission){

            getMenu(permission,function(menu){

                getList(menu,function(list){

                })

            })

        })

    })

})
```

这是什么？

------

# 回调地狱 Callback Hell

特点：

1. 横向发展
2. 层级越来越深
3. 错误处理困难
4. 代码维护困难

所以出现 Promise。

------

# 三、Promise 是什么？

一句话：

> Promise 是一个管理异步操作结果的对象。

它把：

"未来才会完成的事情"

包装成一个对象。

------

例如：

```
const p = new Promise((resolve,reject)=>{

});
```

这个 p 是什么？

它代表：

> 一个未来可能成功，也可能失败的结果。

# 四、Promise 有三个状态（重点）

Promise 内部有三个状态：

```
pending
 ↓
fulfilled

或者

pending
 ↓
rejected
```

也就是：

## 1. pending

等待中：

```
请求还没有回来
```

------

## 2. fulfilled

成功：

```
resolve(data)
```

------

## 3. rejected

失败：

```
reject(error)
```

------

注意：

状态只能变化一次。

例如：

```
resolve("成功");

reject("失败");
```

结果：

还是成功。

因为：

Promise 一旦结束，就不能改变。

------

# 五、Promise 怎么解决回调地狱？

以前：

```
login(function(user){

});
```

现在：

```
login()
.then(user=>{

})
.then(info=>{

})
.then(menu=>{

})
.catch(error=>{

})
```

是不是变成：

流水线？

------

## Promise 链

核心：

```
.then()
```

返回新的 Promise。

例如：

```
Promise.resolve(1)

.then(value=>{

    return value+1;

})

.then(value=>{

    console.log(value);

});
```

执行：

第一次：

```
value =1
```

return：

```
2
```

第二个 then：

```
value=2
```

输出：

```
2
```

# 六、你必须理解 Promise 的执行顺序

这是面试重点。

看：

```
console.log(1);


new Promise((resolve)=>{

    console.log(2);

    resolve();

}).then(()=>{

    console.log(3);

});


console.log(4);
```

输出：

```
1
2
4
3
```

为什么？

因为：

Promise 本身执行器立即执行：

```
console.log(2)
```

但是：

then 是微任务。

执行顺序：

```
同步任务

↓

微任务

↓

宏任务
```

所以：

```
1
2
4
3
```