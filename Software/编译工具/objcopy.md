#### 简介

复制和转换object文件

#### 语法

```
objcopy [选项] infile outfile
```

#### 选项

```
-S, --strip-all	# 不从源文件复制重定位和符号信息
-O bfdname, --output-target=bfdname	# 使用格式bfdname写入到输出文件
	# 所有可能的bfdname，详见"objcopy --info"
--change-addresses incr	# 通过增加incr改变所有section的VMA和LMA地址
--info			# 显示所有支持的架构和object格式
```

#### 示例

```bash
# elf转bin
objcopy -O binary <elf_file> <bin_file>
```

