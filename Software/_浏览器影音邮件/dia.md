解决dia不能输入中文的方法

- 编辑/usr/share/applications/dia.desktop，把`Exec=dia %F`改为`Exec=env GTK_IM_MODULE=sim dia %F`