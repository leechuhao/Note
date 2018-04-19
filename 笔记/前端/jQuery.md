jQuery是一个javascript函数库

特性：

- HTML元素选取
- HTML元素操作
- CSS操作
- HTML事件函数
- javascript特效和动画
- HTML DOM遍历和修改
- AJAX
- utilities

使用方法：

1. 下载到本地，有两个版本的 jQuery 可供下载：

   - Production version - 用于实际的网站中，已被精简和压缩。
   - Development version - 用于测试和开发（未压缩，是可读的代码）

   这两个版本都可以从 [jQuery.com](http://jquery.com/download/) 下载。

2. 通过CDN（内容分发网络）引用，谷歌和微软的服务器都存有 jQuery 。引用方法：[link](http://www.w3school.com.cn/jquery/jquery_install.asp)

所有 jQuery 函数位于一个 document ready （文档就绪）函数，这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。

### jQuery HTML

对DOM节点的操作：

- 获取信息
- 设置
- 添加
- 删除
- 获取并设置CSS
- jquery的css()方法

### jQuery 遍历

对某个元素的祖先、后代、通报元素进行选择

### jQuery AJAX

AJAX 是与服务器交换数据的艺术，它在不重载全部页面的情况下，实现了对部分网页的更新。

- load()
- get()/post()

### jQuery与其它框架共同使用

noconflict()可一释放jQuery对$的占用