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

### 方法2 使用vino图形界面登录

参考系统自带VINO说明

使用时启动/usr/lib/vino/vino-server即可

PC端安装VNCserver


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


