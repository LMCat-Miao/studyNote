### 关于this

```
const person = {
    name: "Tom",

    show() {
        return () => {
            console.log(this.name);
        };
    }
};

const obj = {
    name: "Jerry"
};

const fn = person.show();

obj.fn = fn;

obj.fn();
```

你可以按照我们今天建立的流程：

1. `show()` 的 `this` 是谁？
2. 箭头函数创建时保存了谁的 `this`？
3. 后来虽然变成 `obj.fn()` 调用了，它会不会改变箭头函数的 `this`？

### #我自己的推理结果：

show的this指向的是person，箭头函数没有自己的this，他继承了show的this，obj.fn()obj调用了fn函数，obj=this，调用了obj.fn，但是不会改变箭头函数,最后的结果是Tom

## 第一步

```
const fn = person.show();
```

执行：

```
person.show();
```

谁调用？

```
person
```

所以：

```
show 的 this = person
```

✅ 正确。

------

## 第二步

```
return () => {
    console.log(this.name);
};
```

这是箭头函数。

箭头函数说：

> 我没有自己的 this。

于是：

```
this = show 的 this
```

而：

```
show 的 this = person
```

所以：

箭头函数**永久保存了**：

```
this = person
```

注意这里我用了"永久"两个字。

因为：

**箭头函数的 this 一旦确定，以后谁调用都不会变。**

------

## 第三步

```
obj.fn = fn;
```

只是：

```
obj.fn
      │
      ▼
箭头函数
```

没有执行。

------

## 第四步

```
obj.fn();
```

你说：

> obj 调用了 fn。

✅ 这句话没错。

但是！

这里有一个特别重要的地方。

普通函数：

JavaScript 会说：

```
obj 调用了

↓

this = obj
```

箭头函数：

JavaScript 会说：

```
不好意思

我没有 this

我继续使用创建时保存好的 this
```

于是：

```
this 仍然是 person
```

所以：

```
console.log(this.name);
```

实际上就是：

```
console.log(person.name);
```

输出：

```
Tom
```

------

# 第一题 ⭐（热身）

输出什么？

```
const person = {
    name: "Tom",

    show() {
        console.log(this.name);
    }
};

person.show();
```

------

# 第二题 ⭐

输出什么？

```
const person = {
    name: "Tom",

    show() {
        console.log(this.name);
    }
};

const fn = person.show;

fn();
```

------

# 第三题 ⭐⭐

输出什么？

```
function show() {
    console.log(this.name);
}

const person = {
    name: "Tom"
};

show.call(person);
```

如果把：

```
show.call(person);
```

改成：

```
show();
```

结果又是什么？

为什么？

------

# 第四题 ⭐⭐

输出什么？

```
function show(age) {
    console.log(this.name);
    console.log(age);
}

const person = {
    name: "Tom"
};

show.apply(person, [20]);
```

如果改成：

```
show.call(person, 20);
```

有什么区别？

------

# 第五题 ⭐⭐⭐（开始有难度）

输出什么？

```
const person = {
    name: "Tom",

    show() {
        const test = () => {
            console.log(this.name);
        };

        test();
    }
};

person.show();
```

为什么？

### #解答

第一步

```
person.show();
this = person
```

------

第二步

创建箭头函数：

```
const test = () => {}
```

箭头函数：

```
没有自己的 this

↓

继承 show 的 this

↓

this = person
```

------

第三步

```
test();
```

虽然：

```
test()
```

前面没有对象。

但是：

它是：

```
箭头函数
```

不会重新绑定 this。

所以：

```
this 还是 person
```

输出：

```
Tom
```

------

# 第六题 ⭐⭐⭐

输出什么？

```
const person = {
    name: "Tom",

    show: () => {
        console.log(this.name);
    }
};

person.show();
```

为什么？

这里最容易掉坑。

### 解答：

因为：

```
show: () => {}
```

这是：

```
箭头函数
```

```
没有自己的 this
```

它去哪里找？

```
外层
```

这里外层是谁？

```
Global
```

不是：

```
person
```

所以：

```
this ≠ person
```

**箭头函数定义在普通函数里面 → 继承普通函数的 `this`（第 5 题）**

**箭头函数直接定义在对象属性上 → 它继承的是外层，而不是对象（第 6 题）**

------

# 第七题 ⭐⭐⭐⭐（this 丢失）

输出什么？

```
const person = {
    name: "Tom",

    show() {
        console.log(this.name);
    }
};

setTimeout(person.show, 1000);
```

为什么不是：

```
Tom
```

怎么修改才能输出：

```
Tom
```

至少写两种方法。

------

# 第八题 ⭐⭐⭐⭐（bind）

输出什么？

```
function show() {
    console.log(this.name);
}

const person = {
    name: "Tom"
};

const fn = show.bind(person);

fn();
```

如果：

```
const obj = {
    name: "Jerry"
};

obj.fn = fn;

obj.fn();
```

输出什么？

为什么？

------

# 第九题 ⭐⭐⭐⭐⭐（面试最爱）

输出什么？

```
const person = {
    name: "Tom",

    show() {
        return () => {
            console.log(this.name);
        };
    }
};

const obj = {
    name: "Jerry"
};

const fn = person.show();

obj.fn = fn;

obj.fn();
```

为什么？

------

# 第十题 ⭐⭐⭐⭐⭐（真正的大厂题）

不要运行。

认真分析。

```
const obj = {
    name: "Tom",

    show() {
        console.log(this.name);

        function test() {
            console.log(this.name);
        }

        const arrow = () => {
            console.log(this.name);
        };

        test();

        arrow();
    }
};

obj.show();
```

最终会输出几次？

分别输出什么？

为什么？



# 最后送你一个「this 万能分析模板」

以后无论面试遇到什么 `this` 题，都不要慌，按下面四步走：

### 第一步：看函数类型

```
普通函数？

还是箭头函数？
```

------

### 第二步：如果是普通函数

问自己：

> **是谁调用它？**

例如：

```
obj.fn();
```

就是：

```
this = obj
```

------

### 第三步：如果是箭头函数

问自己：

> **它在哪里定义？**

然后：

```
继承最近外层非箭头函数（或执行上下文）的 this
```

记住一句话：

> **箭头函数的 `this` 在创建时决定，不在调用时决定。**

------

### 第四步：有没有 `call`、`apply`、`bind`

如果有：

- 普通函数：可以修改 `this`
- 箭头函数：**不能修改 `this`**