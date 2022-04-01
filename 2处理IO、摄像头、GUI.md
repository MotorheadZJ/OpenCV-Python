> OpenCV的I/O功能,项目的概念,面向对象的设计
* 从图像文件、视频文件、摄像头设备或内存中的原始数据字节中读取图像
* 将图像写入图像文件或视频文件
* Numpy数组处理图像数据
* 窗口中显示图像
* 处理键盘和鼠标输入
* 实现基于面向对象设计的应用程序
## 基本I/O脚本
### 读取/写入图像文件
* 图像的结构    
将图像存储为一个多维数组,每个像素对应一个值,不同类型的图像数据可以使用不同格式的像素值
  ```python
  import numpy
  # 创建一个二位Numpy数组3*3的黑色正方形
  # 每个像素都使用一个8位整数表示,范围从0-255,0表示黑色,255表示白色,中间值表示灰色
  img = numpy.zeros((3,3),dtype = numpy.uint8)
  print(img)
  ```
  使用cv2.cvtColor函数将灰度图转换成BGR格式       
  ```python
  # 这里可以看到每个像素使用一个三元数组表示,每个整数代表一个颜色通道(BGR)
  import numpy,cv2
  img = numpy.zeros((3,3),dtype = numpy.uint8)
  img = cv2.cvtColor(img,cv2.COLOR_GRAY2BGR)
  print(img)
  # 可以通过shape属性来查看图像的结构
  print(img.shape)
  ```
* 转换图像格式(后缀)
  ```python
  import cv2
  # imread()读取图像  试着打印img 看看是如何读取图像的
  img = cv2.imread("1.jpg") # 这里自己下载一张图片 将这张图片和代码文件放在同一个目录下 命名为1.jpg
  # imwrite()写入图像 试着解释一下两个参数都是什么意思
  cv2.imwrite("2.png",img)
  ```
* imread(filename,flags)        
filename-文件的绝对路径+文件名(如果已经在文件在当前工作目录下不需要路径)     
flags-告诉Python如何读取文件      
3通道BGR就是蓝、绿、红三种颜色,每个占一个通道,有的图像还有其他的通道,比如透明度,也都单独占一个通道      

    |模式|作用|  
    |:-:|:-:|  
    |cv2.IMREAD_COLOR|默认选项,提供3通道BGR图像,每个通道是一个8位(0~255)|
    |cv2.IMREAD_GRAYSCALE|8位灰度图像|
    |cv2.IMREAD_ANYCOLOR|8位BGR图像或8位灰度图像,看文件中的元数据|
    |cv2.IMREAD_UNCHANGED|读取所有的图像数据,包括透明度通道或者|
    |cv2.IMREAD_ANYDEPTH|加载原始位深度的灰度图像,提供每个通道16位的灰度图|
    |cv2.IMREAD_ANYDEPTH\|cv2.IMREAD_ANYCOLOR|加载原始位深度的BGR彩色图像|
    |cv2.IMREAD_REDUCED_GRAYSCALE_2|得到的灰度图像分辨率为原始图像分辨率的一半|
    |cv2.IMREAD_REDUCED_COLOR_2|得到的彩色图像分辨率为原始图像分辨率的一半|         


  ```python
  import cv2
  img = cv2.imread("1.jpg",cv2.IMREAD_GRAYSCALE)
  cv2.imwrite("2.jpg",img)
  ```

* 图像与原始字节    
OpenCV中将图像存储为原始字节,就是numpy.array类型的二维或者三维的数组   
如果用坐标系的来表示的话,第一个索引就是y坐标或者行,第二个索引就是x坐标或者列,第三个坐标表示颜色(如果有的话)        
我们试着将一个包含随机字节的原始字节转换为灰度图像和BGR图像(将原始字节列表转换为二维和三维)     
这里我们可以看到,图像在计算机中就是二维或者三维的原始字节列表     
  ```python
  import cv2
  import numpy
  import os
     
  # os.urandom(size)生成一个适用于加密的size大小的字符串
  # bytearray生成一个原始字节列表
  randomByteArray = bytearray(os.urandom(120000)) # 生成一个包含12000字节的随机列表
  flatNumpyArray = numpy.array(randomByteArray)   # 将生成的随机原始字节列表转换为numpy.array
  # 直接使用numpy.random.randint()函数生成相同的原始字节列表
  grayImage = numpy.random.randint(0,256,120000).reshape(300,400)

  # 将这个原始字节列表重新组成二维列表 并保存成图像(灰度)
  grayImage = flatNumpyArray.reshape(300,400)
  cv2.imwrite("RandomGray.png",grayImage)

  # 将这个原始字节列表重新组成三维列表 并保存成图像(彩色)
  bgrImage = flatNumpyArray.reshape(100,400,3)
  cv2.imwrite("RandomBGR.png",bgrImage)
  ```

* 使用numpy.array访问图像数据
numpy.array相当于图像处理加强版的Python列表,我们知道图像其实就是一堆数据,那么我们就可以使用numpy.array来访问他们      

