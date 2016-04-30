---
title: '用eslint X Airbnb-rules改善程式碼品質'
categories: Nodejs
tags:
- eslint
- airbnb
- nodejs
- javascript
---

在專案合作上，程式碼品質一直是團隊中很重要的元素，每個人寫的風格都不一樣，在後續交接及維護都是非常大的門檻，那麼這時候就需要訂定規則來讓大家在撰寫程式碼時風格相同。

<br>

## 是時候用eslint了

在Eslint官方網站上[(這裡)](http://eslint.org)有非常詳細的教學，可以依照上面的步驟建立屬於自己的規則。eslint目前有支援javascript與jsx，意思是，當你撰寫React的時候也可以搭配eslint改善你的程式碼品質！這就夠神了吧！

<br>

## 搭配airbnb-javascript-rules

在2012/12/2，[hshoff](https://github.com/hshoff)在[airbnb/javascript](https://github.com/airbnb/javascript)上開啟了[第一個Commit](https://github.com/airbnb/javascript/commit/0fcdacc7f5f285c8eb7ad14943ac484d27b3ca55)。接著開始有一些javascript的[rules](https://github.com/airbnb/javascript/commit/cab510342f93791a7487d16258d06ff73edb4507)在上面，直到現在2016年，已經有非常完整(但是也很煩XD)的jsx與javascript-es6的[rules文件](https://github.com/airbnb/javascript)。

不過規則那麼多，基本上不可能將文件看完再開始coding，所以這時候就可以搭配eslint來協助檢查程式碼是否有違反airbnb上所規定的javascript-rules。

<br>

## 該怎麼使用？

因為每次開專案都會先被開發環境搞一輪，所以其實今天是要紀錄一下eslint的環境該怎麼建，不然每次都還要找舊的專案爬實在很累XDDDD(還有es6的環境也要一起裝啊........不過好險我有寫[es6教學](/2016/use-es6-es7-on-nodejs/))

<br>

## GOGOGOGOGOGO 開始安裝！

首先必須安裝eslint，安裝的方式可以使用全局安裝，不過開發上還是建議直接安裝在專案資料夾較佳。(環境是es6，所以下面流程會安裝到babel，請注意)

{% highlight bash %}
npm install eslint babel-eslint
{% endhighlight %}

接下來就是要安裝airbnb幫你做好的eslint規則，接下來要注意一點，你是否有需要開啟react的lint支援。不過怕安裝流程隨時都會變更，請記得安裝前要到airbnb/javascript的[頁面](https://github.com/airbnb/javascript/tree/master/packages)查看最新安裝流程。

<br>

### 1.如果想要react和es6+的eslint-rules

請直接使用[eslint-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)

{% highlight bash %}
npm install eslint-config-airbnb eslint-plugin-import eslint-plugin-react eslint-plugin-jsx-a11y eslint --save-dev
{% endhighlight %}

並在專案資料夾中建立`.eslintrc`設定檔，加入

{% highlight json %}
{
  "extends": "airbnb"
}
{% endhighlight %}

<br>

### 2.如果只想單純使用es6+的eslint-rules

請直接使用[eslint-airbnb-base](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base)

{% highlight bash %}
npm install eslint-config-airbnb-base eslint-plugin-import eslint --save-dev
{% endhighlight %}

並在專案資料夾中建立`.eslintrc`設定檔，加入

{% highlight json %}
{
  "extends": "airbnb-base"
}
{% endhighlight %}

<br>

### notes: 我的eslintrc設定檔 (for es6+)

這才是我發這篇的重點啊啊啊啊，每次為了環境建制被搞到快累死，所以寫了這篇方便之後複製，以下是我的`.eslintrc`設定檔：

{% highlight json %}
{
  "parser": "babel-eslint",
  "env": {
    "browser": true,
    "node": true
  },
  "extends": "airbnb-base",
  "rules": {
    "semi": [2, "never"],
    "arrow-body-style": ["error", "always"],
    "no-console": 0
  }
}
{% endhighlight %}

你可能會好奇為什麼我的設定檔中會有`rules`的設定，這部份就是可以讓你自訂你的eslint規則。在airbnb/javascript的官方文件中，規定是不允許使用`console`的指令。但是對我在開發上來說，我可能需要`console`來協助我，所以這時候就可以將這個規則關掉，當你使用`console`的時候就不會跑出警告囉！

<br>

## 開始debug我的程式碼吧！

當環境建好後，只要在專案資料夾中執行

{% highlight bash %}
    ./node_modules/.bin/eslint file1.js
{% endhighlight %}

就可以幫你執行eslint囉，如果開發習慣有點稍微不好的大概會像[這個樣子](https://www.google.com.tw/search?q=eslint+hall&client=safari&rls=en&source=lnms&tbm=isch&sa=X&ved=0ahUKEwidnenK4LXMAhWGm5QKHc1JCBoQ_AUIBygB&biw=1440&bih=789#imgrc=IyN--DS9LBBEdM%3A)....

{% highlight bash %}
   1:1   error    Unexpected var, use let or const instead                     no-var
   2:1   error    Unexpected var, use let or const instead                     no-var
   3:1   error    Unexpected var, use let or const instead                     no-var
   4:1   error    Unexpected var, use let or const instead                     no-var
   6:1   error    Unexpected var, use let or const instead                     no-var
   7:1   error    Unexpected var, use let or const instead                     no-var
   7:15  error    A constructor name should not start with a lowercase letter  new-cap
   8:1   error    Unexpected var, use let or const instead                     no-var
   9:1   error    Unexpected var, use let or const instead                     no-var
  14:9   error    Strings must use singlequote                                 quotes
  14:14  warning  Missing function expression name                             func-names
  14:14  error    Unexpected function expression                               prefer-arrow-callback
  14:22  error    Missing space before function parentheses                    space-before-function-paren
  15:16  error    Unexpected string concatenation                              prefer-template
  18:18  warning  Missing function expression name                             func-names
  18:18  error    Unexpected function expression                               prefer-arrow-callback
  18:26  error    Missing space before function parentheses                    space-before-function-paren
  20:5   warning  Unexpected console statement                                 no-console
  22:5   warning  Unexpected console statement                                 no-console
  22:18  error    Strings must use singlequote                                 quotes

{% endhighlight %}

這時候，就認命一點把程式碼的品質提升上來吧XDDDDDDDD

(備註：你也可以搭配sublimetext或是atom的eslint外掛，當儲存文件時自動執行eslint來檢測你的程式碼，直接把有問題的部分高亮起來，在這邊不多述)