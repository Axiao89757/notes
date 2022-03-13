# 爬虫-动态JavaScript渲染

## 问题

1. 能够成功爬取

2. 但是爬取下来的html文件永远都是一个“JavaScript不启用系统不能运行”的页面

```html
<body>
<noscript><strong>We're sorry but system doesn't work properly without JavaScript enabled. Please enable it to
    continue.</strong> <strong>非常抱歉! JavaScript不启用系统不能运行。请启用后继续访问。</strong></noscript>
<div id=app></div>
<script src=/Cesium/Cesium.js></script>
<script src=http://api.tianditu.gov.cn/cdn/plugins/cesium/cesiumTdt.js></script>
<script src="http://api.tianditu.gov.cn/api?v=4.0&tk=f42b689639dba77b78dad5b690f2b6d0"></script>
<script src=http://lbs.tianditu.gov.cn/api/js4.0/opensource/openlibrary/HeatmapOverlay.js></script>
<script src=/js/chunk-vendors.a1d77c29.js></script>
<script src=/js/app.3b8db23e.js></script>
</body>
```

## 问题解决

参考[小白学 Python 爬虫（9）：爬虫基础 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904009526935566)

当页面需要动态的加载渲染的时候，爬取下来的html就不是最终的html，所以显示永远是这个页面
