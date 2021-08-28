sudo apt install mongodb	//安装mongodb
mongo -version		//查看版本号
pgrep mongo -l		//查看mongo是否启动成功
service mongodb start 	//启动mongodb
service mongodb stop	//关闭mongodb
mongo			//进入命令模式，前提是启动了mongodb
>show dbs		//显示数据库列表(其中local数据库是存索引的）
>show collections 	//显示当前数据库中的集合
>show users		//显示所有用户
>use yourDB		//切换至数据库yourDB
>db.help()		//显示数据库操作命令
>db.yourCollection.help()	//显示集合操作指令，yourCollection是集合名
