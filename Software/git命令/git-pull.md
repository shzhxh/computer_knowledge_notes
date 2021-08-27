#### 简介

将远程仓库的更改合并到当前分支中

#### 语法

```
git pull [options] [repository [refspec...]]
	# <refspec>...的格式为[+]<src>:<dst>。但和git push相反，在git pull里，<src>指的是远程分支，<dst>指的是本地分支。
```

#### 选项

##### 与合并相关

```
-r, --rebase[=false|true|preserve|interactive]	# 如为true，则在抓取之后将当前分支重置到上游分支之上。如为false，则将当前分支合并到上游分支。如为preserve，则会向git rebase传递--preserve-merges选项，这样本地提交就不会被破坏掉。如为interactive，则开启交互模式。
```



##### 与抓取相关

```
--all	# 抓取所有
```



#### 用法

```
# 如果clone的时候使用--depth选项进行浅层复制，则无法跳转到其它分支，也无法回退到以前的某个提交，此时应执行如下指令
git pull --unshallow	# 取消浅层复制时的所有限制，使本地仓库成为远程仓库的一个完全的复本
```

#### 错误与问题

1. fatal: refusing to merge unrelated histories

   解决方法：当我执行`git pull`时出现此问题，解决方法是`git pull origin main:main`。

   原因分析：网上说这是把两个没有关系的项目合并到一起产生的错误。可我只有一个远程仓库origin，只有一个分支main，并不存在多个没有关系的项目。且执行`git branch -vv`可见本地和远程的分支是有关联的。所以，还不知道真正出问题的原因。
