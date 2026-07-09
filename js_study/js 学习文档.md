# js 学习文档

### 一.变量

------

##### 什么是变量

> 存放数据的容器，变量保存的不是数据，而是数据所指向的地址

没有变量的话，一个值要用很多次的话，修改要修改很多次,有变量的话只需要修改变量就可以了

~~~js
let age = 18;

console.log(age);
console.log(age);
console.log(age);//18

age = 20; 
console.log(age);//20
~~~

> 作用：保存数据，方便重复使用



### 二.var,let,const

------

##### 1.var

1.var的写法

~~~js
var name = "tom";

console.log(name);
~~~

​     也可以修改成：

~~~js
var age = 20;

age = 21;

console.log(age); //21
~~~

2.var可以重复声明变量

~~~js
var a = 10;
var a = 20;
console.log(a);//输出来a=20
~~~

3.var存在变量提升（Hoisting）

~~~js
conlog.log(a);
var a = 20;
~~~

> 会出现undefined的情况，那么实际上js上述代码会变成了下面的格式

~~~js
var a;
console.log(a);
a = 20;
~~~

##### 2.let

1. let写法

   ~~~js
   let a = 20;
   
   console.log(a);
   ~~~

   可以修改成：

   ~~~js
   let age = 20;
   
   age = 30;
   
   console.log(age);//30
   ~~~

2. let不允许重复声明

   ~~~js
   let age = 20;
   
   let age = 30;
   
   console.log(age);//报错 Identifier 'age' has already been declared
   ~~~

   > 主要是防止变量冲突，防止共用一个盒子

3. let没有变量提升

   

   ~~~js
   console,log(age);//报错 Cannot access 'age' before initialization
   
   let age = 20;
   ~~~

   > 

4. let有块级作用域

   ~~~js
   {
       let a = 100;
   }
   
   console.log(a);//报错 Assignment to constant variable.
   ~~~

   > 原因：在块（{}）里面的变量只能在块里面使用

#####   3.const

1.  不能重新赋值（不是不能修改数据）

   ~~~js
   const PI = 3.14
   
   const PI = 3.1415//报错
   ~~~

2. const必须初始化

   ~~~js
   const age = 20;
   
   const age;//这样写是错误的，必须赋值
   ~~~

   

3. const能修改对象吗     

   

   ~~~js
   const person = {
       name: "Tom"
   };
   
   person.name = "Jack";
   ~~~

   这样写不会报错

   > const保证的是变量指向不能改变

   但是如果地址变了，他就会报错

   ~~~js
   person = {};
   ~~~

      const声明的变量是不能重新赋值，但是如果变量保存的数组或者对象，内部的数据任然可以修改，因为const限制的是引用地址，而不是对象本身

   

4. 如果想要强制限制对象完全不能修改怎么做

   使用Object.freeze()

   ~~~js
   const person={
       name:"Tom"
   };
   
   
   Object.freeze(person);
   
   
   person.name="Jack";
   ~~~

   结果是不会生效的，最后的结果还是

   ~~~js
   person{
       name:"Tom"
   }
   ~~~

   Object.freeze()是浅冻结，冻结的是第一层的对象，Object.freeze()限制的是对象内容

   ~~~js
   const user={
       info:{
           age:20
       }
   };
   
   
   Object.freeze(user);
   
   
   user.info.age=30;
   ~~~

   上述是可以修改的

   

   ##### 4.var,let,const的区别

   | 特性             | var        | let        | const      |
   | ---------------- | ---------- | ---------- | ---------- |
   | ES版本           | ES5        | ES6        | ES6        |
   | 是否可以重新赋值 | 可以       | 可以       | 不可以     |
   | 是否可以重复声明 | 可以       | 不可以     | 不可以     |
   | 作用域           | 函数作用域 | 块级作用域 | 块级作用域 |
   | 是否变量提升     | 是         | 否         | 否         |
   | 是否必须初始化   | 否         | 否         | 是         |
   | 是否推荐使用     | 否         | 推荐       | 默认使用   |

   > var 有变量提升，let 和 const 没有
   >  let 可以修改，const 不可以修改

   > 默认使用const，修改使用let,基本上不使用var

### 三.数据类型

------

