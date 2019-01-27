#### firefox支持caj

##### 来源

[CNKI PDF全文下载用户脚本](http://blog.yuelong.info/post/cnki-pdf-js.html)

##### 步骤

1. 安装[Greasemonkey](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/)
2. 安装[普通版脚本](https://greasyfork.org/zh-CN/scripts/18841-cnki-%E4%B8%AD%E5%9B%BD%E7%9F%A5%E7%BD%91-pdf-%E5%85%A8%E6%96%87%E4%B8%8B%E8%BD%BD)
3. 刷新知网页面

#### 控制notifications

notifications是某些网站弹出来的提示消息，有的时候还是挺有用的，比如打开了微信网页版。

##### 禁止所有网页弹出notifications

1. 地址栏输入`about:config`
2. 找到`dom.webnotications.enabled`和`dom.webnotications.serviceworker.enabled`，将它们的`value`修改为`false`

##### 精细控制notifications

1. 地址栏输入`about:perferences#privacy`
2. 找到`Permissions`下的`Notifications`，设置之。

