### git

1.git init    	//初始化git

2.git clone xxx 	//克隆，xxx代表远程仓库的http或者是ssh

3.git status	//查看当前仓库的状态告诉你有哪些文件发生了变化

4.git add -A	//将修改添加后的文件上传到远程仓库区

5.git commit -m'  '	//’ ‘中写的是修改后自己给起的名称

6.git push origin main	//push是将本地仓库的内容上传到远程仓库中去

7.git pull origin main	//pull是将远程仓库的东西下拉到自己的本地仓库中

###### http与ssh

如果出现两个文件上传时用的不同，那么要将http或ssh改成其中的一个，这样才能方便后续上传，使用的命令行是：

1.git remote -v

2.git remote remove origin

3.git add origin xxx	//这个xxx指的是http的地址或者ssh

等到两个文件的配置一样时，在进行上述git命令

