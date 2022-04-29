#### 简介

显示ELF文件的信息(参考：man readelf)

#### 用法

```
readelf [options] <elffiles>
```



#### 选项

```shell
-a, -all  # 显示所有信息
-d, --dynamic	# 显示动态节的内容(dynamic section)
-h, --file-header  //显示elf文件头(file header)
-l, --program-headers, --segment  //显示程序头(program headers)
-S, --sections, --section-headers  //显示节头(section headers)
```

