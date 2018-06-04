---
layout: post
title:  "[OpenCV] 臉部偵測 (FaceDetection)"
date:   2017-11-20 19:58:18 +0800
categories: opencv
comments: true
---
#### 我的實作環境
{: style="color:#3498DB;"}

- **作業系統：** Lubuntu 16.04
- **OpenCV 函式庫版本：** `Version 3.3`
- **程式語言：** C++

> 在作業系統方面不用擔心, 因為 OpenCV 是一個**跨平台**的函數庫

<br/>

#### 先建立檔案
{: style="color:#3498DB;"}

先依序建立以下幾個檔案： 

{% highlight bash %}
mkdir FaceDetection
cd FaceDetection
mkdir Source
touch FaceDetection.cpp
{% endhighlight %}

<br/>

#### 下載人臉及人眼分類器
{: style="color:#3498DB;"}

先進到 Source 目錄中

{% highlight bash %}
cd Source
{% endhighlight %}

下載人臉及人眼分類器

{% highlight bash %}
# 取得人眼分類器
wget https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_eye_tree_eyeglasses.xml

# 取得人臉分類器
wget https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_frontalface_default.xml
{% endhighlight %}

> XML 是可以自行定義各標籤名稱的標記式語言, 透過標記式語言, 電腦之間便可以傳輸各種資訊
> 所以當我們 Load 上面兩個 XML 進程式之後, 就可以使用他們的演算法來找出人臉及眼睛

<br/>

#### 開始撰寫臉部偵測
{: style="color:#3498DB;"}

{% highlight cpp %}
/** FaceDetection.cpp **/
#include <opencv2/opencv.hpp>
#include <iostream>
#include <stdio.h>

using namespace std;
using namespace cv;

/** Print error message **/
void PANIC(char *msg);
#define PANIC(msg){perror(msg); exit(-1);}

/** Function for face detection **/
void DetectAndDraw(Mat frame);

/** Global variables **/
String face_cascade_name = "Source/haarcascade_frontalface_default.xml";
String eyes_cascade_name = "Source/haarcascade_eye_tree_eyeglasses.xml";
CascadeClassifier face_cascade; // Declare the face classifier
CascadeClassifier eyes_cascade; // Declare the eyes classifier
String window_name = "Face detection";

int main(int argc, char *argv[]){
	/* Open the web camera */
	VideoCapture capture = VideoCapture(0);
	Mat frame, image;
	
	/** Load cascade classifiers **/
	if(!face_cascade.load(face_cascade_name))
		PANIC("Error loading face cascade");
	if(!eyes_cascade.load(eyes_cascade_name))
		PANIC("Error loading eyes cascade");
	
	/** After the camera is opened **/
	if(capture.isOpened()){
		cout<<"Face Detection Started..."<<endl;

		for(;;){
			/* Get image from camera */
			capture>>frame; 			
			if(frame.empty())
				PANIC("Error capture frame");
			
			/* Start the face detection function */
			DetectAndDraw(frame);
			
			/** If you press ESC, q, or Q , the process will end **/
			char ch = (char)waitKey(10);
			if(ch==27 || ch=='q' || ch=='Q')
				break;
		}
	}
	else
		PANIC("Error open camera");
	
	return 0;
}

void DetectAndDraw(Mat frame){
    /* Declare vector for faces and eyes */
    std::vector<Rect> faces, eyes;
    Mat frame_gray, frame_resize;
    int radius;
	
    /* Convert to gray scale */
    cvtColor(frame, frame_gray, COLOR_BGR2GRAY);
    //imshow("grayscale", frame_gray);
	
    /* Resize the grayscale Image */
    //resize(frame_gray, frame_resize, Size(), 1, 1, INTER_LINEAR);
    //imshow("resize", frame_resize);
	
    /* Histogram equalization */
    equalizeHist(frame_gray, frame_gray);
    //imshow("equalize", frame_gray);
	
    /* Detect faces of different sizes using cascade classifier */
    face_cascade.detectMultiScale(frame_gray, faces, 1.1, 5, CV_HAAR_SCALE_IMAGE, Size(30, 30));
	
    /** Draw circles around the faces **/
    for (size_t i = 0; i < faces.size(); i++)
    {
        Point center;
 
        /* Draw rectangular on face */
        rectangle(frame, faces[i], Scalar(255, 0, 0), 3, 8, 0);

        Mat faceROI = frame_gray(faces[i]);

        /* Detection of eyes int the input image */
        eyes_cascade.detectMultiScale(faceROI, eyes, 1.1, 1, CV_HAAR_SCALE_IMAGE, Size(3, 3)); 
         
        /** Draw circles around eyes **/
        for (size_t j = 0; j < eyes.size(); j++) 
        {
            center.x = cvRound((faces[i].x + eyes[j].x + eyes[j].width*0.5));
            center.y = cvRound((faces[i].y + eyes[j].y + eyes[j].height*0.5));
            radius = cvRound((eyes[j].width + eyes[j].height)*0.25);
            circle(frame, center, radius, Scalar(0, 255, 0), 3, 8, 0);
        }
    }
 
    // Show Processed Image with detected faces
    imshow( "Face Detection", frame);
}
{% endhighlight %}

<br/>

#### 編譯與執行
{: style="color:#3498DB;"}

{% highlight bash %}
## 編譯
g++ FaceDetection.cpp -o output `pkg-config --cflags --libs opencv`

# 執行
./output
{% endhighlight %}

執行後將你的臉對準攝相頭, 應該會看到你的臉及眼睛被框出來

> 如果你還沒安裝 OpenCV 可以參考[我的文章](https://wenyuangg.github.io/opencv/installation/2017/12/25/opencv-installation.html)來安裝

<br/>

#### 解釋程式
{: style="color:#3498DB;"}

其實過程很簡單, 大致分為以下幾個步驟： 

1. 將人臉和人眼分類器 Load 進來
2. 使用 `cvtColor()` 將影像轉換成`灰階`
3. 使用 `resize()` 更改影像大小比率, 視情況而定, 我們在這裡先不用
4. 使用 `equalizeHist()` 將影像做直方圖等化, 以增強對比度
5. 運用 XML 內的分類器演算法來偵測人臉和人眼
6. 分別框出臉和眼睛
7. 完成臉部偵測

<br/>

---
