pc页面在移动端上的显示效果有出入，因此引入了viewport这个概念。

viewport分为：visual viewport(度量/视图)以及layout viewport(布局)

两者间存在一个缩放比：scale

可以通过 meta 标签来修改viewport的一些参数

```
<meta name="viewport"
	content="width=device-width,
	initial-scale=1,
	minimun-sclae=1,
	maximun-scale=2,
	user-scalable=no">
document.body.clientWidth(visual viewport)
window.innerWidth(layout viewport)
```

