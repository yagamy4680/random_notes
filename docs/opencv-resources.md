[[_TOC_]]

### Installation

http://hanmajor.blogspot.tw/2013/10/linux-ubuntu-opencv.html

```bash
# Prerequisitions
sudo apt-get install -y \
	build-essential \
	libgtk2.0-dev \
	libjpeg-dev \
	libtiff4-dev \
	libjasper-dev \
	libopenexr-dev \
	cmake \
	python-dev \
	python-numpy \
	python-tk \
	libtbb-dev \
	libeigen2-dev \
	yasm \
	libfaac-dev \
	libopencore-amrnb-dev \
	libopencore-amrwb-dev \
	libtheora-dev \
	libvorbis-dev \
	libxvidcore-dev \
	libx264-dev \
	libqt4-dev \
	libqt4-opengl-dev \
	sphinx-common \
	texlive-latex-extra \
	libv4l-dev \
	libdc1394-22-dev \
	libavcodec-dev \
	libavformat-dev \
	libswscale-dev

# Download OpenCV 2.4.8
wget https://github.com/Itseez/opencv/archive/2.4.8.tar.gz
tar xvf 2.4.8.tar.gz
cd opencv-2.4.8
mkdir build
cd ./build

# Configure OpenCV
cmake \
  -D WITH_TBB=ON \
  -D BUILD_NEW_PYTHON_SUPPORT=ON \
  -D WITH_V4L=ON \
  -D INSTALL_C_EXAMPLES=ON \
  -D INSTALL_PYTHON_EXAMPLES=ON \
  -D BUILD_EXAMPLES=ON \
  -D WITH_QT=ON \
  -D WITH_OPENGL=ON ..

# Build and install
make
sudo make install

# Setup environment variables for header files and dynamic libraries
echo "/usr/local/lib" | sudo tee -a /etc/ld.so.conf.d/opencv.conf
sudo ldconfig
echo "PKG_CONFIG_PATH=\$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig" | sudo tee -a /etc/bash.bashrc
echo "export PKG_CONFIG_PATH" | sudo tee -a /etc/bash.bashrc

# Build example
cd ~/opencv-2.4.8/samples/c
./build_all.sh

# Run one example of face detection
./facedetect --cascade="/usr/local/share/OpenCV/haarcascades/haarcascade_frontalface_alt.xml" --scale=1.5 lena.jpg
```

Build OpenCV 2.4.8: 
https://github.com/jayrambhia/Install-OpenCV/blob/master/Ubuntu/2.4/opencv2_4_8.sh

### Installation with IPP

http://choorucode.com/2013/10/04/how-to-install-intel-ipp-on-ubuntu/

```bash
export IPPROOT=/opt/intel/composer_xe_2013.1.117/ipp
export LIBRARY_PATH=$LIBRARY_PATH:/opt/intel/composer_xe_2013.1.117/ipp/lib/intel64:/opt/intel/composer_xe_2013.1.117/compiler/lib/intel64
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/intel/composer_xe_2013.1.117/ipp/lib/intel64:/opt/intel/composer_xe_2013.1.117/compiler/lib/intel64

cmake \
	-D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D WITH_TBB=ON \
    -D BUILD_NEW_PYTHON_SUPPORT=ON \
    -D WITH_V4L=ON \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=ON \
    -D WITH_QT=ON \
    -D WITH_OPENGL=ON \
    -D WITH_IPP=ON \
    ..

export IPPROOT=/opt/intel/composer_xe_2013_sp1.0.061/ipp
export LIBRARY_PATH=$LIBRARY_PATH:/opt/intel/composer_xe_2013_sp1.0.061/compiler/lib/intel64:/opt/intel/composer_xe_2013_sp1.0.061/ipp/lib/intel64
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/intel/composer_xe_2013_sp1.0.061/compiler/lib/intel64:/opt/intel/composer_xe_2013_sp1.0.061/ipp/lib/intel64
```

### Iris Recognition

