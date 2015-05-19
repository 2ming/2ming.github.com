---
layout: post
title: Javascript权威指南核心参考笔记（一）
---

{{ page.title }}
================

<p class="meta">12 May 2015 - Javascript权威指南核心参考笔记（一）</p>

核心参考笔记很长，我会分多个部分来记录。

<b>arguments[]</b> 函数参数数组

<b>arguments.callee</b> 指当前正在执行的函数(通过它可以引用匿名函数自身)

<pre class="language-javascript">
var factorial = function(x){

	if(x<2) return 1;
	else return x*arguments.callee(x-1);//调用自身
}
var y = factorial(5); //120
</pre>

<b>arguments.length</b> 传递给函数的参数个数，以及Arguments对象中数组元素的个数

<pre class="language-javascript">
function check(args){
	console.log(args);//[1,2]
	var actual = args.length;//2 
	var expected = args.callee.length;//3
	if(actual != expected){
		throw new Error("参数个数有误：期望值："+
						expected + ";实际值："+ actual);
	}
}

//演示如何使用check()方法的实例函数
function f(x,y,z){
	check(arguments);
	return x+y+z;
}

f(1,2);
</pre>

<b>Array 数组</b>

方法 

<b>Array.concat()</b> 把元素衔接到数组中，返回新数组。

<pre class="language-javascript">
var a = [1,2,3];
var b = a.concat(4,5);
console.log(a) //[1,2,3]
console.log(b) //[1,2,3,4,5]
</pre>

<b>Array.every()</b> 测试断言函数是否对每个元素为真

<pre class="language-javascript">
[1,2,3].every(function(x){ return x < 5 }) // true 所有元素都
</pre>

<b>Array.filter()</b> 返回通过断言的元素的新数组

<pre class="language-javascript">
[1,2,3].filter(function(x){return x > 1;}); //[2,3]
</pre>

<b>Array.forEach()</b> 为每一个数组元素调用一个函数，无返回值。

<pre class="language-javascript">
var a = [1,2,3];
a.forEach(function(x,i,a){
	a[i]++;
	console.log(x,i,a);
});

//输出：

//1 0 [2, 2, 3]
//2 1 [2, 3, 3]
//3 2 [2, 3, 4]
</pre>

<b>Array.indexOf()</b> 查找数组

返回匹配元素的位置，不匹配则返回-1

<pre class="language-javascript">
['a','b','c'].indexOf('b') // 1
['a','b','c'].indexOf('d') // -1
['a','b','c'].indexOf('a',1) // -1
</pre>

<b>Array.lastIndexOf()</b> 反向查找数组

等同于indexOf()，从数组最后一个元素开始查找

<b>Array.join()</b> 将数组元素衔接为字符串

<pre class="language-javascript">
var a = new Array(1,2,3,"ming");
var s = a.join("+"); //s = "1+2+3+ming";
</pre>

<b>Array.length</b> 数组长度

<b>Array.map()</b> 从数组元素中计算新值,返回新数组。

<pre class="language-javascript">
[1,2,3].map(function(x){return x*x}); //[1,4,9]
</pre>

<b>Array.pop()</b> 移除并返回数组的最好一个元素

<pre class="language-javascript">
var stack = [1,2,3];
var arr2 = [];
stack.pop(); //stack:[1,2] 返回: 3
arr2.pop(); //stack:[] 返回：undefined
</pre>

<b>Array.push</b> 给数组追加元素，返回新数组长度

<pre class="language-javascript">
var stack = [1,2];
stack.push(3,4) //stack:[1,2,3,4] 返回：4
</pre>






