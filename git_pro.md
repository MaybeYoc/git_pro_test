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
