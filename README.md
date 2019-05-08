# Environment-setup

Machine-learning environment setup on PC and Jetson development kit

# 硬件搭建注意事项

使用自带圆口的电源，需要插跳线帽。使用Micro-usb电源不加帽

启动若遇到卡在NVIDIA图标界面，可能是显示器分辨率问题，尝试使用HDMI线代替VGA转接头

无线网卡选取免驱动或支持LINUX的版本

## 烧写系统
烧写系统参考官方网站，注意windows系统烧写后会出现十几个很小的盘，而linux系统不会出现，建议使用linux pc烧写

1.下载官方镜像 https://developer.nvidia.com/embedded/dlc/jetson-nano-dev-kit-sd-card-image

2.格式化SD卡，使用SD Card Formatter格式化SD卡

3 用Etcher写入镜像，Etcher下载地址 https://www.balena.io/etcher/

4.插入TF卡并开机

![](https://developer.nvidia.com/sites/default/files/akamai/embedded/images/jetsonNano/gettingStarted/Jetbot_animation_500x282_2.gif)  

## 增加无线网卡

## 增加摄像头
![](http://images.ncnynl.com/ros/2019/JB3-Assy_12-3.JPG)

摄像头类型为：支持MIPI-CSI的摄像头，如树莓派v2摄像头或树莓派v2广角摄像头。Jetson官方文档指出，Jetson设备只支持IMX219方案的摄像头


## 远程登录
### 方法1 使用SSH命令行登录
使用ifconfig查看网卡信息并ping该地址

PC端安装Xshell客户端

主机端启动SSH服务
```Bash
/etc/init.d/sshd start
```
关于开机自启动该命令，参考https://blog.csdn.net/a912952381/article/details/81205095
和https://www.cnblogs.com/aiweixiao/p/5516974.html

![](https://github.com/jiaoly/PictureCache/blob/master/1.PNG)

### 方法2 使用vino图形界面登录

参考系统自带VINO说明

使用时启动/usr/lib/vino/vino-server即可

PC端安装VNCserver

连接
![](https://github.com/jiaoly/PictureCache/blob/master/2.PNG)

## 扩充swap交换空间内存

```Bash
#查看内存
free -h

# 先禁用以前的
sudo swapoff /swapfile
 
# 修改swap 空间的大小为4G
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096
 
# 设置文件为“swap file”类型
sudo mkswap /swapfile
 
# 启用swapfile
sudo swapon /swapfile

# 此swap会在系统reboot后就会消失，让其一直存在的方法如下：
sudo echo /swapfile none swap sw 0 0 >> /etc/fstab
```
可参考链接https://www.ncnynl.com/archives/201904/2920.html

## 系统备份和恢复

### 系统备份

* 从SD卡备份镜像到PC上
* 取出SD卡，放入SD卡套，插入ubuntu PC
* 执行如下命令：
```Bash
sudo dd if=/dev/mmc1kp0 of=nano-backup-xxxx-xx-xx.img
```
* 等待完成即可得到相应的镜像

### 系统恢复

* 选择etcher(Linux)或者win32DiskImage(Windows)来进行烧录恢复
* 步骤同系统烧录

# 软件环境搭建

尽量不要更换源，直接进行更新
```Bash
sudo apt-get update
sudo apt-get full-upgrade
```
## 检查已安装组件

Jetson-nano的OS镜像已经自带了JetPack，cuda，cudnn，opencv等都已经安装好，并有例子，这些例子安装路径如下所示

TensorRT	/usr/src/tensorrt/samples/
CUDA	/usr/local/cuda-/samples/
cuDNN	/usr/src/cudnn_samples_v7/
Multimedia API	/usr/src/tegra_multimedia_api/
VisionWorks	/usr/share/visionworks/sources/samples/ /usr/share/visionworks-tracking/sources/samples/ /usr/share/visionworks-sfm/sources/samples/
OpenCV	/usr/share/OpenCV/samples/

### (1) 检查CUDA
Jetson-nano中已经安装了CUDA10.0版本，但是此时你如果运行 nvcc -V是不会成功的，需要你把CUDA的路径写入环境变量中。OS中自带Vim工具 ，所以运行下面的命令编辑环境变量
```
sudo vim  ~/.bashrc
```
在最后添加
```
export CUBA_HOME=/usr/local/cuda-10.0
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda-10.0/bin:$PATH
```
然后保存退出

对了最后别忘了source一下这个文件。
```
source ~/.bashrc
```
source后，此时再执行nvcc -V,输出CUDA版本信息等
### (2) 检查OpenCV

Jetson-nano中已经安装了OpenCV3.3版本，可以使用命令检查OpenCV是否安装就绪
```
pkg-config opencv --modversion
```
如果OpenCv安装就绪，会显示版本号3.3.1

### （3）检查cuDNN

Jetson-nano中已经安装好了cuDNN，并有例子可供运行，我们运行一下例子，也正好验证上面的CUDA
```Bash
cd /usr/src/cudnn_samples_v7/mnistCUDNN   #进入例子目录
sudo make     #编译一下例子
sudo chmod a+x mnistCUDNN # 为可执行文件添加执行权限
./mnistCUDNN # 执行
```
## 安装Tensorflow GPU版

### 使用pip进行安装
因为Jetson Nano中已经安装了Python3.6版本，所以安装pip还是比较简单的
```
sudo apt-get install python3-pip python3-dev
```
安装后pip是9.01版本，需要把它升级到最新版，升级后pip版本为19.0.3。这里面升级后会有一个小Bug，需要手动改一下
```
python3 -m pip install --upgrade pip  #升级pip
sudo vim /usr/bin/pip3   #打开pip3文件
```
将原来的
```
from pip import main
if __name__ == '__main__':
    sys.exit(main())
```
改成
```
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
```
修改结束后保存。运行pip3 -V成功后显示
```
~$ pip3 -V
pip 19.0.3 from /home/droneyee/.local/lib/python3.6/site-packages/pip (python 3.6)
```
### 安装相关包
```
sudo apt-get install python3-numpy   
sudo apt-get install python3-scipy
sudo apt-get install python3-pandas
sudo apt-get install python3-matplotlib
sudo apt-get install python3-sklearn
```

### 安装TensorFlow GPU版
（1）确认CUDA已经被正常安装
```
nvcc -V
```
如果能看到CUDA版本号，即为正确安装
（2）安装所需要的包
```
sudo apt-get install python3-pip libhdf5-serial-dev hdf5-tools
```
（3）安装TensorFlow GPU版本 
```
pip3 install --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v42 tensorflow-gpu==1.13.1+nv1
```

（4）安装Keras
```
sudo pip3 install keras
```
安装完成后，进入python3，检查一下安装成果，import keras时，下方提示using TensorFlow backend,就证明Keras安装成功并使用TensorFlow作为backend

测试TensorFlow
``` python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt
 
x_data = np.linspace(-0.5, 0.5, 200)[:, np.newaxis]
noise = np.random.normal(0, 0.02, x_data.shape)
y_data = np.square(x_data) + noise
 
x = tf.placeholder(tf.float32, [None, 1])
y = tf.placeholder(tf.float32, [None, 1])
 
# 输入层一个神经元，输出层一个神经元，中间10个
# 第一层
Weights_L1 = tf.Variable(tf.random.normal([1, 10]))
Biases_L1 = tf.Variable(tf.zeros([1, 10]))
Wx_plus_b_L1 = tf.matmul(x, Weights_L1) + Biases_L1
L1 = tf.nn.tanh(Wx_plus_b_L1)
 
# 第二层
Weights_L2 = tf.Variable(tf.random.normal([10, 1]))
Biases_L2 = tf.Variable(tf.zeros([1, 1]))
Wx_plus_b_L2 = tf.matmul(L1, Weights_L2) + Biases_L2
pred = tf.nn.tanh(Wx_plus_b_L2)
 
# 损失函数
loss = tf.reduce_mean(tf.square(y - pred))
 
# 训练
train = tf.train.GradientDescentOptimizer(0.1).minimize(loss)
 
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    for i in range(2000):
        sess.run(train, feed_dict={x: x_data, y: y_data})
        print("第{0}次，loss = {1}".format(i, sess.run(loss,feed_dict={x: x_data, y: y_data})))
    pred_vaule = sess.run(pred, feed_dict={x: x_data})
    plt.figure()
    plt.scatter(x_data, y_data)
    plt.plot(x_data, pred_vaule, 'r-', lw=5)
    plt.show()
```

## 测试YoloV3

yoloV3也是一个物品检测的小程序，而且搭建起来比较简单。这里要申明，本文用的是yoloV3的tiny版，正式版和tiny版安装的方法都是一样的，只是运行时的配置文件和权重文件不一样。我曾经试图跑正式版，但是跑不起来，基本上到第二次卷积就挂掉了，毕竟nano只有4G内存。

参见官方网站 https://pjreddie.com/darknet/yolo/

1.安装CUDA，OpenCV，cuDNN （已配置好）

2.下载
```
git clone https://github.com/pjreddie/darknet.git
```
3.配置
```
cd darknet
sudo vim Makefile   #修改Makefile
```
4.将Makefile的前三行修改一下
```
GPU=1
CUDNN=1
OPENCV=1
```
5.编译
```
make -j4
```
6.下载权重文件，这里直接下载tiny版的权重文件
```
wget https://pjreddie.com/media/files/yolov3-tiny.weights
```
7.测试
```Bash
./darknet detect cfg/yolov3-tiny.cfg yolov3-tiny.weights data/dog.jpg
layer     filters    size              input                output
    0 conv     16  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  16  0.150 BFLOPs
    1 max          2 x 2 / 2   416 x 416 x  16   ->   208 x 208 x  16
    2 conv     32  3 x 3 / 1   208 x 208 x  16   ->   208 x 208 x  32  0.399 BFLOPs
    3 max          2 x 2 / 2   208 x 208 x  32   ->   104 x 104 x  32
    4 conv     64  3 x 3 / 1   104 x 104 x  32   ->   104 x 104 x  64  0.399 BFLOPs
    5 max          2 x 2 / 2   104 x 104 x  64   ->    52 x  52 x  64
    6 conv    128  3 x 3 / 1    52 x  52 x  64   ->    52 x  52 x 128  0.399 BFLOPs
    7 max          2 x 2 / 2    52 x  52 x 128   ->    26 x  26 x 128
    8 conv    256  3 x 3 / 1    26 x  26 x 128   ->    26 x  26 x 256  0.399 BFLOPs
    9 max          2 x 2 / 2    26 x  26 x 256   ->    13 x  13 x 256
   10 conv    512  3 x 3 / 1    13 x  13 x 256   ->    13 x  13 x 512  0.399 BFLOPs
   11 max          2 x 2 / 1    13 x  13 x 512   ->    13 x  13 x 512
   12 conv   1024  3 x 3 / 1    13 x  13 x 512   ->    13 x  13 x1024  1.595 BFLOPs
   13 conv    256  1 x 1 / 1    13 x  13 x1024   ->    13 x  13 x 256  0.089 BFLOPs
   14 conv    512  3 x 3 / 1    13 x  13 x 256   ->    13 x  13 x 512  0.399 BFLOPs
   15 conv    255  1 x 1 / 1    13 x  13 x 512   ->    13 x  13 x 255  0.044 BFLOPs
   16 yolo
   17 route  13
   18 conv    128  1 x 1 / 1    13 x  13 x 256   ->    13 x  13 x 128  0.011 BFLOPs
   19 upsample            2x    13 x  13 x 128   ->    26 x  26 x 128
   20 route  19 8
   21 conv    256  3 x 3 / 1    26 x  26 x 384   ->    26 x  26 x 256  1.196 BFLOPs
   22 conv    255  1 x 1 / 1    26 x  26 x 256   ->    26 x  26 x 255  0.088 BFLOPs
   23 yolo
Loading weights from yolov3-tiny.weights...Done!
data/dog.jpg: Predicted in 0.239507 seconds.
dog: 56%
car: 52%
truck: 56%
car: 62%
bicycle: 58%
```

![](https://img-blog.csdnimg.cn/20190418111323859.png)

## 调试OpenCV调用摄像头


新建CVtest.py 文件，测试代码
```python
# coding=utf-8
import cv2 as cv
def video_demo():
# 自动配置第一个USB摄像头
    capture=cv.VideoCapture(1)
    while(True):
        ref,frame=capture.read()

        cv.imshow("0",frame)
# 按Esc退出
        c= cv.waitKey(30) & 0xff
        if c==27:
            capture.release()
            break

video_demo()
cv.waitKey()
cv.destroyAllWindows()
```

## 
