### git测试

##### 上传到远程仓库：

1.git init    	//初始化git

2.git clone xxx 	//克隆，xxx代表远程仓库的http或者是ssh

3.git status	//查看当前仓库的状态告诉你有哪些文件发生了变化

4.git add -A	//将修改添加后的文件上传到远程仓库区

5.git commit -m'  '	//’ ‘中写的是修改后自己给起的名称

6.git push origin main	//push是将本地仓库的内容上传到远程仓库中去

7.git pull origin main	//pull是将远程仓库的东西下拉到自己的本地仓库中

##### 修改当前仓库的远程地址

查看当前远程地址

bash

```
git remote -v
```

你会看到类似：

text

```
origin  https://github.com/用户名/仓库名.git (fetch)
origin  https://github.com/用户名/仓库名.git (push)
```

更改为 SSH 地址

bash

```
git remote set-url origin git@github.com:用户名/仓库名.git
```

验证是否修改成功

bash

```
git remote -v
```

现在应该显示：

text

```
origin  git@github.com:用户名/仓库名.git (fetch)
origin  git@github.com:用户名/仓库名.git (push)
```

- 查看：`git remote -v`
- 添加：`git remote add <别名> <地址>`
- 删除：`git remote rm <别名>`
- 修改：`git remote set-url <别名> <新地址>`

##### 拉取远程操作中的内容

第一步：确认当前远程配置

bash

```
git remote -v
```

如果显示为空或者没有 `origin`，继续下一步。

第二步：添加远程仓库

你需要先知道你的 GitHub 仓库地址。在 GitHub 仓库页面点击绿色的 **Code** 按钮，复制 SSH 或 HTTPS 地址：

**SSH 方式**（你已配置 SSH key，推荐）：

bash

```
git remote add origin git@github.com:你的用户名/你的仓库名.git
```

**HTTPS 方式**：

bash

```
git remote add origin https://github.com/你的用户名/你的仓库名.git
```

第三步：验证是否添加成功

bash

```
git remote -v
```

应该看到类似输出：

text

```
origin  git@github.com:你的用户名/你的仓库名.git (fetch)
origin  git@github.com:你的用户名/你的仓库名.git (push)
```

第四步：再次拉取

bash

```
git pull origin main
```
