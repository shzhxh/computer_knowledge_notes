- listf : 列出文件夹里的所有文件

  参数1	: 文件夹

  参数2	: 文件类型

  - listf_cc：列出以c和S结尾的文件，即c编译器所需的文件

- toobj：将文件后缀改为.o，且加上路径前缀obj/
  参数1: 文件名
  参数2: 包名(即obj目录下的子目录名)，可选

- todep: 基于toobj，只是将输出的文件名改为.d
  参数1: 文件名
  参数2: 包名(即obj目录下的子目录名)，可选

- totarget : 给要生成的目标文件加上路径`bin/`

  参数1：要生成的目标文件

- packetname：给一个名称加上前缀\_\_objs_
  参数1: 名称

- cc_template: c编译器模板，用于生成编译规则
  参数1: 要编译的文件
  参数2: 编译器
  参数3: 编译选项
  参数4: 源文件所在的子目录

- add_files: 把文件加到包里(同do_add_files_to_packet)
  参数1: 文件
  参数2: c编译器
  参数3: 编译选项，可选
  参数4: 包，可选
  参数5: 文件所在的子文件夹，可选
  - add_files_cc: 把文件加到包里，只是指定了编译器和编译选项
    参数1:文件
    参数2: 包，可选
    参数3: 附加的编译选项，可选
    参数4: 文件所在的子文件夹，可选

- add_files_cc -> add_files -> do_add_files_to_packet -> packetname, toobj, cc_template

- create_target_cc -> create_target -> do_create_target -> totarget , packetname

- cgtype：

