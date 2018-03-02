---
layout: post
title:  "在 Ubuntu 安裝 OpenCV 與建置 C++ 編譯環境"
date:   2017-12-25 19:58:18 +0800
categories: opencv installation
---
跟著以下步驟, 將指令複製貼上執行, 就可以在你的 Ubuntu 上建置編譯 OpenCV C/C++ 的環境

> 注意： 這只適用在 Linux Debian 以及 MacOS 系統上，Windows 不適用哦!

#### 1. 更新系統
{: style="color:#3498DB;"}

{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

<br/>

#### 2. 下載 OpenCV 的相依套件
{: style="color:#3498DB;"}

{% highlight bash %}
sudo apt-get install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python3.5-dev python3-numpy libtbb2 libtbb-dev
sudo apt-get install libjpeg-dev libpng-dev libtiff5-dev libjasper-dev libdc1394-22-dev libeigen3-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev sphinx-common libtbb-dev yasm libfaac-dev libopencore-amrnb-dev libopencore-amrwb-dev libopenexr-dev libgstreamer-plugins-base1.0-dev libavutil-dev libavfilter-dev libavresample-dev
{% endhighlight %}

<br/>

#### 3. 下載 OpenCV
{: style="color:#3498DB;"}

{% highlight bash %}
sudo su
cd /opt
git clone https://github.com/Itseez/opencv.git
git clone https://github.com/Itseez/opencv_contrib.git
{% endhighlight %}

<br/>

#### 4. 編譯與安裝 OpenCV
{: style="color:#3498DB;"}

因為要編譯 OpenCV 以及建置環境, 所以這個環節需要一點時間

{% highlight bash %}
cd opencv
mkdir release
cd release
cmake -D BUILD_TIFF=ON -D WITH_CUDA=OFF -D ENABLE_AVX=OFF -D WITH_OPENGL=OFF -D WITH_OPENCL=OFF -D WITH_IPP=OFF -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_EIGEN=OFF -D WITH_V4L=OFF -D WITH_VTK=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=/opt/opencv_contrib/modules /opt/opencv/
make -j4
make install
ldconfig
exit
cd ~
{% endhighlight %}

<br/>

#### 5. 查看你的 OpenCV 版本
{: style="color:#3498DB;"}

{% highlight bash %}
pkg-config --modversion opencv
{% endhighlight %}

<br/>

#### 6. 寫個簡單的程式來編譯看看
{: style="color:#3498DB;"}

{% highlight cpp %}
#include "opencv2/opencv.hpp"

using namespace cv;

int main(void){
    Mat srcImg = imread("picture.jpg");
    
    imshow("srcImg", srcImg);
    waitKey(0);
    
    return 0;
}
{% endhighlight %}

- 編譯與執行

{% highlight bash %}
# 編譯
g++ main.cpp -o output `pkg-config --cflags --libs opencv`

# 執行
./output
{% endhighlight %}

> 上面的 main.cpp 請改成你自己程式的名稱

<br/>

#### Reference
{: style="color:#3498DB;"}

- [How to Install OpenCV in Ubuntu 16.04 LTS for C / C++](http://www.codebind.com/cpp-tutorial/install-opencv-ubuntu-cpp/)

<br/>

---
