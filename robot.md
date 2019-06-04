# 基本设置

## 步骤1：烧录JetBot镜像到SD卡上

下载JetBot SD卡镜像 jetbot_image_v0p3p0.zip

！注意 镜像64gb左右

将128 SD卡插入台式机

使用Etcher，选择jetbot_image_v0p3p0.zip镜像并将其烧录到SD卡上

从台式机中取出SD卡

## 步骤2：启动Jetson Nano

将SD卡插入Jetson Nano（micro SD卡插槽位于模块下方）

将显示器，键盘和鼠标连接到Nano

通过将micro USB充电器连接到micro USB端口，打开Jetson Nano的电源

在确认它已启动后，重新连接piOLED，仔细检查接线，然后重新启动。

## 步骤3：将JetBot连接到WiFi

使用用户名jetbot和密码jetbot登录

使用Ubuntu桌面GUI连接到WiFi网络

您的Jetson Nano现在应该在启动时自动连接到WiFi并在piOLED显示器上显示它的IP地址。

## 步骤4：从Web浏览器连接到JetBot


将板卡连接到WiFi后，可以通过执行以下步骤从Web浏览器连接到板卡

使用Ubuntu GUI关闭JetBot

从Jetson Nano拔下HDMI显示器，USB键盘，鼠标和电源

通过插入micro-USB线缆从USB电池组为JetBot供电

在piOLED显示屏上检查机器人的IP地址。

在控制端电脑打开浏览器，如firefox

在地址栏转到http://<jetbot_ip_address>:8888连接到板卡

如输入http://192.168.1.22:8888

单击以打开Jupyter Lab启动器

![](http://images.ncnynl.com/ros/2019/JL01_Basic-Motion.png) 

## 步骤5 测试基本动作

打开~/Notebooks/basic_motion/

打开basic_motion.ipynb， 即python的在线编译环境

![](http://images.ncnynl.com/ros/2019/JL01_Basic-Motion.png) 

按照步骤运行，把鼠标定位到某一代码块，按ctrl+enter运行

# 


