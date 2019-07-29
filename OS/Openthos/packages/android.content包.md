#### Intent

在不同的组件之间传递数据。显式Intent直接指定要启动的活动，隐式Intent使用action和category等信息指定要启动的Intent。每个Intent只能有一个action，但可以有多个category。

addCategory：添加一个category

setData：接收一个Uri对象

putExtra：向活动传递数据

getStringExtra：获取传递过来的字符串数据

getIntExtra：获取传递过来的整型数据

getBooleanExtra：获取传递过来的布尔型数据