#### actions

- [Github Actions 入门教程——阮一峰](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)
- [actions的官方市场](https://github.com/marketplace?type=actions)
- [actions的官方仓库](https://github.com/actions)
- [workflow的语法](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [触发workflow的条件](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)

##### workflow文件

一个workflow就是一次CI的过程，它表现为一个yml文件。一个workflow包若干个job，即`jobs.<job_id>`。一个job包含若干个step，即`jobs.<job_id>.steps`。一个step包含若干个action，即`jobs.<job_id>.steps.run`。

> job是并行的基本单位，job之间是并行的，job内部是串行。一个workflow就是具有共同目标的job的集合。

- name - workflow的名称。如省略，默认为当前文件名。

- on - 触发workflow的条件

  ```
  on.schedule		# 指定工作流的运行时间(使用的cron语法)。
  ```

  

- jobs - 要执行的任务

  ```
  jobs.<job_id>	# 任务的名称
  jobs.<job_id>.if	# 用以指定job运行的条件
  jobs.<job_id>.name	# 任务的说明
  jobs.<job_id>.needs	# 指定运行顺序
  jobs.<job_id>.runs-on	# 指定虚拟机，是必填字段
  jobs.<job_id>.strategy	# 为任务创建matrix，使每个任务都可以有不同的变化。
  	# strategy.fail-fast默认为true，此时matrix里的job只要有一个出错，其它都会退出。若希望matrix里每个job都能执行完，应将其设为false。
  jobs.<job_id>.steps	# 指定任务的运行步骤
  ```
  
- steps - 组成任务的步骤

  ```
  steps[*].name	# 步骤名
  steps[*].run	# 该步骤的命令或action
  steps[*].env	# 该步骤所需的环境变量。
  steps[*].uses	# 指定一个要运行的action，它被当作是step的一部分
  	# action可以来自于当前仓库，一个公共仓库，或已发布的docker镜像
  	# 强烈建议使用action的时候指定版本号，以免action的作者更新后引发问题
  	# 某些action会需要with关键字所带来的参数
  	# action要么是JS文件，要么是docker容器。
  steps[*].with	# 指定action所需的参数
  
  ```

- run - 组成步骤的命令

##### 使用的限制

- Job执行时间：每个job的运行时间不得超过6小时。
- Workflow运行时间：每个workflow运行时间不得超过72小时。
- API的请求：一个仓库里每小时的API请求不得超过1000次。超出的API请求调用会失败，这可能会导致job失败。
- 并行的Job数量：Free用户并行的Job数量不得超过20个，超过的Job将进入队列中。
- Job matrix：一个matrix生成的job数不得超过256个。
- Workflow运行队列：每个仓库10秒内的的wrokflow队列不得超过500个

##### 编写action

- Dockerfile : 用于创建Docker容器action
- action.yml：action的配置
  - `input:<args>`定义接收的参数，其中`required`为true则必须传递此参数。
  - `runs:`定义运行环境，比如docker。
- entrypoint.sh：执行脚本。

##### 例子：代码同步

1. `ssh-keygen`生成一个密钥对，私钥放github的`仓库设置->Secrets`下，公钥放gitee的`个人设置->SSH公钥`下。
2. gitee的个人设置下新建一个私人令牌，保存到github仓库设置的`secrets`下。
3. 在github仓库里`.github/workflow/`目录下新建一个yml文件，使用第三方action实现推送。

##### 例子：缓存文件

- 输入：

  `path` - 要缓存的文件和目录

  `key` - 为保存文件而设置的键

  `restore-keys` - 键的列表，如果缓存没有命中用来恢复缓存

- 输出：

  `cache-hit` - 一个布尔值，表示是否为键找到了确切的匹配。

#### 免密登陆

[使用ssh免密登陆](https://help.github.com/en/articles/connecting-to-github-with-ssh)

```
ls -la ~/.ssh		# 查看本地是否已创建密钥
	# 我的目录里存在id_rsa.pub，表明本地已创建密钥

# 若已创建密钥，则将公钥复制到剪切板
sudo apt install xclip
xclip -sel clip < ~/.ssh/id_rsa.pub

# 登陆github，点右上角头像，选择"Settings"
# 点击“SSH and GPG keys”
# 点击"New SSH key","Title"任取，"Key"直接复制，然后点击"Add SSH key"

# 测试ssh连接
ssh -T git@github.com

# 将原仓库的https协议改为ssh
vim .git/config		# 修改url，将https://改为git@，将github.com/改为github.com:
```

#### 创建wiki
1. 在github自己项目的Settings页面，选择"Wikis"开通wiki，选择"Restrict editing to collaborators only"禁止别人编辑。
2. 在相应的wiki页面创建第一个page，这样才能将wiki clone到本地。
3. 在本地使用 `git clone https://github.com/shzhxh/ucore_os_lab.wiki.git myWiki` 命令将选程wiki克隆到本地的myWiki目录。
4. 为了在本地成功安装gollum，应先用`sudo apt install`命令安装ruby-dev和rubygems-integration,否则会出现安装错误。应使用`gem sources -a`命令添加国内源，用`gem sources --remove`删除官方源，否则会因安装太慢而导致安装失败。
5. 使用`sudo gem install`命令安装rails和gollum。
6. 在myWiki目录使用命令gollum，在浏览器中输入网址：localhost:4567，即可编辑wiki.
7. 使用`git push`命令将wiki上传到github.
