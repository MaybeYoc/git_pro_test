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

