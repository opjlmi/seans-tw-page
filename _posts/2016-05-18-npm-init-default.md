---
title: '如何設定npm init的default預設值'
categories: Nodejs
tags:
- npm
- init
- default
- author
- mail
- license
---

在每次建立nodejs的專案時，有沒有遇到一個問題？每次都要重新輸入作者的姓名、或者是每次想要設定成MIT的license，但是預設都是ISC。有沒有辦法讓一開始預設值就是設定成自己想要的值呢？

<br>

## npm版本

在這次文章中使用的npm版本是v3.9.1，如果是使用比較舊/新的版本，可能指令會比較不太一樣。

<br>

## 設定npm init預設值

npm裡面指令能提供使用者設定init時的預設值。只要輸入下方指令：

{% highlight bash %}
  npm config set init-author-name [yourname]
  npm config set init-author-email [your@email.com]
  npm config set init-author-url [http://your.url]
  npm config set init-license MIT
{% endhighlight %}

<br>

之後建立新的專案時，`npm init`時的預設值就會是你當初設定的值哦！當然也可以直接到`~/.npmrc`裡面直接設定也行！

<br>