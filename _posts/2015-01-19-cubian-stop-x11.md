---
title: 如何關閉Cubian開機自動啟動X11
date: 2015-01-19 22:38:53
categories: Cubieboard
tags:
- cubian
- stop-X11
---
最近開始嘗試玩嵌入式系統的部份，所以就把塵放已久的Raspberry pi跟Cubie board拿出來，在使用兩個板子之間的時候總是有個疑問，為何Cubie board安裝官網的Cubian之後，開機都會自動啟動X11，實在很希望關掉。今天就來紀錄一下如何關閉自動開機啟動X11的功能。

##只有一步
你只要進去 `/etc/X11/default-display-manager`，將第一行註解掉即可。

{% highlight bash %}
sudo vi /etc/X11/default-display-manager
{% endhighlight %}

{% highlight bash %}
#/usr/bin/slim
{% endhighlight %}


就這樣，結束。
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
就這樣而已啊真的XDDDDDDDDD