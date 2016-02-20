---
title: nodejs冷知識
categories: Nodejs
tags:
- nodejs
- trivia
---

即日起開一篇『nodejs冷知識』，把一些短篇的除雷文都放在這，以免我每次都發一篇廢文然後裡面只有幾句話。歡迎各位有任何冷知識可以提供給我做紀錄啊XDDDDDDDDDDDDDDD

<br>

## 歡迎來到nodejs冷知識

<br>   

- npm在顯示Global List時，可以不要把相依套件都顯示出來嗎？

可以，使用`npm list -g --depth=0`除雷即可。

<br>

- nvm安裝新版的時候可以把舊版套件一併重新安裝嗎？

可以，使用`nvm install v5.6 --reinstall-packages-from=5.5`除雷即可。