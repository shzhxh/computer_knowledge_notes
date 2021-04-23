#### 简介

将远程仓库的更改合并到当前分支中

#### 语法

```
git pull [options] [repository [refsepc...]]
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

