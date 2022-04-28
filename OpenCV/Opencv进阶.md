# Opencv进阶

### 头文件

```c++
#include<opencv2/imgcodecs.hpp>
#include<opencv2/highgui.hpp>
#include<opencv2/imgproc.hpp>
#include<opencv2/objdetect.hpp>
#include<iostream>
```

## 案例一

### .read()

功能：按帧读取视频，配合着waitKey()函数使用

```c++
void main(){
	string path = "Video/vtest.avi";
	VideoCapture cap(path);
	Mat img;
	while (true)
	{
		cap.read(img);
		imshow("Image", img);
		int c = waitKey(20);//20是帧数
		if (c == 27) {
			break;
		}
	}
}
```

## 案例二

### 高斯模糊

​		高斯模糊能够把某一高斯曲线周围的像素色值统计起来，采用数学上加权平均的计算方法得到这条曲线的色值，最后能够留下人物的轮廓，即曲线

#### GaussianBlur()

**功能**：一般用于图像预处理，对图像进行高斯滤波，去除噪声

**原型**：

```c++
void cv::GaussianBlur(InputArray scr, OutputArray dst, Size ksize, double sigmaX, double sigmaY, int borderType);
```

**参数**：

*InputArray scr*：输入的图像

*OutputArray dst*：输出图像

*Size ksize*：高斯卷积核的大小，是奇数

*double sigmaX, double sigmaY*：表示x和y方向的方差，如果y=0则y方向的方差与x相等

*int borderType*：边界的处理方式，一般默认（=BORDER_DEFAULT）

#### Canny()

```c++

```

#### getStructuringElement()

```c++

```

#### dilate()

```c++

```

#### erode()

```c++

```

```c++
void main() {

	string path = "Picture/face03.png";
	Mat img = imread(path);
	Mat imgGray;
	Mat imgBlur;
	Mat imgCanny;
	Mat imgDil;
	Mat imgErode;

	cvtColor(img, imgGray, COLOR_BGR2GRAY);
	GaussianBlur(imgGray, imgBlur, Size(3, 3), 3, 0);//高斯模糊
	Canny(imgBlur, imgCanny, 25, 75);

	Mat kernel = getStructuringElement(MORPH_RECT, Size(5, 5));//范围（扩张）/（腐蚀）
	dilate(imgCanny, imgDil, kernel);//扩张
	erode(imgDil, imgErode, kernel);//腐蚀

	imshow("Image", img);
	imshow("Image Gray", imgGray);
	imshow("Image Blur", imgBlur);
	imshow("Image Canny", imgCanny);
	imshow("Image Dilation", imgDil);
	imshow("Image Erode", imgErode);
	waitKey(0);

}
```



```c++
void main() {

	string path = "Picture/face12.jpg";
	Mat img = imread(path);

	Mat imgResize;
	Mat imgCrop;

	cout << img.size() << endl;
	resize(img, imgResize, Size(0,0), 0.5, 0.5);

	Rect roi(200, 100, 300, 250);
	imgCrop = img(roi);

	imshow("Image", img);
	imshow("Image Resize", imgResize);
	imshow("Image Crop", imgCrop);
	
	waitKey(0);
}
```



#### circle()

```

```

#### rectangle()

```

```

#### line()

```

```

#### putText()

```

```

```c++
void main() {
	string path = "Picture/face12.jpg";
	Mat img(512, 512, CV_8UC3, Scalar(255, 255, 255));
    
	circle(img, Point(256, 256), 155, Scalar(0, 69, 255), FILLED);
	rectangle(img, Point(130, 226), Point(382, 286), Scalar(255, 255, 255), FILLED);
	line(img, Point(130, 296), Point(382, 296), Scalar(255, 255, 255), 2);
	putText(img, "Murtaza's Workshop", Point(137, 262), FONT_HERSHEY_DUPLEX, 0.75, Scalar(0, 69, 255), 2);
	imshow("Image", img);
	waitKey(0);
}
```

### 对 Point2f 的理解

Point_类不用多言，里面两个成员变量x，y。Point_<int>就是Point2i，也是Point，Point_<float>就是Point2f，Point_<double>就是Point2d

其实也可以理解Point_为结构体...所以srcPoint[3]可以理解为结构体数组，存储了三个这样的结构体

### warpPerspective()



