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

<pre class="language-javascript">

var _ = function(obj) {
  if (obj instanceof _) return obj;
  if (!(this instanceof _)) return new _(obj);
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

_([1,2,3]).each(console.log)

</pre>

