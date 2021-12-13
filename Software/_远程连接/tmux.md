#### tmux的原理

tmux使用c/s结构，主要包括如下模块：

- server：输入tmux命令就开启了一个server
- session会话：一个server可包含多个session
- window：一个session可以包含多个window
- pane面板：一个window可以包含多个pane

#### 语法

```
tmux [options] [command [flags]]
```



#### 命令

##### 客户端与会话

```bash
attach, attach-session [-dEr] [-c working-dir] [-t target-session]
	# 如在tmux外部，则在当前终端创建一个新的client并把它附加到target-session。如在tmux内部，则切换到当前client。

ls, list-sessions [-F format]
	# 列出server管理着的所有会话
	
new-session [options] [shell-cmd]	# 创建一个新的会话。
	-d	# 新会话不附加到当前终端。(默认是附加到当前终端的)
	
rename, rename-session [-t target-session] new-name
	# 给会话重命名
```



##### 窗口与面板

```bash
renamew, rename-window [-t target-vindow] new-name
	# 给窗口重命名
```



##### 键绑定

#### 选项

可以通过变量的值来控制tmux的外观和行为。有三类选项：server选项、session选项和window选项。

```
set, set-option [-aFgoqsuw] [-t target-session | target-window] <option> <value>
	mouse [on | off]	# 如设置为on则允许鼠标事件。详见#鼠标支持。
set-window-option [-aFgoqu] [-t target-window] <option> <value>
show-options [-gqsvw] [-t target-session | target-window] [option]
show-window-options [-gv] [-t target-window] [option]
```



#### 激活控制台

```
ctrl + b
```

#### 系统操作

```
d		# 脱离当前会话(通过tmux attach可重新进入脱离的会话)
```



#### 窗口操作

```
c		# 创建新窗口
&		# 关闭当前窗口
数字键		# 切换到指定窗口
l		# 切换到上一个窗口
f		# 依据窗口编号向前切换
n		# 住所窗口编号向后切换
w		# 通过窗口列表切换窗口
```



#### 面板操作

```
"		# 将面板分为上下两块
%		# 将面板分为左右两块
空格		# 对所有的面板重新布局
z		# 最大化当前面板
x		# 关闭当前面板
！
o		# 切换面板
方向键		# 切换面板
翻页键		# 翻页模式
```

#### 命令模式

```
:	# 进入命令模式
```

#### 配置

```
# 配置文件在~/.tmux.conf

```

