---
title: 如何使用Nodejs執行Python Script
date: 2015-08-09 18:53:15
categories: Nodejs
tags:
- nodejs
- javascript
- python
- python-shell
---
Javascript是最近蠻熱門的程式語言（尤其是Nodejs與iojs的出現），在Github上面Javasciprt也是第二多被使用的語言。想要看程式語言的排名可以到 [這裡](http://langpop.com) 看。我原本其實是以Python為主，不過最近開始慢慢轉到Nodejs後，發現可能要用到之前寫的python程式，但是又很懶得重新再把python的程式再改寫成Nodejs。所以當然希望可以用Nodejs呼叫之前寫的Python程式，然後再經過Nodejs作一些網頁/後端的處理。

不過因為這篇的應用方式比較簡單，所以我直接將步驟寫下來：

<br>

## 使用npm下載python-shell
{% highlight js %}
npm install python-shell
{% endhighlight %}

<br>

## app.js
{% highlight js %}
var PythonShell = require('python-shell');

var options = {
    mode: 'json',
    pythonOptions: ['-u'],
    scriptPath: './',
};

var test =  new PythonShell('test.py', options);
test.on('message',function (message) {
    console.log(message);
});
{% endhighlight %}

<br>

## test.py
{% highlight python %}
import json
import time

print json.dumps({'bar': ('baz', None, 1.0, 2), 'hi':'1'})
time.sleep(3)
print json.dumps({'bar': ('baz', None, 1.0, 2), 'hi':'2'})
time.sleep(3)
{% endhighlight %}

<br>

當你執行`node app.js`時，他就會自動一行一行顯示出來。前提是你在pythonOpetions裡面要加上`-u`的參數，如果沒有加的話，`test.py`會執行完才讓`event:message`抓到。[(原因)](http://stackoverflow.com/questions/14258500/python-significance-of-u-option)

詳細的API說明可以到 [python-shell on Github](https://github.com/extrabacon/python-shell) 查看。