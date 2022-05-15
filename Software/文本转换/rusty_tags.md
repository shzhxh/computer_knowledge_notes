```bash
cargo install rusty_tags	# 安装rusty_tags
rusty_tgas vi		# 生成rusty_tags.vi
```

支持标准库

```shell
# 定义环境变量RUST_SRC_PATH则可支持标准库
vim ~/.bashrc # 添加export RUST_SRC_PATH=$(rustc --print sysroot)/lib/rustlib/src/rust/library/
```

配置文件在`~/.rusty-tags/config.toml`

配置vim，在`~/.vimrc`里添加如下内容：

```
autocmd BufRead *.rs :setlocal tags=./rusty-tags.vi;/,$RUST_SRC_PATH/rusty-tags.vi
autocmd BufWritePost *.rs :silent! exec "!rusty-tags vi --quiet --start-dir=" . expand('%:p:h') . "&" | redraw!
```