1. ##### 什么是数据类型

   理解一个概念

   > 数据类型决定了数据是什么，以及JS要怎么处理它

   比如：

   ```
   let age = 18;
   ```

   JavaScript 看到：

   ```
   18
   ```

   知道：

   这是数字。

   所以它可以：

   ```
   age + 10
   ```

   结果：

   ```
   28
   ```

   ------

   但是：

   ```
   let name = "Tom";
   ```

   这里：

   ```
   "Tom"
   ```

   是字符串。

   所以：

   ```
   name + "你好"
   ```

   结果：

   ```
   Tom你好
   ```

   ------

   不同的数据类型，有不同的行为。

2. ##### JavaScript的数据类型分类

   JavaScript 数据类型分两大类：

   ```
   数据类型
   
   ├── 基本数据类型（Primitive）
   │
   └── 引用数据类型（Reference）
   ```

3. ##### 基本数据类型（特点：保存值    复制独立）

   1. ###### Number（数字）

      JavaScript 的 Number 类型里面，不只有普通数字：

      ```
      Number
      |
      ├── 正常数字
      │
      ├── Infinity（无限大）
      │
      └── NaN
      ```

      例如：

      ```
      let age = 18;
      
      let price = 99.9;
      ```

      都是 Number。

      JavaScript 不区分：

      ```
      int
      float
      double
      ```

      全部：

      ```
      Number
      ```

      > 特殊值NaN

      NaN：

      Not a Number

      不是一个有效数字。

      例如：

      ```
      let a = "hello" - 10;
      
      console.log(a);
      ```

      结果：

      ```
      NaN
      ```

      因为：

      ```
      hello
      
      不能转换成数字
      ```

      ------

      但是：

      注意：

      ```
      typeof NaN
      ```

      结果：

      ```
      number
      ```

      这是面试经典。

      为什么？

      因为 NaN 是 Number 类型里面表示：

      > 无效数字

      它不是字符串，也不是其他类型。

      NaN表示一个无法确定的数字结果，两个错误结果不一定相同

      ~~~js
      console.log(NaN === NaN);
      
      false//错误结果
      ~~~

   2. ###### String（字符串）

      字符串就是文本。

      例如：

      ```
      let username="Tom";
      ```

      三个写法：

      ```
      "hello"
      
      'hello'
      
      `hello`
      ```

      ------

      > 模板字符串（工作非常常用）

      以前：

      ```
      let name="Tom";
      
      console.log("你好"+name);
      ```

      现在：

      ```
      let name="Tom";
      
      console.log(`你好${name}`);
      ```

      结果：

      ```
      你好Tom
      ```

      ------

      > &{}中不仅可以放变量，也可以放表达式

      例如：

      ```
      let a=10;
      let b=20;
      
      
      console.log(`结果是${a+b}`);
      ```

      执行：

      ```
      ${10+20}
      ```

      结果：

      ```
      结果是30
      ```

      ------

      甚至函数：

      ```
      function getName(){
          return "Tom";
      }
      
      
      console.log(`你好${getName()}`);
      ```

      结果：

      ```
      你好Tom
      ```

      工作场景：

      例如接口返回：

      ```
      {
       username:"张三"
      }
      ```

      页面：

      ```
      `欢迎${username}登录`
      ```

      非常常见。

   3. ###### Boolean(布尔值)

      只有：

      ```
      true
      
      false
      ```

      主要用于判断。

      例如：

      登录状态：

      ```
      const isLogin=true;
      ```

      权限：

      ```
      const isAdmin=false;
      ```

      ------

      工作：

      大量 if 判断：

      ```
      if(isLogin){
      
          showPage();
      
      }
      ```

   4. ###### Undefined

      意思：

      > 已经声明，但是没有值。

      例如：

      ```
      let age;
      
      console.log(age);
      ```

      结果：

      ```
      undefined
      ```

      ------

      什么时候出现？

      常见：

      1. 声明变量没赋值

      ```
      let a;
      ```

      ------

      2. 函数没有返回值

      ```
      function fn(){
      
      }
      
      console.log(fn());
      ```

      结果：

      ```
      undefined
      ```

      ------

      3. 对象不存在属性

      ```
      const user={
          name:"Tom"
      };
      
      
      console.log(user.age);
      ```

      结果：

      ```
      undefined
      ```

   5. ###### Null（一个表示“空对象引用”的特殊基本类型值。）

      表示：

      > 空值，主动设置为空。

      例如：

      ```
      let user=null;
      ```

      意思：

      现在没有用户。

      以后：

      ```
      user={
       name:"Tom"
      }
      ```

   6. ###### Symbol

      ES6新增。

      作用：

      创建唯一值。

      例如：

      ```
      let id=Symbol();
      ```

      两个 Symbol：

      ```
      Symbol()==Symbol()
      ```

      结果：

      ```
      false
      ```

      因为每一个都是唯一的。

      ------

      用途：

      对象唯一属性。

      例如：

      ```
      const id=Symbol();
      
      const user={
          [id]:100
      }
      ```

      ------

      普通开发较少。

   7. ###### Biglnt

      处理超大整数。

      例如：

      ```
      let num=999999999999999999n;
      ```

      末尾：

      ```
      n
      ```

      表示 BigInt。

      普通 Number：

      最大安全整数：

      ```
      Number.MAX_SAFE_INTEGER
      ```

      超过可能不准确。

