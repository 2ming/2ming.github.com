---
layout: post
title: javascript闭包（closure）
---

{{ page.title }}
================

<p class="meta">29 Apr 2015 - jekyll博客搭建</p>

以前面试的时候，有面试官问到过此问题，当时没回答出来。

后来回去了，深入了解了下关于闭包，下面是我的笔记和理解。

1.变量的作用域

假如你直接在函数外面定义一个变量，函数是可以直接访问这个变量的。

<pre>
var a = 0;
    d = 4;

function demo(){
	var b = 1;
	    c = 2;
	console.log("a="+a);//0 函数内没有a变量然后向函数外面查找
	console.log("d="+d);//4 函数内没有d变量然后向函数外面查找
}

function demo2(){
	var d = 5;
	    a = 1;
	console.log("a2="+a);//1 函数内从新给a赋值1 所以所处的是1
	console.log("d2="+d);//5 函数内有定义d变量 
}


demo();
demo2();
//console.log(b);//error b定义在函数demo里面 外面访问不到
console.log(c);//2 函数demo里 定义了一个全局变量
console.log(a)//1 //函数demo2改变了全局变量a的值
console.log(d)//4 //函数demo2没有改变全局变量的值	
</pre>



我们来理解下上面的例子，然后明白

函数外面定义变量加上var是全局变量，不加则是局部变量

函数里面定义变量加上var是函数内部局部变量，不加则是全局变量

2.闭包

那么问题来了，如何读取到函数的局部变量呢？？

<pre>
	function demo(){
	var a = 111;
	
	//我就是闭包
	function demo2(){
		console.log(a);//访问到了
	}

	return demo2;//把demo2暴露出去
}

var d = demo();

    d();//111;
</pre>

闭包很难理解吧，那就记住一句话

能读取其他函数内部变量的函数就是闭包，比如demo2

3.闭包作用

都说闭包，闭包，那闭包有啥用，一般来说有2个。
一个是读取其他函数内部的变量，
另一个是让读取到的变量始终保存在内存中。

听起来是不是感觉很吊的样子，其实也就那个样子...

<pre>
function d1(){
	var a = 111;
	
	aAdd = function(){
		a +=1;
	}

	function d2(){
		console.log(a);
	}

	return d2;
}

var d = d1();

    d();//111;

//这个时候 a已经保存在内存中了

    aAdd();

    d();//112	
</pre>


就是这么回事，当然我们也不能乱用闭包，这样会导致大量的变量保存在内存中，导致内存泄露的可能。





