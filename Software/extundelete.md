#### 示例

```
# 1 目标分区不能挂载，或设置为只读分区
# 2 获取删除时间
date -d "yyyy-mm-dd hh:mm" +%s
# 3 恢复指定时间后被删除的文件
extundelete /dev/sdaX --after <time> --restore-all
```

