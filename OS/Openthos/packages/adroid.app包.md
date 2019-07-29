#### 类Activity：活动

包含用户界面，主要用于和用户交互。活动有四种状态：运行状态(处于返回栈的栈顶)、暂停状态(不在栈顶但仍然可见)、停止状态(不在栈顶且不可见)、销毁状态(从返回栈中移除)。

- onActivityResult：被创建活动回调此函数返回数据
- **onCreate**方法：创建活动，当活动开始的时候应调用此函数。应在此方法中完成活动的初始化。
  - findViewById：通过xml文件里的id属性找到view
  - requestWindowFeature方法：扩展窗口属性
  - setContentView方法：给当前活动加载布局
  - getIntent：获得启动当前活动的Intent
- onBackPressed：当用户按下Back键的时候调用此函数
- onCreateOptionsMenu：在活动中创建菜单
  - getMenuInflater方法：返回一个MenuInflater对象
- **onDestroy**：销毁活动。应在此方法中完成释放内存的操作。
- onOptionsItemSelected：定义菜单响应事件
- **onPause**：系统准备启动或恢复另一个活动时调用。
- **onRestart**：使活动由停止状态变为运行状态。
- **onResume**：活动准备好和用户交互时调用。
- **onStart**：使活动由不可见变为可见。
- **onStop**：活动完全不可见时调用。
- setResult：用于向调用它的活动返回数据
- startActivity：开启一个新的活动
- startActivityForResult：启动活动，且在活动销毁时将数据返回给上一个活动