4. ##### 引用数据类型（特点：保存地址，复制引用，可能共享数据）

   1. 主要：

      ```
      Object
      Array
      Function
      ```

   1. Obiect

      对象：

      保存多个数据。

      例如：

      ```
      const user={
          name:"Tom",
          age:18
      };
      ```

      结构：

      ```
      user
      
      ↓
      
      对象地址
      
      
      {
       name:"Tom",
       age:18
      }
      ```

      ------

      访问：

      ```
      user.name
      ```

      结果：

      ```
      Tom
      ```

   2. Array

      存多个值。

      例如：

      ```
      const arr=[1,2,3];
      ```

      访问：

      ```
      arr[0]
      ```

      结果：

      ```
      1
      ```

      ------

      注意：

      数组也是对象。

      ```
      typeof []
      ```

      结果：

      ```
      object
      ```

   3. Function

      函数也是对象。

      例如：

      ```
      function fn(){
      
      }
      ```

      ------

      typeof：

      ```
      typeof fn
      ```

      结果：

      ```
      function
      ```

      注意：

      虽然函数也是对象。

      但是 typeof 特殊处理。

5. ##### 如何准确判断类型

   ###### 方法1：typeof

   适合：

   基本类型。

   ------

   ###### 方法2：instanceof

   判断对象关系。

   例如：

   ```
   [] instanceof Array
   ```

   结果：

   ```
   true
   ```

   ------

   ###### 方法3：Object.prototype.toString（面试高级）

   例如：

   ```
   Object.prototype.toString.call([])
   ```

   结果：

   ```
   [object Array]
   ```

   最准确。

### 四.数据转换

------

1. ##### JavaScript为什么要有数据转换

   先看一个例子。

   ```
   let a = "100";
   let b = 20;
   
   console.log(a + b);
   ```

   输出：

   ```
   10020
   ```

   为什么不是：

   ```
   120
   ```

   因为：

   这里：

   ```
   a
   ```

   是：

   ```
   String
   ```

   而：

   ```
   b
   ```

   是：

   ```
   Number
   ```

   两个类型不同。

   JavaScript 必须做一件事情：

   > **先把其中一个数据转换成另一个类型，然后才能计算。**

   这种过程就叫：

   > **类型转换（Type Conversion）**

2. ##### 为什么JavaScript会自动转换

   因为 JavaScript 是：

   > **弱类型语言（Weakly Typed Language）**

   例如：

   ```
   let a = 10;
   ```

   后来：

   ```
   a = "hello";
   ```

   完全没问题。

   说明：

   变量没有固定类型。

   JavaScript 会自动帮你转换。

3. ##### JavaScript有两种类型转换

   第一种：

   ###### 显式转换（主动转换）

   就是：

   开发者自己写。

   例如：

   ```
   Number("100")
   ```

   或者：

   ```
   String(100)
   ```

   或者：

   ```
   Boolean(0)
   ```

   都是：

   你主动要求转换。

   ------

   第二种：

   ###### 隐式转换（自动转换）

   例如：

   ```
   "100"+20
   ```

   你没有写：

   ```
   String()
   ```

   但是：

   JavaScript 自动把：

   ```
   20
   ```

   变成：

   ```
   "20"
   ```

   所以得到：

   ```
   "10020"
   ```

   这就是：

   隐式转换。

4. ##### JavaScript什么时候发生类型转换

   记一句口诀：

   > **当不同类型的数据需要一起运算或者比较时，JavaScript 就可能发生类型转换。**

   最常见有四种：

   ```
   +
   
   -
   
   ==
   
   if()
   ```

