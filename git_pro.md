# Pro Git

## 1 配置

### 1.1 用户信息

* `git config --system`全局配置`/etc/gitconfig`

* `git config --global`用户配置`~/.gitconfig`
* `.git/config`

### 1.2 文本编辑器

```shell
git config --global core.editor emacs
```

### 1.3 差异分析工具

```shell
git config --global merge.tool vimdiff
```

可识别kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，和opendiff 等合併工具的輸出信息。

### 1.4 查看配置信息

```shell
git config --list
```

只看某个配置

```shell
git config user.name //example
```

## 2. Git基础

### 2.1 在工作目录中初始化新仓库

```shell
git init
```

### 2.2 从现有仓库克隆

```shell
git clone xxxxxxxxx

// 指定目录名
git clone xxxxxxxxx mygit
```

### 2.3 查看已暂存和为暂存的更新

查看尚未暂存的文件更新了哪些部分
```shell
git diff
```

查看已暂存和上次提交的差异
```shell
git diff --cached

or

git diff --staged

```

### 2.4 commit

```shell
git commit -v // -v可将具体改变注释到commit中
```

```shell
git commit -a // -a可跳过add操作直接全部commit
```

### 2.5 git rm

-f 选项强制删除暂存区文件

--cached 移除git管理但是保留在工作区

### 2.6 查看提交历史

```shell
git log
```

-p 展开每次提交的内容差异，用 -2 显示最近两次的更新, 用--word-使用单词层面对比替换行层面对比，
使用 -U1将上下文默认行数3改为1

--stat 显示增改行数统计

--pretty= 指定使用完全不同于默认的方式展示提交历史，如 --pretty=online

--pertty=format 自定义输出格式

git log --pretty=format:"%h - %an, %ar : %s"
选项	说明
%H	提交对象（commit）的完整哈希字串
%h	提交对象的简短哈希字串
%T	树对象（tree）的完整哈希字串
%t	树对象的简短哈希字串
%P	父对象（parent）的完整哈希字串
%p	父对象的简短哈希字串
%an	作者（author）的名字
%ae	作者的电子邮件地址
%ad	作者修订日期（可以用 -date= 选项定制格式）
%ar	作者修订日期，按多久以前的方式显示
%cn	提交者(committer)的名字
%ce	提交者的电子邮件地址
%cd	提交日期
%cr	提交日期，按多久以前的方式显示
%s	提交说明

限定输出长度

```shell
git log -n <n=1,2,3,4,...,n>
```

### 2.7 撤销操作

修改最后一次提交

```shell
git commit --amend
```

取消暂存区文件

git reset HEAD <file>...

取消文件的修改

git checkout -- <file>...

### 2.8 远程操作

```shell
git remote // 列出每个远程仓库的简短名字，一般默认为origin

-v 显示相对应的克隆地址
```

添加一个新的远程仓库，指定一个名字以便引用
```shell
git remote add [shortname] [url]

git remote add pb git://github.com/paulboone/ticgit.

git fetch pb // remote后 fetch获取主干更新，pull 拉取更新

git remote show [remote-name]获取远程仓库的详细信息
```

远程仓库的删除和重命名
```shell
git remote rename origin paul // origin修改为paul 
```

```shell
git remote rm paul // 移除对应远端仓库
```

### 2.9 打标签

#### 列出已有标签
```shell
git tag // -l "*.xx" 只查看匹配的标签
```

含附注的标签,使用 -a(annotated)
```shell
git tag -a v1.4 -m "my version 1.4"

git show v1.4 // 查看标签信息
```

签署标签，将-a改为-s(signed)

普通标签
```shell
git tag v1.4
```

验证标签
```shell
git tag -v [tag-ame] v(verify) // 需要GPG验证签名
```

往后加注标签
```shell
git tag -a v1.2 [commit id]
```

分享标签
```shell
git push origin [tagname] // 推送某一个tag
git push origin --tags // 推送所有tag
```

配置别名
```shell
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --'
```

## 3 分支

### 3.1 分支创建与合并

创建一个分支
```shell
git branch testing
```

切换一个分支
```shell
git checkout testing
```

**HEAD是一个特殊指针，指向当前工作分支**

创建分支并且切换
```shell
git checkout -b testing
```

### 3.2 分支管理
```shell
git branch -v // 查看各个分支最后一个提交对象信息
```

```shell
git branch --merged // 显示已merge到当前分支的分支
git branch --no-merged // 显示没有merge到当前分支的分支
```

分支建议
master --->
	|----> develop ---->
				|------> topic

### 3.3 远程分支

远程分支标识(远程仓库名)/(分支名)
```shell
git push (远程仓库名) (分支名)
```

```shell
git fetch origin //  获取服务器上最新数据，但不同步至工作目录，并且在本地以 origin/master形式列出一个指针

git checkout -b [local-brname] origin/[remote-brname] // 与服务器分支同步并拉取到本地新建分支

git checkout --track origin/[remote-brname] 上述命令简化版本并且本地分支名与远程仓库一致
```

提交本地分支
```shell
git push [远程名] [本地分支]:[远程分支]
```

**删除远程分支**

无厘头语法：
比如删除远程分支 serverfix
```shell
git push [远程名] :[分支名]

git push origin :serverfix
```

### 3.4 分支衍合
把一个分支中的修改整合到另一个分支两种方式：merge和rebase

rebase 将其他分支修改从两个共同祖先开始，衍合至一个新提交。
一般我們使用衍合的目的，是想要得到一個能在遠程分支上乾淨應用的補丁—比如某些項目你不是維護者，
但想幫點忙的話，最好用衍合：先在自己的一個分支裡進行開發，當準備向主項目提交補丁的時候，
根據最新的 origin/master 進行一次衍合操作然後再提交，這樣維護者就不需要做任何整合工作
（譯註：實際上是把解決分支補丁同最新主幹代碼之間衝突的責任，化轉為由提交補丁的人來解決。），
只需根據你提供的倉庫地址作一次快進合併，或者直接採納你提交的補丁。

```shell
git rebase branch-name // 衍合banrch-name分支到当前分支
```

```shell
git rebase master branch-name // 取出banrch-name分支然后在主分支master上重演
```

```shell
master --->
	|----> server ---->
				|------> client

git rebase --onto master server client // 在master上跳过server分支修改衍合client到master
```

***一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行衍合操作***

## 4 服务器Git
git clone --bare [url] [.git =alias name] //只克隆.git目录


## 5 分布式开发
提交规范
```shell
本次更新的简要描述（50 个字符以内）

如果必要，此处展开详尽阐述。段落宽度限定在 72 个字符以内。
某些情况下，第一行的简要描述将用作邮件标题，其余部分作为邮件正文。
其间的空行是必要的，以区分两者（当然没有正文另当别论）。
如果并在一起，rebase 这样的工具就可能会迷惑。

另起空行后，再进一步补充其他说明。

 - 可以使用这样的条目列举式。

 - 一般以单个空格紧跟短划线或者星号作为每项条目的起始符。每个条目间用一空行隔开。
   不过这里按自己项目的约定，可以略作变化。
```
