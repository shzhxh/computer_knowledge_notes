* 递归删除目录XXX
  ` rm -rf XXX`

* 反向删除当前目录下除a,b外的所有目录和文件

  `ls | grep -v a | grep -v b | xargs rm -rf`
