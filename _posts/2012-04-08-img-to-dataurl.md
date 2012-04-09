---
layout: post
title: 图片转dataURL小工具一枚
category : mobile
tags : [tools, dataurl, webapp]
---
{% include JB/setup %}

开发WebApp时经常需要将图片转换成dataURL（[关于dataURL](http://www.google.com.hk/search?sourceid=chrome&ie=UTF-8&q=dataURL)），于是做了个Favelet，绝对能帮你节省不少时间。

{% highlight javascript %}
 javascript:(function(e){function b(g){this.name=g.fileName;this.size=Math.ceil(g.size/102.4)/10+' K';var d=this;var f=new FileReader();f.onload=function(h){d.data=this.result;d.appendTo('img2dataurl')};f.readAsDataURL(g)}b.prototype.appendTo=function(g){var f=e.getElementById(g);var d=[f.innerHTML];d.push('<div style=\'height:250px;overflow:hidden;padding:20px 40px; border-bottom:1px solid #CCC;\'><img style=\'width:250px;float:left;\' src=\'');d.push(this.data);d.push('\' /><p style=\'margin:0 0 0 270px;font-size:14px; height:20px; line-height:20px;white-space:nowrap;\'>');d.push(this.name);d.push('</p><p style=\'margin:0 0 10px 270px;font-size:12px; color:#666; height:20px; line-height:20px;white-space:nowrap;\'>');d.push(this.size);d.push('</p><textarea style=\'display:block; height:196px; width:330px; margin:0 0 0 270px; border:1px solid #DDD;\' onmouseover=\'this.select()\' >');d.push(this.data);d.push('</textarea></div>');f.innerHTML=d.join('')};var a=e.createElement('div');a.setAttribute('style','position:absolute; top:0; left:50%; margin-left:-360px; width:720px; text-align:left; background-color:#F4F4F4;box-shadow:0 0 6px rgba(0,0,0,.2);');a.setAttribute('id','img2dataurl');a.innerHTML='加载完成，请拖入图片';e.body.appendChild(a);e.addEventListener('dragenter',function(d){d.stopPropagation();d.preventDefault()},false);e.addEventListener('dragover',function(d){d.stopPropagation();d.preventDefault()},false);e.addEventListener('drop',c,false);function c(g){g.stopPropagation();g.preventDefault();var d=g.dataTransfer.files;a.innerHTML='';for(var f=0;f<d.length;f++){new b(d[f])}}})(document);
{% endhighlight %}