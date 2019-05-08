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

因为Jetson Nano中已经安装了Python3.6版本，所以安装pip还是比较简单的
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
