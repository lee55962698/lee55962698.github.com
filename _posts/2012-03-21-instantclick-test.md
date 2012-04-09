---
layout: post
title: 关于instantClick的几则实验
category : mobile
tags : [tools, front-end, webapp, jquery]
---
{% include JB/setup %}

>instantClick is a simple jQuery plugin, aimming to avoid the default 300ms delay for 'click' event in touch devices.

[InstantClick][github]预期的基本功能已经实现了，虽然还没有进行全面的测试，但近期事情比较多应该暂时不会再理会它了，所以就以v1.0发布了。

当然这不是这篇的主题，在开发的过程中遇到一些问题，并且做了几个实验，记录于此。

###原理###

在移动设备浏览器上手指快速点击屏幕后，事件发生的顺序是 ```touchstart [ - touchmove ] - touchend - click```


快速点击屏幕之后的在```touchend```之后浏览器会等待几百毫秒来等待下一次点击(双击)，如果没有则触发```click```事件，所以会给人感觉存在一定的延时。这种延时在一些WebApp中会带来不好的体验（试想一个打地鼠游戏或者在线的拨号面板），以至于给人WebApp不如NativeApp流畅的错觉。


如果```touchstart```与```touchend```之间的时间间隔大于该等待值并且没有触发移动事件(```touchmove```)，则```click```事件会立即触发。


在测试过程中，Android4.0自带浏览器表现的很奇怪。经测试，当```meta```标签里有```user-scale=no```时,```click```事件会在```touchend```事件后立即触发，但依然有明显的延迟。


###实验设备###

[Page1](http://jsbin.com/afiwok/9)

测试手指离开屏幕(```touchend```)与绑定事件触发之间的延时<br/>
Test_A: ```$(this).click()``` (快速点击/长按)<br/>
Test_B: ```$(this).instantClick()```

[Page2](http://jsbin.com/iketuc/3) 

with ```<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">```<br/>
测试手指离开屏幕(```touchend```)与绑定事件触发之间的延时<br/>
Test_C: ```$(this).click()``` (快速点击/长按)

###实验结果###

<table><tbody><tr><td>
</td><th>android2.2</th><th>android2.3
</th><th>android4.0
</th><th>android4.0 with chrome beta</th></tr><tr><th>Test_A:</th><td>290/3</td><td>335/3</td><td>302/12</td><td>290/8</td></tr><tr><th>Test_B:</th><td>7</td><td>40</td><td>63 有延时感</td><td>20无延时感</td></tr><tr><th>Test_C:</th><td>318/19</td><td>342/12</td><td>14/9 但有明显延迟</td><td>300/5</td></tr></tbody></table>

单位：ms，前一数字为快速点击的结果，后为按住200ms左右提起手指的结果。

###结论###
在android2.x上该插件表现非常好，4.0中设置```meta user-scalable=no```后```click```事件能被立即触发，故使用该插件并不会更流畅，但相比2.x依然会有明显的延迟感。在chrome for android上测试结果与2.x相似。

[github]: https://github.com/lee55962698/instantClick