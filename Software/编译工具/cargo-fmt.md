```
cargo fmt [flags] [options] [-- <rustfmt_options>...]
	# 一个实用工具，使用rustfmt来格式化当前crate的所有bin和lib文件。
	
	## flags
	--all		# 格式化所有的包，包含它们的本地的基于路径的依赖。
	--check
	-h, --help
	-q, --quiet
	-v, --verbose
	--version
	
	## options
	--manifest-path <manifest-path>
	--message-format <message-format>
	-p, --package <package>...
```