5. ##### 加号（+）最特殊

   ```
   console.log(1+2);
   ```

   结果：

   ```
   3
   ```

   因为：两个都是数字。直接加。

   ------

   但是：

   ```
   console.log("1"+2);
   ```

   结果：

   ```
   12
   ```

   因为：

   JavaScript 看见：

   ```
   String
   ```

   于是：

   > **这是字符串拼接。**

   于是：

   把：

   ```
   2
   ```

   变成：

   ```
   "2"
   ```

   过程：

   ```
   "1"+2
   
   ↓
   
   "1"+"2"
   
   ↓
   
   "12"
   ```

   ------

   再看：

   ```
   console.log(1+"2");
   ```

   结果：

   ```
   12
   ```

   过程：

   ```
   1
   
   ↓
   
   "1"
   
   ↓
   
   "1"+"2"
   
   ↓
   
   "12"
   ```

   ------

   所以：

   > 面试口诀①

   > **只要 + 两边有一个字符串，就会变成字符串拼接。**

6. ##### 减号不会拼字符串

   例如：

   ```
   console.log("100"-20);
   ```

   结果：

   ```
   80
   ```

   减法不能拼字符串。

   JavaScript 会：

   把：

   ```
   "100"
   
   ↓
   
   100
   ```

   然后：

   ```
   100-20
   
   ↓
   
   80
   ```

   ------

   再看：

   ```
   console.log("100"*2);
   ```

   结果：

   ```
   200
   ```

   因为：

   乘法也必须是数字。

   ------

   再看：

   ```
   console.log("100"/2);
   ```

   结果：

   ```
   50
   ```

   ------

   所以：

   > 面试口诀②

   除了：

   ```
   +
   ```

   其他：

   ```
   -
   
   *
   
   /
   
   %
   ```

   都会优先转成 Number。

7. ##### Boolean转Number

   JavaScript 内部规定：

   ```
   true
   
   ↓
   
   1
   
   false
   
   ↓
   
   0
   ```

   ------

   所以：

   ```
   true+1
   ```

   过程：

   ```
   1+1
   
   ↓
   
   2
   ```

   结果：

   ```
   2
   ```

8. ##### Null转Number

   JavaScript 规定：

   ```
   null
   
   ↓
   
   0
   ```

   ------

   例如：

   ```
   console.log(null+1);
   ```

   过程：

   ```
   0+1
   
   ↓
   
   1
   ```

   结果：

   ```
   1
   ```

9. ##### undefined 转 Number

   例如：

   ```
   Number(undefined)
   ```

   结果：

   ```
   NaN
   ```

   为什么？

   undefined表示：

   没有值。不能变成数字。

   所以：

   ```
   undefined
   
   ↓
   
   NaN
   ```

   例如：

   ```
   console.log(undefined+1);
   ```

   结果：

   ```
   NaN
   ```

### 五.作用域

1. 什么是作用域

   > 作用域，就是变量能够被访问的范围。

   例如：

   ```
   let age = 18;
   ```

   age属于：全局作用域。整个文件：几乎都可以访问。

   ------

   例如：

   ```
   function test(){
   
       let age = 18;
   
   }
   ```

   这里age只能里面访问

   ```
   test()
   ```

   外面直接报错

   ```
   console.log(age);
   ```

   因为已经离开它的作用域。

2. 为什么要有作用域

   如果没有作用域，那么整个程序将会变得很混乱，谁覆盖谁等很多问题的出现，所以作用域就是为了

   > 变量隔离

3. 有几种作用域

   三种

   > - 全局作用域
   > - 函数作用域
   > - 块级作用域

   1. 全局作用域

      例如：

      ```
      let name="Tom";
      ```

      它属于全局。任何函数都可以访问。

   2. 函数作用域

      例如：

      ```
      function test(){
      
          let age=18;
      
      }
      ```

      这里：

      ```
      test()
      
      ↓
      
      age
      ```

      只能test里面访问。外面没有办法访问。

      例如：

      ```
      test();
      
      console.log(age);
      ```

      报错。

      画图理解他们的逻辑：

      ```
      全局
      
      │
      
      ├──test()
      
            │
      
            └──age
      ```

      外面碰不到age，所以无法打印

   3. 块级作用域

      例如：

      ```
      {
      
          let age=18;
      
      }
      ```

      注意：

      这里只是一个花括号。不是函数。但是依然形成新的作用域。

      例如：

      ```
      {
      
          let age=18;
      
      }
      
      console.log(age);
      ```

      结果：报错。

      因为：

      let属于块级作用域。

      ------

      但是：

      ```
      {
      
          var age=18;
      
      }
      
      console.log(age);
      ```

      输出：

      ```
      18
      ```

      因为：var没有块级作用域。

