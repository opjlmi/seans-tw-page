---
title: 如何編譯opencv3.0(含extra modules)
date: 2015-06-22 15:48:40
categories: RaspberryPi
tags:
- raspberry-pi
- opencv
- python
---
最近opencv終於正式升到3.0了，想說來嘗試把自己Pi2上面的opencv升級上去，不過在升級完之後一直有出現一些問題。想說要執行一些相關的套件都一直找不到，所以在這邊紀錄一下遇到的狀況。
<!-- more -->
## 下載opencv
請先到 [Itseez的Github Page](https://github.com/Itseez) 下載最新的`Opencv`與 `Opencv_contrib`程式碼。

```bash Terminal 
    git clone https://github.com/Itseez/opencv.git
    git clone https://github.com/Itseez/opencv_contrib.git
```

## 資料夾結構

為了說明方便，請在下指令的時候小心資料夾放置的位置，當你執行完上面兩行之後，資料夾結構應該是長這樣： (請注意後續執行指令時的資料夾位置)
```sh 資料夾結構
-執行指令的位置
└./opencv
  └./opencv/build <-這個等等才會建立
└./opencv_contrib
```

## 準備編譯

我編譯的環境就只是希望可以在python上面使用opencv3.0，如果你跟我相同目的，你可以照著我的步驟做。但如果你需要其他額外的設定可以直接到Opencv的[Github頁面](https://github.com/Itseez/opencv)查看。

```bash Terminal
    sudo apt-get install build-essential cmake libgtk2.0-dev pkg-config \
    python-dev python-numpy libavcodec-dev libavformat-dev libswscale-dev \
    libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22 git -y
```
<p></p>
```bash Terminal 
    cd opencv
    mkdir build
    cd build
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D BUILD_ZLIB=ON -D WITH_V4L=ON -D WITH_GSTREAMER=ON \
    -D WITH_OPENEXR=ON -D WITH_UNICAP=ON -D BUILD_PYTHON_SUPPORT=ON \
    -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
    -D DBUILD_PERF_TESTS=OFF ..
```

**請注意cmake的那行指令** ，我下指令的時機已經在`opencv/build`資料夾裡面，所以`OPENCV_EXTRA_MODULES_PATH`參數才會需要往上兩層。當你 `cmake` 指令執行完畢後，請繼續執行：

```bash Terminal 
    make
    sudo make install
```

如果你是使用Pi2，記得下`make`指令時加上`-j4`的參數，這樣子編譯速度會快很多。

## 確認你的OpenCV版本

我在做第一次編譯的時候，有遇到opencv沒有升級的狀況，所以順便在這邊作個查版本的筆記。

```bash Terminal 
    pi@seanspi $ python

    Python 2.7.3 (default, Mar 18 2014, 05:13:23)
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import cv2
    >>> cv2.__version__
    '3.0.0-dev'
```

當你看到3.0.0代表你就升級成功了，不過你不覺得為什麼在第一步編譯時要另外加裝`opencv_contrib`呢？原因是因為我當初是在練習 [這篇](http://www.makezine.com.tw/make2599131456/raspberry-pi10) 的程式碼，但是很奇怪的問題是，每次執行程式碼是總是會在`cv2.createEigenFaceRecognizer()`出錯，就算更新到3.0還是會有一樣的問題。

原因是因為到了3.0版之後，`createEigenFaceRecognizer`已經移動到`cv2.face`底下，所以之後當你需要呼叫相關函數的時候，必須使用：

```bash Python
    cv2.face.createEigenFaceRecognizer()
```

相關opencv3.0的API教學，可以到[這邊](http://docs.opencv.org/3.0-beta/modules/refman.html)查看。