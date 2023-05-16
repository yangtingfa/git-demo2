

# **git**

#### **配置：**

1. *name: git config  - -global user.name  “yangtingfa”*
2. *email: git config  - -global user.email  “2861995205@qq.com”*

#### **使用git：**

1. ​	git  status   —–查看当前仓库状态

2. ​    git  init        —–初始化git仓库

3. 刚刚添加到项目中的文件处于未跟踪的状态

   未跟踪—-》暂存                git   add    . \文件名 	或者   git    add .

   暂存——-》未修改            git   commit  -m  “备注内容”   或者              git commit -a  -m “xxxxx”   提交所有未修改文件

   未修改—-》修改                

#### **常用命令**

​	1.重置文件

```bash
git restore ".\1.txt"   #恢复文件
git restore --staged 'filename'  #取消暂存状态
```

   2.删除文件

``` bash
git rm  "filename"
git rm   "filename"  -f   强制删除
```

3.移动文件

```bash
git mv from to  #移动文件
```

4.查看日志

```bash
git -log
```

### **分支**

git在存储文件的时候，每一次提交都会产生一个节点，节点形成一个树状结构，默认情况下只有一个分支master，只后可以创建分支，分支之间相互独立，在其他分支上修改代码不会影响其他的分支，

```bash
git branch  #查看分支
git branch "分支名"   #创建分支
git branch -d "分支名"  #删除分支
git switch "分支名"    #切换分支
git switch -c "分支名"   #创建并切换分支
git checkout -b "分支名"   #创建并切换分支
```

在真实开发中，我们都是在自己的分支上修改编写代码，完成后将自己的分支代码合并到主分支上去,

快速合并：同一条分支情况下首先切换到主分支上，执行git merge bug1 之后git branch -d bug1删除分支，之后合并完毕

###  变基

```bash
git rebase 需要变基到哪个分支上
```

在开发中，还可以通过变基来完成分支的合并。之前从哪个节点分支出去变成直接连接到最新的节点。

①.当发起变基时，git会首先找到两条分支的共同祖先

②.对比现在的分支内容和之前祖先的历史的不同，将他们的不同提出到一个临时文件中

③.当基底换成最新的修改节点

④.以当前基底开始，重新执行历史操作。

相比于merge，最终合并的结果是一样的，但是变基会让提交记录更加的简洁，如果分支已经提交给了远程仓库，尽量不要使用变基。

### 远程仓库

远程仓库可以被多人同时访问使用，方便协同操作。目前两个公共库：Git-Hub和Gitee

将本地库上传到github

```bash
git remote add origin https://github.com/yangtingfa/git-demo.git
     #git remote add <remote name> <url>
     
git branch -M main
	#修改分支名为main
	
git push -u origin main
	#git push 将代码上传到服务器上
```

将本地库上传到gitee

```bash
git remote add gitee https://gitee.com/loginyang/git-demo.git
git push -u gitee "main"
```

