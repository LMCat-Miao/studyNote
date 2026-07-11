## ⭐⭐⭐⭐⭐ 第一梯队（100%）

### ① prototype 和 `__proto__` 有什么区别？

标准回答：

> prototype 是函数的属性，用来存放共享成员。

> `__proto__` 是对象的属性，指向构造函数的 prototype。

------

### ② new 做了什么？

必须回答：

```
① 创建对象

② 连接 prototype

③ this 指向对象执行构造函数

④ 返回对象
```

最好还能写：

```
myNew()
```

------

### ③ instanceof 原理

必须会画：

```
obj

↓

__proto__

↓

prototype

↓

...

↓

null
```

------

### ④ prototype 为什么存在？

不要回答：

> 为了继承。

真正答案：

> **为了共享方法，节省内存。**

继承只是后来利用了它。

------

### ⑤ class 和 prototype 什么关系？

必须回答：

> class 是 prototype 的语法糖。

------

# ⭐⭐⭐⭐ 第二梯队（80%）

### Object.create()

为什么：

```
Object.create(proto)
```

能够继承？

其实：

就是：

```
新对象

↓

__proto__

↓

proto
```

------

### constructor

为什么：

```
cat.constructor === Cat
```

成立？

因为：

constructor：

在：

```
Cat.prototype
```

里面。

------

### hasOwnProperty()

为什么：

```
tom.hasOwnProperty("eat")
```

返回：

```
false
```

因为：

eat：

在：

```
prototype
```

不是：

tom。

------

# ⭐⭐⭐ 第三梯队（中大厂）

### Object.getPrototypeOf()

和：

```
__proto__
```

关系？

------

### isPrototypeOf()

底层：

其实：

也是：

原型链查找。

------

### 为什么 null 没有 prototype？

因为：

```
Object.prototype.__proto__

↓

null
```

链必须结束。