4. 作用域的作用

   > 决定一个变量在哪里能访问，在哪里不能访问。

   例如：

   ```
   let a=1;
   
   function test(){
   
       let b=2;
   
   }
   ```

   这里：

   ```
   a
   
   全局
   
   所有地方
   
   ↓
   
   可以访问
   ```

   但是：

   ```
   b
   
   ↓
   
   只能：
   
   test()
   
   里面。
   ```

5. 作用域和生命周期的关系

   1. 1. **作用域（Scope）**：

         > **变量在哪里可以访问？**

         例如：

         ```
         function test() {
             let a = 10;
         }
         ```

         `a` 的作用域就是 `test` 函数内部。

         ------

         **生命周期（Lifetime）**：

         > **变量什么时候创建？什么时候销毁？**

         例如：

         ```
         function test() {
             let a = 10;
         }
         
         test();
         ```

         调用 `test()` 时：

         ```
         a 被创建
         ```

         函数执行结束：

         ```
         a 没有人引用
         
         ↓
         
         可以被垃圾回收（GC）释放
         ```

         所以：

         - **作用域**决定"能不能访问"。
         - **生命周期**决定"还在不在内存里"。

6. 作用域链

   作用域链的核心就是：内层作用域可以访问外层作用域，而外层作用域不能访问内层作用域。

   假设有下面的代码：

   ```
   let a = 1;
   
   function outer() {
       let b = 2;
   
       function inner() {
           let c = 3;
   
           console.log(a);
           console.log(b);
           console.log(c);
       }
   
       inner();
   }
   
   outer();
   ```

   把它画成作用域图，就是：

   ```
   全局作用域
   │
   ├── a = 1
   │
   └── outer作用域
        │
        ├── b = 2
        │
        └── inner作用域
             │
             └── c = 3
   ```

   JavaScript 查找变量，其实像爬楼梯一样。

   ```
   inner
   
   ↑
   
   outer
   
   ↑
   
   global
   ```

   一层一层往上找。

   这条"查找路线"就叫：

   > **作用域链（Scope Chain）**

规定：

> 内层可以访问外层，外层不能访问内层

​     这样每个函数都有自己的“私有空间”，防止某些重要信息泄露

在**函数定义的时候**就已经确定了作用域链不是动态生成的

例如：

```
let a = 1;

function test() {
    console.log(a);
}
```

当：

```
function test()
```

写出来的时候。

JavaScript：

已经知道：

```
test

↓

外层：

Global
```

所以以后无论在哪里调用。作用域链都不会改变。

------

例如：

```
let a = 1;

function test() {
    console.log(a);
}

function run() {
    let a = 100;

    test();
}

run();
```

```
1
```

输出的结果是1.

因为：

```
test
```

定义的时候。

外层：

就是：

```
Global
```

不是：

```
run
```

所以：

作用域链：

早就固定了。

这个特点：

叫：

> **词法作用域（Lexical Scope）**

也叫：

> **静态作用域。**

> **变量查找遵循什么原则？**

标准回答：

> **JavaScript 采用词法作用域。查找变量时，会先在当前作用域查找；如果没有，再沿着作用域链逐层向外查找，直到全局作用域；如果全局作用域也没有，则抛出 `ReferenceError`。查找只会向外，不会向内。**

### 六.this的定义与用法

------

1. this的本质

   > 是一个动态的代词
   >
   > this 其实是函数的一个隐藏参数。

   **核心概念**：`this` 是函数执行时的上下文对象，它的值**不是在函数定义时确定的，而是在函数执行时动态确定的**。同一个函数，用不同的方式调用，`this` 的指向可能完全不同。

