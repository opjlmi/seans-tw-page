---
title: 使用ES6與ES7在nodejs上執行程式
categories: Nodejs
tags:
- nodejs
- es6
- es7
- babel
---

最近開始在台南創立[jsTainan](https://www.facebook.com/jstainan/)，覺得應該是該好好來寫Javascript了。然後又聽說最近ES6很夯，所以想說來寫一下ES6應該會很潮，誰知道從那個時候開始就開始踩了一堆的雷....... (雷雷夥伴的概念哈哈哈哈)，所以在這邊來紀錄一下如何在nodejs上**\*\*順利\*\***執行ES6與ES7的方法。

<br>

## 環境安裝

如果你要使用ES6的話，你必須要先安裝babel。要安裝babel的話有兩種方式，一種是**\*\*使用global的方式\*\***安裝，一種是**\*\*直接安裝在專案資料夾\*\***內。當然兩種的使用方式就不太一樣了，等等都會說明到。

<br>

## 使用Global方式安裝Babel

{% highlight bash %}
npm install -g babel-core
{% endhighlight %}

### 加入ES6、ES7支援讓babel順利執行

現在babel6版後，所有語言的轉換器都必須要另外安裝，所以請使用npm安裝以下套件(es2015表示ES6轉換器，stage-3表示ES7轉換器)：

{% highlight bash %}
npm install babel-preset-es2015 babel-preset-stage-3 --save-dev
{% endhighlight %}

### 新增.babelrc

接下來請在專案資料夾中新增 `.babelrc` ，並在裡面輸入：

{% highlight bash %}
{
    presets: [ "es2015", "stage-3" ]
}
{% endhighlight %}

### 挖賽，程式終於可以動辣

接下來只要直接執行 `babel-node your-file.js` 就可以成功執行了。恭喜你終於不用踩那麼多雷了，不過之後當然還是會踩雷，那機率就是人品問題了XDDDD

<br>

## 直接將babel安裝在專案資料夾內

除了Global的安裝方法之外，你也可以直接將babel安裝在專案資料夾中。這時候只需要一次全部裝進去就可以囉！

{% highlight bash %}
npm install babel-core babel-polyfill babel-preset-es2015 babel-preset-stage-3 --save-dev
{% endhighlight %}

## 如果你是使用nodejs6以上的版本

如果你是安裝nodejs v6以上的版本，因為在6版之後ES2015已經支援到96%，所以你只需要安裝[babel-preset-es6-node6](https://www.npmjs.com/package/babel-preset-es6-node6)將缺少的features補上即可。但是如果還是有使用到es7以上的語法還是得安裝 `babel-preset-stage-3` 哦！

### 比較特別的是.....
如果你想用 `node your-file.js` 執行的話，你必須要在你的啟動程式引用

{% highlight js %}
require('babel-core/register');
require('babel-polyfill');
{% endhighlight %}


而且要小心！你的起始程式並不能有任何ES6或ES7的相關語法，除了其他include進來的程式才能有。小心啊小心.......雖然說ES6+nodejs基本上目前是一個好像有點(非常)雷的坑，不過跳進來就對啦，反正看到一個雷就除一個，是吧XDDDDD