```c++
#include<opencv2/opencv.hpp>
#include<iostream>
using namespace std;
using namespace cv;
 
float w = 255, h = 155;
Mat image, image_copy, matrix, imgWarp;
Point2f pt1[4] = { {0.0,0.0}, {0.0f,0.0f}, {0.0f,0.0f}, {0.0f,0.0f} }, pt2[4] = { {0.0f,0.0f},{w,0.0f}, {0.0f,h},{w,h} };
string winName = "Image";

static void onMouse(int event, int x, int y, int flags, void* userdata) {
	static int i = 0;
	switch (event)
	{
		case EVENT_LBUTTONDOWN://左击
		{
			pt1[i] = Point(x, y);
			circle(image_copy, pt1[i], 1, Scalar(0, 0, 255), 10);
			i++;
			cout << i << endl;
			if (i == 4) {
				matrix = getPerspectiveTransform(pt1, pt2);
				warpPerspective(image_copy, imgWarp, matrix, Size(w, h));
				imshow("Image Warp", imgWarp);
				break;
			}	
		}
	}
	imshow(winName, image_copy);
}
int main() {
	system("color 2F");

	string path = "Picture/020.jpg";
	image= imread(path);
	image.copyTo(image_copy);

	
	namedWindow(winName, WINDOW_AUTOSIZE);
	imshow(winName, image);

	setMouseCallback(winName, onMouse, (void*)(&image));

	
	waitKey(0);
	return 0;

}
```

### 图像形状识别

```c++
#include<opencv2/imgcodecs.hpp>
#include<opencv2/highgui.hpp>
#include<opencv2/imgproc.hpp>
#include<opencv2/objdetect.hpp>
#include<iostream>
#include<string>
using namespace cv;
using namespace std;



void getContours(Mat imgDil, Mat img) {
	
	vector<vector<Point>>	contours;
	vector<Vec4i>	hierarchy;

	findContours(imgDil, contours, hierarchy, RETR_EXTERNAL, CHAIN_APPROX_SIMPLE);
	//drawContours(img, contours, -1, Scalar(0, 255, 0), 10);

	for (int i = 0; i < contours.size(); i++) {
		int area = contourArea(contours[i]);
		cout << area << endl;

		vector<vector<Point>>	conPoly(contours.size());
		vector<Rect> boundRect(contours.size());
		string objectType;


		if (area > 1000) {
			float peri = arcLength(contours[i], true);
			approxPolyDP(contours[i], conPoly[i], 0.02 * peri, true);
			drawContours(img, conPoly, i, Scalar(255, 0, 255), 2);
			cout << conPoly[i].size() << endl;
			boundRect[i] = boundingRect(conPoly[i]);
			//rectangle(img, boundRect[i].tl(), boundRect[i].br(), Scalar(0, 255, 0), 5);

			int objCor = (int)conPoly[i].size();

			if (objCor == 3) { objectType = "Tri"; }
			if(objCor ==  4) { 

				float aspRatio = (float)boundRect[i].width / (float)boundRect[i].height;
				cout << aspRatio << endl;
				if (aspRatio > 0.95 && aspRatio < 1.05) { objectType = "Square"; }
				else { objectType = "Rect"; }
				objectType = "Rect"; }
			if(objCor >4) { objectType = "Circle"; }

			drawContours(img, conPoly, i, Scalar(255, 0, 255), 2);
			rectangle(img, boundRect[i].tl(), boundRect[i].br(), Scalar(0, 255, 0), 5);
			putText(img, objectType, { boundRect[i].x, boundRect[i].y - 5 }, FONT_HERSHEY_DUPLEX, 0.75, Scalar(0, 69, 255),0.5);

		}
	}
}


int main() {
	string path = "Picture/几何图形.jpg";
	Mat image = imread(path);
	Mat imgGray, imgBlur, imgCanny, imgDil, imgErode;
	if (image.empty()) {
		cout << "ERROR" << endl;
		return 1;
	}

	cvtColor(image, imgGray, COLOR_BGR2GRAY);//获取灰度图
	GaussianBlur(imgGray, imgBlur, Size(3, 3), 3, 0);//高斯模糊
	Canny(imgBlur, imgCanny, 25, 75);//边缘
	Mat kernel = getStructuringElement(MORPH_RECT, Size(3, 3));
	dilate(imgCanny, imgDil, kernel);//膨胀

	getContours(imgDil, image);

	imshow("Image", image);
	/*imshow("Image Gray", imgGray);
	imshow("Image Blur", imgBlur);
	imshow("Image Canny", imgCanny);
	imshow("Image Dil", imgDil);*/
	waitKey(0);
	return 0;
}
```

**运行结果**

![image-20210531182749864](C:\Users\53421\AppData\Roaming\Typora\typora-user-images\image-20210531182749864.png)