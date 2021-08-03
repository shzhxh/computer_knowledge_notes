#### actions

- [Github Actions 入门教程——阮一峰](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)
- [actions的官方市场](https://github.com/marketplace?type=actions)
- [actions的官方仓库](https://github.com/actions)
- [workflow的语法](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [触发workflow的条件](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)

##### workflow文件

一个workflow就是一次CI的过程，它表现为一个yml文件。一个workflow包若干个job，即`jobs.<job_id>`。一个job包含若干个step，即`jobs.<job_id>.steps`。一个step包含若干个action，即`jobs.<job_id>.steps.run`。

- name - workflow的名称。如省略，默认为当前文件名。

- on - 触发workflow的条件

- jobs - 要执行的任务

  ```
  jobs.<job_id>	# 任务的名称
  jobs.<job_id>.name	# 任务的说明
  jobs.<job_id>.needs	# 指定运行顺序
  jobs.<job_id>.runs-on	# 指定虚拟机，是必填字段
  jobs.<job_id>.steps	# 指定任务的运行步骤
  ```
  
- steps - 组成任务的步骤

  ```
  steps[*].name	# 步骤名
  steps[*].run	# 该步骤的命令或action
  steps[*].env	# 该步骤所需的环境变量。
  steps[*].uses	# 
  ```

- run - 组成步骤的命令

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
