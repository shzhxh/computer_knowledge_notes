#### 类Activity：活动

包含用户界面，主要用于和用户交互。

- onActivityResult：

- onCreate方法：创建活动，当活动开始的时候应调用此函数
  - findViewById：通过xml文件里的id属性找到view
  - requestWindowFeature方法：扩展窗口属性
  - setContentView方法：给当前活动加载布局
  - getIntent：获得启动当前活动的Intent
- onCreateOptionsMenu：在活动中创建菜单
  - getMenuInflater方法：返回一个MenuInflater对象
- onOptionsItemSelected：定义菜单响应事件
- setResult：用于向调用它的活动返回数据
- startActivity：开启一个新的活动
- startActivityForResult：启动活动，且在活动销毁时将数据返回给上一个活动