2. this的五大绑定

   1. 规则一：默认绑定（独立函数调用）

      当函数被**直接调用**，没有通过任何对象时：

      - **非严格模式**：`this` 指向全局对象（浏览器中是 `window`，Node.js 中是 `global`）
      - **严格模式**：`this` 为 `undefined`

      ```
      function test() {
          console.log(this);
      }
      test(); // 非严格模式：window；严格模式：undefined
      ```

   2. 规则二：隐式绑定（对象方法调用）

      当函数作为**对象的方法**被调用时（如 `obj.fn()`），`this` 指向调用该方法的对象。

      javascript

      ```
      const obj = {
          name: '张三',
          sayHi() {
              console.log(this.name);
          }
      };
      obj.sayHi(); // 输出：'张三'，this 指向 obj
      ```

      **注意**：如果是多层嵌套（如 `obj1.obj2.fn()`），`this` 只绑定到**最后一层**调用者

   3. 规则三：显示绑定（call/apply/bind）

      通过 `call`、`apply` 或 `bind` 可以**手动指定**函数执行时的 `this` 指向。

      ```
      function greet() {
          console.log(`Hello, ${this.name}`);
      }
      const person = { name: 'Alice' };
      greet.call(person);  // Hello, Alice
      greet.apply(person); // Hello, Alice
      const bound = greet.bind(person);
      bound();             // Hello, Alice
      ```
      
      **三者区别**：

      - `call`：参数逐个传递
      - `apply`：参数以数组形式传递
      - `bind`：返回一个新函数，不立即执行
      
   4. 规则四：构造函数绑定（new关键字）

      当函数通过 `new` 关键字作为**构造函数**调用时，`this` 指向**新创建的实例对象**。

      javascript

      ```
      function Person(name) {
          this.name = name;
      }
      const p = new Person('张三');
      console.log(p.name); // '张三'
      
      
      ```

      call 可以传参数

      > call只能修改普通函数，不能修改箭头函数

      ```
      function show(age, city) {
          console.log(this.name);
          console.log(age);
          console.log(city);
      }
      
      const person = {
          name: "Tom"
      };
      
      show.call(person, 20, "哈尔滨");
      ```
   
      输出：
   
      ```
      Tom
      20
      哈尔滨
      ```
   
      这里：
   
      ```
      call(person, 20, "哈尔滨")
      ```
   
      第一个参数永远是：
   
      ```
      this
      ```
   
      后面的参数：
   
      就是函数自己的参数。
   
      #apply 和 call 几乎一样。
   
      唯一的区别：
   
      call：
   
      ```
      show.call(person, 20, "哈尔滨");
      ```
   
      参数一个一个写。
   
      ------
   
      apply：
   
      ```
      show.apply(person, [20, "哈尔滨"]);
      ```
   
      参数放数组里。
   
      输出：
   
      ```
      Tom
      20
      哈尔滨
      ```
   
      ###它和 call 最大区别：
   
      **call 会执行函数。**
   
      例如：
   
      ```
      show.call(person);
      ```
   
      执行了。
   
      ------
   
      但是：
   
      ```
      show.bind(person);
      ```
   
      不会执行。
   
      它会返回一个：
   
      **新的函数。**
   
      例如：
   
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
   
      输出：
   
      ```
      Tom
      ```
   
      注意：
   
      这里：
   
      ```
      show.bind(person);
      ```
   
      没有执行。
   
      它只是返回：
   
      ```
      一个新的函数
      ```
   
      这个新的函数：
   
      永远：
   
      ```
      this = person
      ```
   
      以后：
   
      ```
      fn();
      ```
   
      或者：
   
      ```
      obj.fn = fn;
      
      obj.fn();
      ```
   
      都不会改变。
   
      因为：
   
      bind 已经固定好了。
   
   5. 规则五：箭头函数——词法绑定
   
      箭头函数**没有自己的 `this`**，它会捕获**定义时所在外层作用域**的 `this` 值。
   
      ```
      const obj = {
          name: '张三',
          sayHi: () => {
              console.log(this.name); // this 来自外层（全局），不是 obj
          }
      };
      obj.sayHi(); // undefined（因为 window.name 不存在）
      ```
   
     **普通函数会根据调用方式决定 `this`。**
   
     **箭头函数根本不会根据调用方式决定 `this`。**  
   
      ~~~this` 不是在函数定义时确定的，而是在函数调用时由 JavaScript 引擎根据调用方式自动绑定的一个隐藏引用。普通函数拥有自己的 `this`，箭头函数没有自己的 `this`，而是从外层词法作用域继承。
   
   

### 七.闭包

