# 步骤1：Flash JetBot镜像到SD卡上

下载JetBot SD卡图像jetbot_image_v0p3p0.zip
将64GB + SD卡插入台式机
使用Etcher，选择jetbot_image_v0p3p0.zip镜像并将其烧录到SD卡上
从台式机中取出SD卡

# 步骤2：启动Jetson Nano

将SD卡插入Jetson Nano（micro SD卡插槽位于模块下方）
将显示器，键盘和鼠标连接到Nano
通过将micro USB充电器连接到micro USB端口，打开Jetson Nano的电源
在确认它已启动后，重新连接piOLED，仔细检查接线，然后重新启动。

# 步骤3：将JetBot连接到WiFi

使用用户jetbot和密码jetbot登录
使用Ubuntu桌面GUI连接到WiFi网络
您的Jetson Nano现在应该在启动时自动连接到WiFi并在piOLED显示器上显示它的IP地址。

# 步骤4：从Web浏览器连接到JetBot

将机器人连接到WiFi后，可以通过执行以下步骤从Web浏览器连接到机械手
使用Ubuntu GUI关闭JetBot
从Jetson Nano拔下HDMI显示器，USB键盘，鼠标和电源
通过插入micro-USB线缆从USB电池组为JetBot供电
等待JetBot启动
在piOLED显示屏上检查机器人的IP地址。
在下一个命令中输入this代替<jetbot_ip_address>
# 步骤5：- 安装最新软件（可选）

JetBot GitHub存储库可能包含比预先安装在SD卡映像上的软件更新的软件。
要安装最新软件：
如果您还没有，请转到http://<jetbot_ip_address>:8888连接到您的机器人
单击+图标以打开Jupyter Lab启动器
启动一个新终端
通过输入以下命令从GitHub获取并安装最新的JetBot存储库
