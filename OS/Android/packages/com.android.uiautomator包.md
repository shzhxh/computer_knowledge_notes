详见：[uiautomator](https://developer.android.google.cn/reference/android/support/test/uiautomator/package-summary)

### 接口

#### IAutomationSupport

为运行测例提供辅助支持

#### UiWatcher

### 类

#### By

是一个工具类，可以用简单的方法创建BySelector对象。

#### BySelector

#### Configurator

#### EventCondition

#### SearchCondition

#### UiAutomatorInstrumentationTestRunner

#### UiAutomatorTestCase

代表运行在设备上的UI Automator测试用例

- `public UiDevice getUiDevice() `

  获取当前UiDevice的实例。相当于在当前测例里运行`UiDevice.getInstance()`

#### UiCollection

#### UiDevice

提供对设备状态信息的访问，也可以用来模拟设备上的用户动作。

- `public boolean drag(int startX, int startY, int endX, int endY, int steps)`

  实现从一个坐标到另一个坐标的滑动。初始坐标为(startX, startY)，结束坐标为(endX, endY)。变量steps用来控制滑动速度，表示滑动操作需要执行的步数，一步用时5毫秒。返回值为真表明执行正确，为假表明执行错误或坐标无效。

- `public void wakeUp()`

  若屏幕是关闭的，此方法模拟的是按下电源键。若屏幕是点亮的，此方法什么都不做。

- `public boolean pressEnter()`

  模拟短按ENTER键。成功则返回true，否则返回false。

- `public boolean pressKeyCode(int keyCode)`

  模拟短按keyCode。成功则返回true，否则返回false。可以在类android.view.KeyEvent获取键码的详细信息。111代表esc键。

#### UiObject

是对视图(view)的一种表示。一旦创建了一个UiObject实例，它就可以被满足selector标准的多个视图重用。

- `public UiObject(UiSelector selector)`

  构造一个UiObject来表示一个视图，它要符合指定的selector标准。

- `public Rect getVisibleBounds()`

  返回视图的可见边框。如果视图只部分可见，则只报告可见部分的边框。

- `getChild`

- `click`

#### UiObject2

#### UiObject2Condition

#### UiSelecor

将布局层次中的元素指定给目标，通过属性来过滤，这些属性诸如文本值、内容描述、类名和状态信息。还可以根据元素在布局层次中的位置来定位它。

- `public UiSelector resourceId(String id)`

  设置查找规则来匹配给定的resource ID。变量id是要匹配的值，返回的是满足条件的UiSelecotr。

- `className`

- `isChecked`

- `text`

#### UiScrollable

继承自UiCollection，对滚动布局元素的条目提供了搜索支持。该类支持水平或垂直滚动的控制。

- `public UiScrollable(UiSelector container)`

- `getChildByText`

#### Until

#### Tracer

跟踪对UiAutomator API的调用，并输出到logcat或logfile。每个需要跟踪的UiAutomator的public方法都应该在一开始加上Tracer.trace。追踪默认是关闭的，需要显式地开启。

- `public static void trace(Object... arguments)`

  追踪UiAutomator里的方法。

### 枚举

#### Direction

### 异常

#### StaleObjectException

#### UiObjectNotFoundException