---
layout: post
title: underscore源码分析
---

{{ page.title }}
================

underscore 最主要的一个特性是链式调用

<pre class="language-javascript">

_([1,2,3]).each(console.log)

// 1 0 (3) [1, 2, 3]
// 2 1 (3) [1, 2, 3]
// 3 2 (3) [1, 2, 3]
</pre>

我们先简单的实现链式调用的功能

实现 _.each([1,2,3],console.log) 是很简单的 ，直接_.each函数就搞定了

关键的_().each实现思路也很简单，我们可以这么理解
当我们返回了new _(obj),如果想调用each方法时，只要把each绑定到prototype就可以了
我们再把传参obj传给each方法，可参考_.each.apply(_, args)

<pre class="language-javascript">

var _ = function(obj) {
  //因为new _持续调用_, 所以加入if 控制死循环
  if (!(this instanceof _)) return new _(obj);
  //把传入的obj参数保存起来
  this._wrapped = obj;
};

_.each = function(obj, iteratee) {
  var i, length;
  for (i = 0, length = obj.length; i < length; i++) {
       iteratee(obj[i], i, obj);
    }
  return obj;
};


_.prototype.each = function() {
  var args = [this._wrapped];
  [].push.apply(args, arguments);
  return _.each.apply(_, args)
};

_.each([1,2,3],console.log)
_([1,2,3]).each(console.log)

</pre>


