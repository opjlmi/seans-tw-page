---
title: 啟用Blender CUDA功能
date: 2015-09-30 10:54:35
categories: 3D-Design
tags:
- CUDA
- Blender
- enable
- nvidia
- graphic-card
- tutorial
---

最近在學校有上Blender的課，第一堂課就是要把顯卡開啟CUDA支援（學校機器搭的是Nvidia的顯卡）。所以在這邊筆記一下，以免下次要裝環境又找不到資料。
<!-- more -->

## 0. 環境
- Debian Jessie 8
- Nvidia 顯卡
- 本篇文章在2015/09/30撰寫完畢

<br>

## A. Enable contrib non-free
{% highlight bash %}
$ sudo vi /etc/apt/sources.list
{% endhighlight %}

deb & deb-src 那兩行後面新增 contrib non-free (通常在前面幾行而已)

{% highlight vi %}
deb http://opensource.nchc.org.tw/debian/ jessie main contrib non-free
deb-src http://opensource.nchc.org.tw/debian/ jessie main contrib non-free
{% endhighlight %}

ps.小心別改到security的或其他行
{% highlight bash %}
$ sudo apt-get update; sudo apt-get upgrade
{% endhighlight %}

## B. Install Nvidia Kernel Driver
{% highlight bash %}
$ sudo apt-get install nvidia-kernel-dkms nvidia-driver
$ sudo apt-get install nvidia-cuda-toolkit nvidia-xconfig
{% endhighlight %}

## C. Disable Nvidia module

要關閉這些module的原因，是因為在新版Jssie中，早已經沒有nv與ast這兩個module，所以當你沒關閉的時候他就會無限reload.....無限找不到.....然後就在`startx`那邊卡超級久才能進去Xwindow。

{% highlight bash %}
$ sudo vi /etc/modprobe.d/fbdev-blacklist.conf
{% endhighlight %}

在最下面新增以下三行：
{% highlight vi %}
blacklist nv
blacklist ast
blacklist nouveau
{% endhighlight %}

存檔

## D. Rebuild X config file
{% highlight bash %}
$ sudo nvidia-xconfig
{% endhighlight %}


## E.Reboot

## F. Check nvidia module
{% highlight bash %}
$ lsmod | grep nvidia
{% endhighlight %}
