---
title: 使用Octoprint和webcam監控控制3Dprinter
date: 2015-01-15 08:33:33
categories: RaspberryPi
tags:
- raspberry-pi
- arduino
- octoprint
- 3dprinter
- prusa-i3
- webcam
- raspberry-pi-camera-module
---
近期開始玩目前最夯的3D印表機，不過其實覺大部份的Opensoure的3D印表機(像是Prusai3)，要另外使用遠端控制的話，就要在另外去弄外接的東西去控制。其實網路上遠端控制3D印表機的資料其實很多，不過為了我臨時又需要看資料，那我就整理在這邊好了。
**\*\*本文也有提供Raspberry Camera Module的教學\*\***

<br>

##首先你需要準備......
1. 一台使用Arduino控制的3D印表機(像是Prusai3)
2. Raspberry pi
3. 一台Webcam (Raspberry Camera Module也可)

<br>

##行前工作
Octoprint是目前網路上可以提供控制3Dprinter的OpenSource專案，目前我只有使用Raspberry pi測試過沒問題，如果有人是使用BananaPi或是Cubieboard安裝有成功的話，歡迎在下面留言告知。
**\*\*以下所有指令都需在Raspberry pi下執行。\*\***

(ps.本文並不會介紹設定Raspberry pi的部份)


<br>

##開始！

<br>

###Raspberry Pi 環境建置
首先我們先把Raspberry pi的基本東西先安裝起來，且去下載並安裝OctoPrint。
{% highlight bash %}
cd ~
sudo apt-get install python-pip python-dev git
sudo apt-get install python-setuptools
git clone https://github.com/foosel/OctoPrint.git
cd OctoPrint
sudo python setup.py install
mkdir ~/.octoprint
{% endhighlight %}

再把pi使用者加入`tty`以及`dialout`群組。加入dialout群組的原因，是因為這樣子pi使用者才會有控制Arduino板的權利。

{% highlight bash %}
sudo usermod -a -G tty pi
sudo usermod -a -G dialout pi
{% endhighlight %}

接下來你就可以在pi使用者下執行 `octoprint` ，就可以把Octorprint啟動了。執行完指令，就可以使用相同區網的電腦連線至 `http://<yourdecive-ip>:5000/` ，你就會看到Octorprint的控制畫面了。

ps.相關Octorprint與Arduino設定不在這邊多做說明。

<br>

###Webcam環境建制

RaspberryPi在網路攝影機的部份都已經幫你設定好，不過你之前如果沒有啟動功能，你的Webcam可能會驅動不成功。所以你必須執行 `sudo raspi-config` 進入RaspberryPi的設定介面，必且選擇 `Enable Camera` 將Webcam的功能打開，存檔並重新開機。接下來請執行下面的指令：

{% highlight bash %}
cd ~
sudo apt-get install subversion libjpeg8-dev imagemagick libav-tools cmake
git clone https://github.com/jacksonliam/mjpg-streamer.git
cd mjpg-streamer/mjpg-streamer-experimental
make
sudo make install
{% endhighlight %}

接下來要先測試Webcam有無設定錯誤，請輸入：

{% highlight bash %}
sudo ./mjpg_streamer -o "./output_http.so -w ./www" -i "./input_uvc.so -fps 30" 
{% endhighlight %}

<br>

**如果你是用Raspberry Camera Module**

{% highlight bash %}
sudo ./mjpg_streamer -o "./output_http.so -w ./www" -i "./input_raspicam.so -fps 30 -x 480 -y 320 -ex night" 
{% endhighlight %}

照正常來說，你應該是可以使用`http://<yourdecive-ip>:8080/`進入查看Webcam設定，如果正常可以看到，就可以進行下一步了。

<br>

##整合設定

接下來就需要去Octoprint裡面設定，將Webcam正常啟用。編輯 `.octoprint/config.yaml` 裡面，修改設定如下：
{% highlight yaml %}
webcam:
ffmpeg: /usr/bin/avconv
snapshot: http://127.0.0.1:8080/?action=snapshot
stream: http://`Your\_Raspi's\_IP`:8080/?action=stream
{% endhighlight %}

<br>

##設定自動啟動

自動啟動的部份有分兩種，一種是開機自動啟動，一種是開完機在下指令，這兩種方式我都會作介紹。

<br>

###1.開完機再下指令

首先請先在家目錄建立一個 `octoprint_start.sh` 檔案，內文如下：
**\*\*如果你是用Raspberry Camera Module，請直接替換指令即可。\*\***
{% highlight bash %}
#!/bin/sh
OCTOPRINT_HOME=/home/`User_id`/`Your_Octoprint_Home`
MJPEG\_STREAMER\_HOME=/home/`User_id`/`Your_mjpg-streamer_Home`

# start mjpeg streamer
sudo $MJPEG_STREAMER_HOME/mjpg_streamer -i "$MJPEG_STREAMER_HOME/input_uvc.so -fps 30" -o "$MJPEG_STREAMER_HOME/output_http.so "&

# start the webui
/usr/local/bin/octoprint
{% endhighlight %}

當你建立完畢後，可以執行 `sh ./octoprint_start.sh` ，正常來說應該可以正常啟動Webcam與Octoprint了。 

<br>

###2.開機自動啟動指令###

當你建立 `octoprint_start.sh` 完畢後，只需要執行 `sudo vi /etc/rc.local` ，並且在最後一行 `exit 0` 前面加入以下程式碼：

{% highlight bash %}
sudo -u pi sh ~pi/octoprint_start.sh
{% endhighlight %}

這樣子設定後，之後開機都會直接執行Octoprint並且幫你把Webcam的功能打開。不過有一個缺點，就是當你執行後，他會卡在Octoprint的裡面。不過當然你還是可以使用ssh連線進去，還是一樣可以下指令的。
