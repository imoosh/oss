## 安装配置Git

### 安装Git

Linux平台安装git

```
# ubuntu
$ sudo apt-get install git 

# centos
$ sudo yum install -y git
```

MacOS平台安装git

```
$ brew install git
```

### git config -- 配置Git

```
$ git config --global user.name "Name"
$ git config --global user.email "email@example.com"
```

### git init -- 创建版本库

说明: 创建一个git仓库，创建之后会在当前目录生成一个.git的目录。

```shell
$ git init
Initialized empty Git repository in /Users/wayne/Documents/learngit/.git/
```

## 修改管理

### git add -- 添加修改到缓冲区

```shell
# 添加文件到缓冲区
$ git add test.txt

# 添加所有文件到缓冲区
$ git add .
$ git add --all
```

### git rm -- 删除缓冲区修改

把修改从缓冲区中删除。

```shell
$ touch a.txt
$ git add a.txt
$ git commit -m "add a.txt"
[master bfc7475] add a.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt

# 先删除，再提交
$ git rm a.txt
$ git commit -m "delete a.txt"
[master c12c586] delete a.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 a.txt
```

使用文件管理器或者`rm`命令删除文件，然后通过`git status`可以查看到删除了哪些文件，然后再通过`git rm(add亦可)` + `go commit`完成修改。

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

如果是手动误删，而且已经提交过版本库，可以从版本库中恢复。

```shell
# 从版本库中恢复误删的文件
$ git checkout -- a.txt
$ git status
On branch master
nothing to commit, working tree clean
```

**特别注意**

* 从未提交到版本库中的文件被删除，无法恢复；
* 误删的文件，只能通过版本库恢复到最新版本，会丢失最近一次提交后的修改内容；



### git commit -- 提交修改到仓库

提交缓冲区的所有修改到本地仓库。**如果修改了文件没有add到缓冲区，不会被提交的。**

```shell
$ git commit -m "add 1.txt"
[master (root-commit) fe7a3bb] add 1.txt
 1 file changed, 1 insertion(+)
 create mode 100644 1.txt
```

所以每次修改完到最后提交仓库有两种方法：

- **修改1 -> git add -> 修改2 -> git add -> git commit**

- **修改1 -> git add -> git commit -> 修改2 -> git add -> git commit**

###git status -- 查看状态

说明: 查看修改的状态。

* 新文件未添加，查看状态

  ```shell
  waynes-MacBook-Air:learngit wayne$ echo abc > 1.txt
  waynes-MacBook-Air:learngit wayne$ git status
  On branch master
  
  No commits yet
  
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
  
  	1.txt
  
  nothing added to commit but untracked files present (use "git add" to track)
  ```

* 添加新文件，查看状态

  ```shell
  waynes-MacBook-Air:learngit wayne$ git add .
  waynes-MacBook-Air:learngit wayne$ git status
  On branch master
  
  No commits yet
  
  Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
  
  	new file:   1.txt
  
  ```

* 提交文件，查看状态

  ```shell
  waynes-MacBook-Air:learngit wayne$ git commit -m "add 1.txt"
  [master (root-commit) 68dd499] add 1.txt
   1 file changed, 1 insertion(+)
   create mode 100644 1.txt
   
  waynes-MacBook-Air:learngit wayne$ git status
  On branch master
  nothing to commit, working tree clean
  ```

* 修改文件，查看状态

  ```shell
  waynes-MacBook-Air:learngit wayne$ echo 123 >> 1.txt 
  waynes-MacBook-Air:learngit wayne$ git status
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
  
  	modified:   1.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
  ```

* 再次提交，查看状态

  ```shell
  waynes-MacBook-Air:learngit wayne$ git add .
  waynes-MacBook-Air:learngit wayne$ git commit -m "modify 1.txt"
  [master 83bbd3b] modify 1.txt
   1 file changed, 1 insertion(+)
  waynes-MacBook-Air:learngit wayne$ git status
  On branch master
  nothing to commit, working tree clean
  ```

### git reset -- 回退版本

### git checkout -- 撤销修改

| 工作区 | 缓冲区 | 本地仓库 | 远程仓库 |
| :----: | :----: | :------: | :------: |
|  <—>   |  <—>   |   <—>    |   <—>    |

* 场景一：丢弃工作区的修改，使用命令`git checkout -- file`；
* 场景二：丢弃缓冲区的修改，使用命令`git reset HEAD file`，回到场景一；
* 场景三：丢弃本地库的修改，使用命令`git reset --hard version_id`；回退到场景二；

### git log -- 查看日志

```shell
$ git log
```

## 远程仓库

### 添加远程仓库

```shell
$ git remote add origin http://192.168.1.205:8080/wayne/notes.git
```

添加后，远程库的别名就是`origin`，这是git默认叫法，也可以改成其他的。

### 推送本地库

```shell
$ git push -u origin master
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，第一次推送`master`分支时，加上`-u`的作用：

- 将本地的`master`分支推送到远程的`master`分支；
- 将本地的`master`分支与远程的`master`分支关联起来，后面推送可以不带`-u`选项；

### 克隆远程库

要克隆一个远程库，必须知道远程库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

```shell
$ git clone http://192.168.1.205:8080/wayne/notes.git
```

---

### 参考`https://www.liaoxuefeng.com/wiki/896043488029600`

## 

## GIT命令行中文乱码

```shell
➜  notes git:(master) ✗ git status            
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   "\345\267\245\344\275\234/Linux/\347\275\221\347\253\231\351\203\250\347\275\262/\346\220\255\345\273\272ngrok\346\234\215\345\212\241.md"
```

解决方法：

```shell
git config --global core.quotepath false
```