[Iris recognition](http://en.wikipedia.org/wiki/Iris_recognition) is an automated method of biometric identification that uses mathematical pattern-recognition techniques on video images of the irides of an individual's eyes, whose complex random patterns are unique and can be seen from some distance.
  - [JavaVIS](http://sourceforge.net/projects/javavis/)
  - [Project Iris](http://projectiris.co.uk/), iris detection developed in C++ and Qt (cross-platform). ![iris-detection](http://projectiris.co.uk/images/application-thumb.png)

[Video-based Automatic System for Iris Recognition](http://www.nist.gov/itl/iad/ig/vasir.cfm), [source codes](http://www.nist.gov/itl/iad/ig/upload/NIST_VASIR_Beta2-2_src-3.zip) can be downloaded.

To assess VASIR's performance and practical feasibility, the Iris Challenge Evaluation (ICE) dataset and the Multiple Biometric Grand Challenge (MBGC) dataset were employed, with results as described in the publications below
  - http://www.nist.gov/itl/iad/ig/mbgc.cfm
  - http://www.nist.gov/itl/iad/ig/ice.cfm

[Iris Recognition by Libor](http://www.csse.uwa.edu.au/~pk/studentprojects/libor/)
  - [paper](http://www.csse.uwa.edu.au/~pk/studentprojects/libor/LiborMasekThesis.pdf)
  - [source code](http://www.csse.uwa.edu.au/~pk/studentprojects/libor/sourcecode.html) (matlab)


#### Eye Detection

http://answers.opencv.org/question/12034/face-eyes-and-iris-detection/

A novel approach for accurate localisation of the pupils in real time is proposed in this paper: Timm and Barth. Accurate eye centre localisation by means of gradients. In Proceedings of the Int. Conference on Computer Theory and Applications (VISAPP), volume 1, pages 125-130, Algarve, Portugal, 2011. INSTICC. (also see youtube video at Accurate eye center localisation for low-cost eye tracking) There is also a github repository that implements this paper: C++ implementation And a blog article.
  - http://thume.ca/projects/2012/11/04/simple-accurate-eye-center-tracking-in-opencv/
  - https://github.com/trishume/eyeLike (yagamy: opencv based implementation)

You can use this paper also to locate the position of the eyes. Bolme, D.S. "Average of Synthetic Exact Filters", IEEE Conference on Computer Vision and Pattern Recognition, 2009. CVPR 2009. PDF, Source code
  - http://www.cs.colostate.edu/~bolme/publications/Bolme2009Asef.pdf
  - https://github.com/laoyang/ASEF (yagamy: very thin implementation, good for integration)





### Automotive CV

[Car Detect](https://github.com/skandhurkat/CarDetect)



### Performance Improvements

[Can i capture at 20 fps with C++ and Opencv?](http://stackoverflow.com/questions/17314360/can-i-capture-at-20-fps-with-c-and-opencv)
```c
// disable RGB conversion
cap.set(CV_CAP_PROP_CONVERT_RGB , false);
// increase FPS from webcam capture as much as possible
cap.set(CV_CAP_PROP_FPS , 60);
```

[How to Achieve 30 fps with BeagleBone Black, OpenCV, and Logitech C920 Webcam](http://blog.lemoneerlabs.com/3rdParty/Darling_BBB_30fps_DRAFT.html#Xlibjpeg-turbo), key point is to use [libjpg-turbo](http://libjpeg-turbo.virtualgl.org/) to replace libjpg because YUYV to RGB conversion is slow and no way to optimize, although MJPG to RGB is also slow but it can be optimized by libjpg-turbo.

[OpenCV: C++ and C performance comparison](http://stackoverflow.com/questions/11376368/opencv-c-and-c-performance-comparison/11376867)
- Resize you video frames to be smaller. many times, you can extract the information from a 200x300px image, instead of a 1024x768. The area of the first one is 10 times smaller.
- Use simpler operations instead of complicated ones. Use integers instead of floats. And never use `double` in a matrix or a `for` loop that executes thousands of times.
- Prefer grayscale obtained directly from native YUV.
- ARM processors have NEON. Learn and use it. It is powerfull! (Same as SSE on Intel processors)

### MJPEG streaming over HTTP for OpenCV

[Simple Python Motion Jpeg (mjpeg server) from webcam. Using: OpenCV,BaseHTTPServer](https://gist.github.com/n3wtron/4624820)

[Streaming OpenCV Output Through HTTP/Network with MJPEG](http://ariandy1.wordpress.com/2013/04/07/streaming-opencv-output-through-httpnetwork-with-mjpeg/)

- using mjpeg-streamer: `mjpeg_streamer -i "input_file.so -f /home/ariandy/mjpg" -o "output_http.so -w /usr/local/www"`
- using another native opencv app to output mjpeg file: `./bgsubtract2 /home/ariandy/mjpg/out.mjpg -bgs`

[MJPEG Server for Webcam in Python with OpenCV](http://hardsoftlucid.wordpress.com/2013/04/11/mjpeg-server-for-webcam-in-python-with-opencv/), one python file to achieve the goal.

### References

[CMVision from CMU](http://www.cs.cmu.edu/~jbruce/cmvision/), To create a simple, robust vision system suitable for real time robotics applications. The system aims to perform global low level color vision at video rates without the use of special purpose harware.

[OpenCV Python Tutorials](http://opencvpython.blogspot.tw/p/list-of-articles-in-this-blog.html)

- Welcome to OpenCV-Python !!!    ---- Introduction Page
- OpenCV - Software That Sees      ---- Brief Introduction about OpenCV
- Python - A Powerful Friend           ---- Brief Introduction about Python
- Install OpenCV in Windows for Python 
- Drawing Histogram in OpenCV-Python 
- Contour features  ---- A tutorial on all the functions related to contours
- Skeletonization using OpenCV-Python ---- A simple thinning algorithm
- Barcode Detection                      ---- A tutorial on how to find barcode
- Simple Digit Recognition OCR in OpenCV-Python ---- Shows use of kNN algorithm
- Sudoku Solver - Part 1 ---- Theory behind the sudoku solver
- Sudoku Solver - Part 2 ---- Finding Sudoku Border and corners
- Sudoku Solver - Some Common Questions 
- Difference between Matrix Arithmetic in OpenCV and Numpy 
- Image Derivatives and its Applications - Sobel, Scharr, Laplace and Canny
- Contours - 1: Getting Started
- Contours - 2 : Brotherhood
- Contours - 3 : Extraction
- Contours - 4 : Ultimate
- Fast Array Manipulation in Numpy



### Code Snippets

Manipulate pixels of given IplImage:

```c
IplImage* iplObj = cvCreateImage( cvSize( 800 , 600 ) , IPL_DEPTH_8U , 3 );
//---------------------------------------------------------
for (int y = 0; y < iplObj->height; y++)
	for (int x = 0; x < iplObj->width; x++)
	{
		int B = ((uchar *)(iplObj->imageData + y*iplObj->widthStep))[x*iplObj->nChannels + 0];
		int G = ((uchar *)(iplObj->imageData + y*iplObj->widthStep))[x*iplObj->nChannels + 1];
		int R = ((uchar *)(iplObj->imageData + y*iplObj->widthStep))[x*iplObj->nChannels + 2];
		//---------------------------------------------------------
		((uchar *)(iplObj->imageData + y*iplObj->widthStep))[x*iplObj->nChannels + 0] = B;
		((uchar *)(iplObj->imageData + y*iplObj->widthStep))[x*iplObj->nChannels + 1] = G;
		((uchar *)(iplObj->imageData + y*iplObj->widthStep))[x*iplObj->nChannels + 2] = R;
	}
//---------------------------------------------------------

cvNamedWindow( "WinName" , 1 );
cvShowImage( "WinName" , iplObj );
cvWaitKey(0);
```

[CvArr、Mat、CvMat、IplImage、BYTE转换（总结而来）](http://blog.csdn.net/wuxiaoyao12/article/details/7305848)

**CvMat -> IplImage**

```c++
cvGetImage(matI,img);
cvSaveImage("rice1.bmp",img);
```

**CvMat -> Mat** (与IplImage的转换类似，可以选择是否复制数据。)

```c++
Mat::Mat(const CvMat* m, bool copyData=false);
```

And, other snippets:

- IplImage -> Mat
- IplImage -> CvMat
- IplImage * -> BYTE * 


#### Vision

- [FREAK: FAST RETINA KEYPOINT](http://www.ivpe.com/freak.htm), A large number of vision applications rely on matching keypoints across images. The last decade featured an arms-race towards faster and more robust keypoints and association algorithms: SIFT, SURF, and more recently BRISK to name a few. These days, the deployment of vision algorithms on smart phones and embedded devices with low memory and computation complexity has even upped the ante: we need to make descriptors faster to compute, more compact while remaining robust to scale, rotation and noise.
- [Replacing SIFT with FREAK](http://answers.opencv.org/question/2346/replacing-sift-by-freak/)

FREAK : Fast Retina Keypoint
http://forum.biotrump.com/viewtopic.php?f=8&t=181&p=251#p251

It is believed that the human retina extracts details from images using Difference of Gaussians (DoG) of
various sizes and encodes such differences with action potentials.
The topology of the retina plays an important role.
We propose to mimic the same strategy to design our image descriptor.
http://www.nature.com/nature/journal/v467/n7316/full/nature09424.html
Functional connectivity in the retina at the resolution of photoreceptors

github
https://github.com/kikohs/freak.git
openCV FREAK
http://docs.opencv.org/modules/features2d/doc/feature_detection_and_description.html#id5
home page : Alexandre Alahi, Ph.D.
http://www.ivpe.com/freak.htm
paper
http://www.ivpe.com/papers/freak.pdf


Replacing SIFT by FREAK
http://answers.opencv.org/question/2346/replacing-sift-by-freak/

Yes. You just need to detect keypoints with FAST and call FREAK like this:

FREAK extractor;
extractor.compute(image, keypoints, descriptors);

https://github.com/kikohs/freak/blob/master/demo/freak_demo.cpp

### dlib

[Python Stuff and Real-Time Video Object Tracking](http://blog.dlib.net/2015/02/dlib-1813-released.html)

[Real-Time Face Pose Estimation](http://blog.dlib.net/2014/08/real-time-face-pose-estimation.html)
![](http://1.bp.blogspot.com/-FtyIjfFokzQ/U__h1sAoEEI/AAAAAAAAAR0/URuVhX9cR-E/s1600/landmarked_face2.png)

### Articles

[Sliding Windows for Object Detection with Python and OpenCV](https://www.pyimagesearch.com/2015/03/23/sliding-windows-for-object-detection-with-python-and-opencv/)

![](http://www.pyimagesearch.com/wp-content/uploads/2015/03/sliding-window-animated-adrian.gif)