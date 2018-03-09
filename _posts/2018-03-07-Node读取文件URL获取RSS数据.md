---
layout: post
title: Node读取文件URL获取RSS数据
---

{{ page.title }}
================

2018的主要目标就是学习Node，话说node应该是14年就开始听说过。15年就开始学习，断断续续，一直不得其解。
不说了，说多了都是泪😭

我们首先看一下功能需求

<img src="//2ming.github.com/images/1520414629371.jpg">

这里我们要而外用到2个包,request(简化的HTTP客户端)和htmlparser(把原始的RSS数据转换成JavaScript)

另外我们准备了一个rss_feeds.txt的文件，里面是RSS的URl

<pre class="language-javascript">
// rss_feeds.txt
https://www.zhihu.com/rss
http://www.toodaylab.com/feed
</pre>

1、checkForRSSFile 检查文件存在

<pre class="language-javascript">
//fs.access 检查一个文件是否存在,且不操作
//如果读取不到文件，看下文件路径是否正确，fs默认从根目录开始读取文件
fs.access(configFilename, function(exists) {
    if (!exists)
      return next(new Error('Missing RSS file: ' + configFilename));

    next(null, configFilename);
  });
</pre>

2、readRSSFile 读取文件

<pre class="language-javascript">
// fs.readFile读取文件，返回Buffter
fs.readFile(configFilename, function(err, feedList) {
    if (err) return next(err);

    //把buffter转换成字符串后替换前后的空格并"\n"拆分成数组
    feedList = feedList
               .toString()
               .replace(/^\s+|\s+$/g, '')
               .split("\n");
    //随机获取小于等于数组长度的数
    var random = Math.floor(Math.random()*feedList.length);
    next(null, feedList[random]);
  });
</pre>

3、downloadRSSFeed 获取Rss数据

<pre class="language-javascript">
//request 请求url,获取url文件内容
request({uri: feedUrl}, function(err, res, body) {
    if (err) return next(err);
    if (res.statusCode != 200)
      return next(new Error('Abnormal response status code'))

    next(null, body);
  });
</pre>

4、parseRSSFeed 转换数据

<pre class="language-javascript">
  // 把RSS转换成javaScript数据
  var handler = new htmlparser.RssHandler();
  var parser = new htmlparser.Parser(handler);
  parser.parseComplete(rss);
  if (!handler.dom.items.length)
    return next(new Error('No RSS items found'));

  var item = handler.dom.items.shift();
</pre>

总结：

fs.access检查文件应用，fs.readFile读取文件
request发送请求获取数据
htmlparser对HTML,XML页面的解析
