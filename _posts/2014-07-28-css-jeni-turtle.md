---
title: 做一張CSS手刻圖
date: 2014-07-28 21:39:56
categories: CSS
tags:
- css
- turtle
- webkit
---

上週六去參加了桃園前端開發者社群辦的 **熊大與喬巴的CSS3 Workshop**，這個Workshop是在教大家如何使用CSS去手刻圖案。講者有說這種CSS手刻其實是做爽的，不過做起來真的很爽啊哈哈哈哈哈！我這次挑了傑尼龜來當作這次Workshop的目標成品，雖然做起來有點(?)好笑，不過還是來紀錄一下好了。
ps....真的醜到很好笑XD

##Live Demo
[手刻傑尼龜HTML](https://3fae846b4f4260ad3885a08e87ba1f78895ffd48.googledrive.com/host/0B6P9S7j0fBuYZVFmVWstd3pkUkk/)
[手刻傑尼龜CSS](https://3fae846b4f4260ad3885a08e87ba1f78895ffd48.googledrive.com/host/0B6P9S7j0fBuYZVFmVWstd3pkUkk/setting.css)
##前置準備
在編輯器的部份建議使用Sublime+LiveStyle，
瀏覽器使用Chrome即可(記得也要裝LiveStyle)。
一定要記得裝LiveStyle，不然F5會按到手軟......Orz
##基本語法
<p>首先介紹最基本的語法， `width` 與 `height` 是在控制圖層的長與寬，可以使用像素(px)為單位，或者是使用百分比(%)來控制都行，百分比如果使用100%就是代表是 **目前瀏覽器視窗** 的大小。而 `font-size` 由字面上看起來就是控制字體大小， `background` 就是在控制圖層的背景顏色，可以使用pink、red、blue...等顏色單字控制，當然也可以使用rgb去控制(用rgba控制就是多alpha透明度的功能)。
`overflow` 這個東西就要特別說明一下，當你有設定圖層的長寬時，當裡面的東西超過原本的圖層大小，內容就會被截掉。截掉的方式可以選擇scroll(捲軸)或hidden(隱藏)。
</p>
{% highlight css %}
  width: 200px;
  height: 350px;
  font-size: 20px;
  overflow: hidden;
  background: rgb(125,200,205);
{% endhighlight %}

<p>margin與padding的部份可以分別稱作為外距與內距。這邊不多作說明，直接把margin與padding丟到Google上面就會有一堆資料。
</p>

{% highlight css %}
  margin: 0;
  padding: 0;
{% endhighlight %}

<p>`border` 是用來控制邊寬的線條。當你使用 `border` 的時候就可以讓你的圖層邊框有線，使用方法就是 `border: 邊寬粗細 樣式 顏色` 。這樣子設定就可以讓圖層邊框有顏色。
`border-radius` 是用來控制邊框的圓角功能，只要輸入數字就可以控制圖層變成圓的，當然這部份可以使用像素也可以使用百分比去作控制。不過為什麼後面會有一個除法的符號呢？當你使用除法符號的時候，就可以控制水平與垂直的圓角角度，詳細的控制方式還是要自己動手做會比較清楚一點。
</p>
{% highlight css %}
  border: 3px solid black;
  border-radius: 50%/ 55% 55% 25% 25%;
{% endhighlight %}
<p>
當然在控制邊框粗細的地方也可以各別作設定，下面有範例可以參考。當然可以搭配rgb或直接顏色都可以，不過 `left` 跟 `right` 是使用 `transparent` ， `transparent` 可以讓圖層顏色變透明，對，就是透明。
</p>
{% highlight css %}
  border-top:40px solid rgb(125,200,215);
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
  border-bottom: 0;
{% endhighlight %}
<p>接下來是對圖層的位置設定，設定的方式有兩種，一種是 `absolute` (絕對位置)，一種是 `relative` (相對位置)。當設定完position之後，才能開始使用 `top` 與 `left` 去設定圖層xy位置。
</p>
{% highlight css %}
  position: relative;
  top: 75px;
  left: 85px;
{% endhighlight %}
<p>`transform` 簡單來說就是可以把圖層變形的工具，rotate(旋轉)、matrix(傾斜)、matrix(縮放)都可以，不過記得前面要加上 `-webkit-` ，否則有些設定在某些瀏覽器是看不到的。
</p>
{% highlight css %}
  -webkit-transform:rotate(30deg);
{% endhighlight %}
<p>
當要製作手刻圖的時候，比如說今天要做個眼睛，眼睛簡單來說有分三層(眼白、眼珠、瞳孔)，但是有時候在html的時候沒刻好，可能眼白跑到最上層，把眼珠跟瞳孔都蓋掉了。這時你就可以使用 `z-index` 來設定圖層的前後，只要數字越大就越前面，數字越小就在越後面。
</p>
{% highlight css %}
    z-index: -2;
{% endhighlight %}
<p>`before` 與 `after` 是比較特別的用法，也稱作 `偽元素` ，當你使用這個語法的時候，可以讓一個圖層裡面作兩個設定，使用方法如下。
</p>
{% highlight css %}
  *:before, *:after{
    content:'';
    display: block;
  }

  .fin1line:before{
      border:2px solid black;
  }

  .fin1line:after{
      border:2px solid black;
  }
{% endhighlight %}

##注意
在刻html的時候，記得在div設定的時候一律使用class，千萬不要使用id！原因在於id只能使用一次，但是class卻可以無限使用。以下是範例。
{% highlight html %}
<div class="a b c d"></div>
//代表此圖層使用.a .b .c .d的設定
<div id="hello"></div>
//代表此圖層使用#hello的設定，而且其他圖層不能在稱作hello
{% endhighlight %}

##開始製作手刻圖吧
上面雖然語法有那麼多，不過實際上就是針對每個圖層去作設定，把圖層變成圓的、紅的....etc.。然後一層一層拼起來就是你要的東西。手刻圖並不難，難的地方是在於你花多少時間。現在就開始自己製作自己的手刻圖吧！