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

# 自主避障

## 步骤1 在JetBot上收集数据

使用具有广角附件的Raspberry Pi V2相机，在有限的数据集上训练该模型。

通过导航到http://<jetbot_ip_address>:8888连接到您的机器人

使用默认密码jetbot登录

通过选择Kernel -> Shutdown All Kernels...来关闭所有其他正在运行的笔记本...

导航到~/Notebooks/collision_avoidance/

打开并跟随data_collection.ipynb笔记操作

![](http://images.ncnynl.com/ros/2019/JL03a_Data-Collection.png)

在Web浏览器中导航到https://courses.nvidia.com/dli-event

输入事件代码DLI_Jet_Demo

如果您尚未登录，请登录您的NVIDIA开发者帐户

选择查看View Course -> Course -> Click here to begin -> Start

等待几分钟，以便云培训机器进行设置

选择Launch Task启动Jupyter Lab

在Jupyter Lab选项卡中，导航到~/collision_avoidance

打开并跟随train_model.ipynb笔记操作

## 步骤2 在云上训练神经网络

![](http://images.ncnynl.com/ros/2019/JL03b_Training.png)

在Web浏览器中导航到https://courses.nvidia.com/dli-event

输入事件代码DLI_Jet_Demo

如果您尚未登录，请登录您的NVIDIA开发者帐户

选择查看View Course -> Course -> Click here to begin -> Start

等待几分钟，以便云培训机器进行设置

选择Launch Task启动Jupyter Lab

在Jupyter Lab选项卡中，导航到~/collision_avoidance

打开并跟随train_model.ipynb笔记操作

## 步骤3 在JetBot上运行实时演示

![](http://images.ncnynl.com/ros/2019/JL03c_Live-demo.png)

通过导航到http://<jetbot_ip_address>:8888连接回机器人

使用默认密码jetbot登录

通过选择Kernel -> Shutdown All Kernels...来关闭所有其他正在运行的笔记本...

导航到~/Notebooks/collision_avoidance

打开并跟随live_demo.ipynb笔记操作

给JetBot足够的空间来移动。

## 可参考官方视频

http://www.youtube.com/watch?v=6cLk9TSgFSw

# 目标追踪

![](http://images.ncnynl.com/ros/2019/JL04_Object-Following.png)

通过导航到http://<jetbot_ip_address>:8888连接到您的机器人

通过选择Kernel -> Shutdown All Kernels...关闭所有其他正在运行的笔记本...

导航到~/Notebooks/object_following/

将预先训练好的ssd_mobilenet_v2_coco.engine模型上传到此文件夹

还要确保示例3中的防碰撞模型在~/Notebooks/collision_avoidance中

打开并跟随live_demo.ipynb笔记操作

收集更多的防撞数据

尝试不同的神经网络架构（torchvision包有很多！）

修改新任务的碰撞避免示例

参考视频：

http://www.youtube.com/watch?v=MBUEbU9Q6wg
