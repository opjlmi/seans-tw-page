---
title: 利用Jekyll與Github Page建立自己的Dev-Blog
categories: Github
tags:
- jekyll
- hexo
- github
- github-pages
- gitcafe
- cname
- blog
---

想建立一個自己的Dev Blog嗎？有些人一直想要創一個屬於自己的開發者Blog，但是卻沒有經費去申請網頁主機，或是覺得[Wordpress](https://tw.wordpress.org)門檻太高，安裝步驟太繁雜。其實有更好的選擇，你可以使用[Jekyll](https://jekyllrb.com)或[hexo](https://hexo.io)之類的平台，直接創一個自己的靜態Blog。空間也不用擔心，因為他只是一堆靜態網頁檔而已，可以直接在[Github](https://github.com)或[GitCafe](https://gitcafe.com)部屬即可。況且，Github Pages目前也有[支援Jekyll](https://help.github.com/articles/using-jekyll-with-pages/)，也就是說你不需要做一大堆的設定就可以使用。那還能不用Jekyll嗎？

<br>

## 下載Jekyll

因為Jekyll是基於ruby的語言寫的，所以你必須先[安裝Ruby](https://www.ruby-lang.org/zh_tw/downloads/)才能使用。當你安裝完Ruby後(含gem)，你就可以使用下面的步驟安裝Jekyll。

{% highlight bash %}
~$ gem install jekyll
{% endhighlight %}

接下來要利用jekyll建立一個新專案並啟動Server

{% highlight bash %}
~$ jekyll new my-awesome-site
~$ cd my-awesome-site
~/my-awesome-site$ jekyll serve
{% endhighlight %}

基本上就這樣而已，很簡單吧。

<br>

## 部屬至Github

Github有一個Pages的服務，讓你可以把自己的靜態網頁丟到Github上。如果你使用Jekyll就簡單了，只要把整包產出來的檔案直接Push到Github上就可以。不過這邊有兩個設定的問題，你的網址是要自訂？還是直接用Github所提供的account.github.io網址呢？

<br>

### Plan A.我想要使用Github內建的github.io網址
如果你要使用內建的就比較簡單，只要確認你的branch是在master上，並將Repo名字設定為account.github.io`(**記得要把account改成你的帳號**)`，後續直接將檔案推到Github上面即可。

{% highlight bash %}
~/my-awesome-site$ git init
~/my-awesome-site$ git add .
~/my-awesome-site$ git commit -m 'init git'

git remote add origin https://github.com/account/account.github.io.git
git push -u origin master
{% endhighlight %}

### Plan B.我想要使用自己購買的網址
如果你想要使用自己購買的網址，你必須要先將branch設定為`gh-pages`，並新增一個CNAME的檔案，內容為你要綁定的網址。並到你的網址供應商那邊將網址使用`CNAME`
的方式綁定至`account.github.io`。
{% highlight bash %}
~/my-awesome-site$ vi CNAME

seans.tw
{% endhighlight %}

{% highlight bash %}
~/my-awesome-site$ git init
~/my-awesome-site$ git checkout -b gh-pages
~/my-awesome-site$ git add .
~/my-awesome-site$ git commit -m 'init git'

~/my-awesome-site$ git remote add origin https://github.com/account/account.github.io.git
~/my-awesome-site$ git push -u origin gh-pages
{% endhighlight %}

當你push到Github上的時候，就可以直接使用網址看到你的Blog囉！

<br>

## 如何新增文章

在Jekyll中，資料夾架構如下：

{% highlight bash %}
my-awesome-site
|- _drafts     草稿資料夾
|- _layouts    模版資料夾
|- _plugins    外掛資料夾
|- _posts      文章資料夾
|- _site       靜態網頁轉換存放處
_config.yaml   網站設定檔
{% endhighlight %}

基本上新增文章很簡單，只要新增檔案至`_posts`就可以了，不過必須遵守`年-月-份-文章網址名稱.md`的檔名規範才行，不然會失效喔！

新增完畢後，必須要在檔案最上面設定你的文章設定，如下：


{% highlight bash %}
---
title: 利用Jekyll與Github Page建立自己的Dev-Blog
categories: Github
tags:
- jekyll
- hexo
---
想建立一個自己的Dev Blog嗎？有些人一直想要創一個屬於自己的開發者Blog，但是卻沒有經費去申請網頁主機，或是覺得Wordpress門檻太高，安裝步驟太繁雜。其實有更好的選擇，你可以使用Jekyll或hexo之類的平台.....
{% endhighlight %}

當然在文章中，標籤與分類並沒有強制要設定，只是在這邊做個範例，如果只有title也是可以順利啟動成功的喔！

最後新增完文章記得部屬到Github上，這樣網站上面才會更新哦！

{% highlight bash %}
~/my-awesome-site$ git add .
~/my-awesome-site$ git commit -m 'add new post'
~/my-awesome-site$ git push -u origin [gh-pages|master]
{% endhighlight %}

恭喜你擁有自己的Dev Blog囉！

<br>

### 相關資料
- [Jekyll 官方網站](https://jekyllrb.com)
- [Jekyll Theme](http://jekyllthemes.org)
- [hexo](https://hexo.io)