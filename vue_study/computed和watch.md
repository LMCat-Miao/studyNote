# 一、先理解一个真实业务场景

假设你的后台系统有用户信息：

```
const user = reactive({

    username:"李妙",

    role:"admin"

})
```

现在页面需要显示：

```
欢迎你，李妙
```

你可以直接：

```
<h1>
欢迎你，{{user.username}}
</h1>
```

没有问题。

------

但是现在需求变了：

如果管理员：

显示：

```
欢迎管理员 李妙
```

普通用户：

显示：

```
欢迎用户 李妙
```

怎么办？

你可能写：

```
<h1>

{{ 
 user.role==="admin"
 ?"欢迎管理员"+user.username
 :"欢迎用户"+user.username
}}

</h1>
```

虽然可以运行。

但是：

模板越来越复杂。

------

Vue提供：

## computed

把复杂计算抽出来。

------

# 二、computed是什么？

一句话：

> computed是根据已有数据，计算出一个新的数据。

例如：

已有：

```
用户角色
用户名
```

计算：

```
欢迎语
```

关系：

```
原始数据

username
role

↓

computed

↓

welcomeText
```

------

# 三、computed基本使用

代码：

```
<script setup>

import {reactive,computed} from "vue"


const user=reactive({

    username:"李妙",

    role:"admin"

})


const welcomeText=computed(()=>{


    if(user.role==="admin"){

        return "欢迎管理员 "+user.username

    }


    return "欢迎用户 "+user.username


})


</script>
```

模板：

```
<h1>

{{welcomeText}}

</h1>
```

显示：

```
欢迎管理员 李妙
```

------

# 四、computed内部发生了什么？

重点理解。

执行：

```
computed(()=>{

return user.username

})
```

Vue会记录：

这个计算依赖：

```
welcomeText

依赖

↓

user.username

user.role
```

以后：

如果：

```
user.username="Tom"
```

Vue知道：

影响welcomeText。

所以：

重新计算。

------

这个过程：

叫：

## 依赖追踪

也就是你之前学的响应式：

```
读取数据

↓

收集依赖


修改数据

↓

触发更新
```

------

# 五、computed为什么比函数好？

看区别。

## 方法

```
function getWelcome(){

return "欢迎"+user.username

}
```

模板：

```
{{getWelcome()}}
```

每次页面更新：

都会执行。

比如：

页面有：

```
按钮
列表
表格
```

任何一个地方更新：

函数重新运行。

------

## computed

```
const welcome=computed(()=>{

return "欢迎"+user.username

})
```

特点：

### 有缓存

第一次：

计算：

```
欢迎李妙
```

第二次使用：

直接拿结果。

只有：

username变化：

才重新计算。

------

所以：

面试回答：

> computed具有缓存机制，依赖的数据没有变化时不会重新计算，而普通方法每次渲染都会执行。

------

# 六、computed实际项目应用

你的后台项目里面：

很多地方都会用。

## 1. 用户欢迎语

```
const welcome=computed(()=>{

return `欢迎${user.name}`

})
```

------

## 2. 商品数量

原始：

```
const cart=ref([
商品1,
商品2
])
```

计算：

```
const total=computed(()=>{

return cart.value.length

})
```

------

## 3. 权限判断

以后：

```
const isAdmin=computed(()=>{

return user.role==="admin"

})
```

模板：

```
<button v-if="isAdmin">

删除用户

</button>
```

------

# 七、现在进入watch

如果说：

computed：

> 我需要一个计算结果

那么：

watch：

> 当某个数据变化时，我执行一个动作。

重点：

watch不是产生数据。

它是：

执行副作用。

------

什么叫副作用？

例如：

- 请求接口
- 修改localStorage
- 打印日志
- 开启定时器

这些都是副作用。

------

# 八、watch基本使用

例如：

监听用户名变化。

```
import {ref,watch} from "vue"


const username=ref("")


watch(username,(newValue,oldValue)=>{


console.log("新的值:",newValue)

console.log("旧的值:",oldValue)


})
```

------

用户输入：

第一次：

```
Tom
```

输出：

```
新的值:Tom
旧的值:
```

继续：

改：

```
Jack
```

输出：

```
新的值:Jack
旧的值:Tom
```

------

# 九、watch执行流程

例如：

```
username.value="Tom"
```

发生：

```
数据变化

↓

Vue发现username被watch监听

↓

执行watch函数

↓

发送请求
```

------

# 十、watch最常见场景：搜索框

比如后台：

用户列表。

输入：

```
Tom
```

自动搜索。

代码：

```
const keyword=ref("")


watch(keyword,(value)=>{


console.log("请求用户:",value)


})
```

用户输入：

```
a

↓

请求

ab

↓

请求

abc

↓

请求
```

这就是：

watch。

------

# 十一、watch和computed区别（面试重点）

这是高频问题。

## computed

关注：

### 计算结果

例如：

```
价格

↓

优惠后价格
```

代码：

```
const finalPrice=computed(()=>{

return price.value*0.8

})
```

------

## watch

关注：

### 数据变化后做事情

例如：

```
搜索词变化

↓

请求接口
```

代码：

```
watch(keyword,()=>{

fetchData()

})
```

------

一句话记忆：

> computed负责“算”，watch负责“做”。

------

# 十二、放入你的后台项目理解

## 登录页面

用户名：

```
const username=ref("")
```

密码：

```
const password=ref("")
```

这里不用computed。

因为：

不是计算。

------

## 登录按钮是否可点击？

例如：

用户名密码都有：

按钮可用。

这里用computed：

```
const canLogin=computed(()=>{


return username.value 
&& password.value


})
```

模板：

```
<button :disabled="!canLogin">

登录

</button>
```

------

## 登录成功监听token变化

watch：

```
watch(token,()=>{


console.log("token变化")


})
```