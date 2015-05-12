---
layout: post
title: ECMAScript 6 笔记（一）
---

{{ page.title }}
================

<p class="meta">12 May 2015 - ECMAScript 6 笔记（一）</p>

let 声明变量

<pre class="language-javascript">
{
	let a = 10;
	var b = 1;
}
a //a is not definde
b //1
</pre>


let 只在声明的代码块中生效

<pre class="language-javascript">
//var 变量声明
var a = [];
for(var i = 0;i<10;i++){
	a[i] = function(){
		console.log(i);
	}
}
a[6](); //10

</pre>

<pre class="language-javascript">
//闭包方法
var a = [];
for(var i = 0;i<10;i++){
	a[i] = (function(i){
		return function(){
			console.log(i);
		}
	})(i)
}
a[6](); //6

</pre>

<pre class="language-javascript">
//let 声明
var a = [];
for(let i = 0;i<10;i++){
	a[i] = function(){
		console.log(i);
	}
}
a[6](); //6

</pre>

我们看上面的代码，a[6]返回6才是我们想要的值。

如果是以前，我们可能要利用闭包才能得到我们想要的值。而let出现后，代码变得简单，也更加的清晰。