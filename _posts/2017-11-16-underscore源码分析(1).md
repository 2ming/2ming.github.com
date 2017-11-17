---
layout: post
title: underscore源码分析
---

{{ page.title }}
================

underscore 版本1.8.3 最主要的一个特性是链式调用

<pre class="language-javascript">

_([1,2,3]).each(console.log)

// 1 0 (3) [1, 2, 3]
// 2 1 (3) [1, 2, 3]
// 3 2 (3) [1, 2, 3]
</pre>

我们先简单的实现链式调用的功能

实现 _.each([1,2,3],console.log) 是很简单的 ，直接_.each函数就搞定了

<p>关键的_().each实现思路也很简单，我们可以这么理解</p>
<p>当我们返回了new _(obj),如果想调用each方法时，只要把each绑定到prototype就可以了</p>
<p>我们再把传参obj传给each方法，可参考_.each.apply(_, args)</p>

<a href="https://jsfiddle.net/2ming/p3cv5fnm/1/">jsfiddle在线调试</a>
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

但是 我们怎么实现下面的功能呢
<pre class="language-javascript">
_([1,2,3]).map(function(a){return a * 2}).each(console.log) //报错

// 2 0
// 4 1
// 6 2
</pre>
在用underscore的时候我们发现有个_.chain方法，可以实现链式连写
<pre class="language-javascript">
_.chain([1,2,3]).map(function(a){return a * 2}).each(console.log)

// 2 0
// 4 1
// 6 2
</pre>
我们接下来简单实现
主要的关键点是chainResult函数，返回_(obj).chain()
<a href="https://jsfiddle.net/2ming/p3cv5fnm/4/">jsfiddle在线调试</a>
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
_.map = function(obj, iteratee, context) {
    var length = obj.length,
        results = Array(length);
    for (var index = 0; index < length; index++) {
      results[index] = iteratee(obj[index], index, obj);
    }
    return results;
  };
 var chainResult = function(instance, obj) {
    return instance._chain ? _(obj).chain() : obj;
  };
  
_.chain = function(obj) {
  var instance = _(obj);
  instance._chain = true;
  return instance;
};

_.prototype.each = function() {
  var args = [this._wrapped];
  [].push.apply(args, arguments);
  return chainResult(this, _.each.apply(_, args))
};
_.prototype.map = function() {
  var args = [this._wrapped];
  [].push.apply(args, arguments);
  return chainResult(this, _.map.apply(_, args))
};
_.prototype.chain = function() {
  var args = [this._wrapped];
  [].push.apply(args, arguments);
  return chainResult(this, _.chain.apply(_, args))
};

_.chain([1,2,3]).map(function(a){ return a * 2}).each(console.log)
</pre>

我们发现prototype原型链的时候调用的方法都一样的，可以写个函数循环
<a href="https://jsfiddle.net/2ming/p3cv5fnm/5/">jsfiddle在线调试</a>
<pre class="language-javascript">
function mixin(){
  _.each(['chain', 'each', 'map'], function(name) {
    var func = _[name]
    _.prototype[name] = function() {
      var args = [this._wrapped];
      [].push.apply(args, arguments);
      return chainResult(this, func.apply(_, args));
    };
  });
}
</pre>
