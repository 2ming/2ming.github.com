---
layout: post
title: Nodejs之express学习笔记（1）
---

{{ page.title }}
================

<p class="meta">10 Jun 2015 - Nodejs之express学习笔记(一)</p>

写之前，想写一段题外话。</br>
其实作为一个菜鸟，我最大的体会就是：</br>
不是我不想学习，而是不知道怎么学习；不是我不认真学习，而是学了后还是什么都不会的感觉。</br>
其实你请教前辈或者牛人的话，一般都会甩给你一句话：“代码写的少，多写多练“。</br>
前辈，牛人说的都是事实，但当我们去练习但时候，却发现无从下手，一次次的，积极性自然下降。</br>
我们去看别人的代码或者去看项目的api，我们即使时照着别人的代码写一遍又一遍，我们感觉还是不会。</br>
这里面的一个最主要的原因就是－“编程思想”。</br>
因为我们不懂得，为什么要这么写，这么写的好处。因为我们一拿到手的就是牛人写的精炼的代码。</br>
像我们这种还没什么思想的菜鸟，只会明白所以然，却不会明白之所以然。我们可以去思考牛人在想什么。</br>
虽然一个很简单但demo牛人只要几分钟，但我们可能会花上一天的时间，我觉得这个时间是值得的。</br>
我不想总是徘徊在似懂非懂的边缘，我们应该要有更高的追求。</br>
出于上面的一些思考，会有下面的一系列的笔记，我也不知道效果怎么样。</br>
但我们的确实缺少练习，练习不仅仅时照抄，更要学习思想。</br>
实践出真知，大家拭目以待。</br>

回归正题...

下面代码，是我根据express示例里面auth文件，然后自己学习写的。

首先思路，要实现都功能。

1.输入用户名、密码，验证用户名、密码是否正确。

知道功能后，我们开始着手写代码

1.导入相应都模块

2.初始化

3.把view表单文件写好

4.绑定端口，输入地址时，显示相关的页面

5.提交表单，提交的内容，跟密钥进行匹配，根据提交的信息做相关的处理

<pre class="language-javascript">
//第一步
//导入相应的模块
//express Web框架
//body-parser 涉及到页面操作
//hash 密码加密
var express = require("express");
var bodyParser = require('body-parser');
var hash = require('./pass').hash;

//第二步
//初始化，生成express对象
//设置模版,需获取表单内容，解析客户端请求的内容
var app = express();

app.set('view engine', 'ejs'); //设置view模版
app.set('views', __dirname + '/views');//设置view模版路径

//解析客户端请求的body中的内容
app.use(bodyParser.urlencoded({ extended: false }));

//输入地址跳转
app.get('/', function(req, res){
  //指向登入
  res.redirect('/login');
});
//输入地址跳转
app.get('/login', function(req, res){
  //显示登入页面
  res.render('login');
});

//提交表单
//通过authenticate函数，去严重用户名和密码是否正确
app.post('/login',function(req,res){
	//req.body.username
	//req.body.password
	//导入用户名跟秘密，生成密钥，看是否匹配
	authenticate(req.body.username,req.body.password,function(err,user){
		//if(err) ;
		if(user){
			console.log("登入成功")
		}else{
			console.log("登入失败")
			res.render('login')
		}
	})
})

//验证已知的用户名密码
var users = {
	tj:{
		"name":'tj'
		}
}
//传入密码和回调函数，生产密钥
hash('foobar',function(err,salt,hash){
	if(err) throw err;
//console.log(salt,hash)
	users.tj.salt = salt;
	users.tj.hash = hash;
})


function authenticate(name,pass,fn){

	var user = users[name];
	//console.log(user)

	if(!user) return fn(new Error('cannot find user'));

	hash(pass,user.salt,function(err,hash){
		if(err) return fn(err)

		if(user.hash == hash) return fn(null,user);

		fn(new Error('invalid password'))
	})
}

//绑定端口
if (!module.parent) {
  app.listen(3000);
  console.log('Express started on port 3000');
}
</pre>

<pre class="language-javascript">
//密钥生成相关代码
var crypto = require('crypto'); 



var len = 128;

var iterations = 128000;

exports.hash = function(pass,salt,fn){

	if(3 == arguments.length){
		crypto.pbkdf2(pass,salt,iterations,len,function(err,hash){
			fn(err,hash.toString('base64'))
		});
	}else{
	  //初次生成密钥
		fn = salt;
		crypto.randomBytes(len,function(err,salt){

			if(err) return fn(err);

			salt = salt.toString('base64');

			crypto.pbkdf2(pass,salt,iterations,len,function(err,hash){
			  //返回密钥，并保存。
				fn(null,salt,hash.toString('base64'))
			})
		})

	}

}
</pre>

<pre class="language-javascript">
<code>
//第三步写好表单
<form method="post" action="/login">
  <p>
    <label>Username:</label>
    <input type="text" name="username">
  </p>
  <p>
    <label>Password:</label>
    <input type="text" name="password">
  </p>
  <p>
    <input type="submit" value="Login">
  </p>
</form>
</code>
</pre>
