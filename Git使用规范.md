# Git使用规范

## 目的

- 统一团队的Git工作流
- 统一团队Git Commit日志标准，便于后续提交日志查看及索引

## git工作流

- git分支规范：

  - `dev`为日常开发分支
  - `master`为主分支，属保护分支，不能直接在此进行代码修改和提交
  - `feature`新功能开发分支，当完成一个功能并测试通过后进行合并到dev分支中
  - `fix`线上紧急漏洞修复分支，从master分支拉取创建，修复完bug后合并到dev和master分支（需在dev完成测试后才能合并master分支）

  * `feature`和`fix`分支可在使用后删除，具体视个人情况而定

## git提交规范

> 每次提交必须按照规范，写明本次提交的类型，并对修改内容做出明确描述

- 提交格式
  - 提交类型：1.提交描述  2.提交描述2
- 提交类型
  - `fix`:问题的修复
  - `feature`:有新功能的增加
  - `update`:现有功能模块的代码改动
  - `improve`:优化某个模块
  - `debug`:提交某个功能模块的调试代码

```
示例：
(单种修改类型) feature: 1.新增xxx功能  2.新增yyy功能
(多种修改类型) feature + update: 1.新增xxx功能  2.更改xx模块样式
```



## git常用操作

### 1.将本地项目添加到远程仓库

```
在本地项目中执行以下命令
git init  (初始化git项目，已经初始化过的无需执行)
git remote add origin 远程仓库地址
git pull origin master
git add . （添加项目所有文件）
git commit -m '项目初始化'
git push -u origin master
```

### 2.暂时贮藏改动代码

场景：当前分支修改了部分文件但未修改完成，临时有紧急情况，需要在当前分支进行其他模块修改并提交。此时，可以使用贮藏功能将改动的文件存储到临时空间，将本分支还原到修改之前。

```
1.git stash save "当前存储的变动描述" : 执行存储时，添加备注 (执行存储后，会把改动文件从当前分支中还原)

2.git stash list : 查看stash列表

3.git stash apply : 应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash apply stash@{$num} ， 比如第二个：git stash apply stash@{1} 

4.git stash pop : 命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下,默认为第一个stash,即stash@{0}，如果要应用并删除其他stash，命令：git stash pop stash@{$num} ，比如应用并删除第二个：git stash pop stash@{1}

5.git stash drop stash@{$num} : 丢弃stash@{$num}存储，从列表中删除这个存储

6.git stash clear : 删除所有缓存的stash
```

### 3.版本回滚

```
1.回退到当前版本（放弃所有修改）
git reset --hard

2.撤销某个文件的修改
git checkout demo.js

3.回退到某一版本但保存自该版本起的修改
git reset 版本号

4.回退到和远程版本一样
有时候，当发生错误修改需要放弃全部修改时，可以以远程分支作为回退点退回到与远程分支一样的位置，执行的命令如下
git reset --hard origin/master // origin代表你远程仓库的名字，master代表分支名

5.回退远程仓库的版本
先在本地切换到远程仓库要回退的分支对应的本地分支，然后本地回退至你需要的版本，然后执行：
git push <仓库名> <分支名> -f 
```

### 4.解决代码冲突

合并代码后，因多人修改同一模块，可能会产生代码冲突，此时需要手动解决。

```
示例：以下是冲突代码提示
<<<<<< HEAD
const demo = 111; 
======
const demo = 222;
>>>>>> 865sdf7612b3u23h4234

说明：
<<<<<< HEAD
此区间的代码指，当前分支下你所改动的代码
======
此区间代表你合并代码后，他人改动的代码
>>>>>> 865sdf7612b3u23h4234

需要对比两个区间的代码，根据实际情况修改代码，修改完成后，删除所有冲突标记。
然后执行以下操作：
git add .
git commit -m "fix：解决代码冲突"
git push origin 分支名`
```

