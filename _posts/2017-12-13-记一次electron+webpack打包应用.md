---
layout: post
title: electron+webpack打包应用
---

{{ page.title }}
================
<img src="//2ming.github.com/images/20171213-1.jpeg">


第一步：拉取<a href="https://github.com/electron/electron-quick-start" target="_blank">electron-quick-start</a>库,这里不做过多介绍，具体可参考链接<a href="http://www.jianshu.com/p/785ed0ac08ee" target="_blank">Electron + React DeskTop开发环境搭建</a>开发环境搭建

第二步：我引入了webpack+react,主要是react打包的一些配置文件，具体的可以看源代码，源码地址会后面贴出

第三步：引入<a href="https://github.com/electron-userland/electron-builder" target="_blank">electron-builder</a>打包应用程序，关键的package.json指定了打包的文件，配置如下：

<pre class="language-javascript">
"files": [
  "dist/",
  "index.html",
  "main.js",
  "package.json"
]
</pre>

参考的一些库：

<a href="https://github.com/SimulatedGREG/electron-vue" target="_blank">electron-vue</a>
 <a href="https://github.com/fireyy/react-antd-admin" target="_blank">react-antd-admin</a>
  <a href="https://segmentfault.com/a/1190000007616641">自动更新的完整教程（Windows 和 OSX）</a>
  <a href="https://github.com/2ming/electron-docs" target="_blank">源码地址</a>