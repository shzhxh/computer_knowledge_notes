[Home](index)->Create wiki on github
---
1. 在github自己项目的Settings页面，选择"Wikis"开通wiki，选择"Restrict editing to collaborators only"禁止别人编辑。
2. 在相应的wiki页面创建第一个page，这样才能将wiki clone到本地。
3. 在本地使用 `git clone https://github.com/shzhxh/ucore_os_lab.wiki.git myWiki` 命令将选程wiki克隆到本地的myWiki目录。
4. 为了在本地成功安装gollum，应先用`sudo apt install`命令安装ruby-dev和rubygems-integration,否则会出现安装错误。应使用`gem sources -a`命令添加国内源，用`gem sources --remove`删除官方源，否则会因安装太慢而导致安装失败。
5. 使用`sudo gem install`命令安装rails和gollum。
6. 在myWiki目录使用命令gollum，在浏览器中输入网址：localhost:4567，即可编辑wiki.
7. 使用`git push`命令将wiki上传到github.
