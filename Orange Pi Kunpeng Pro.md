# Orange Pi Kunpeng Pro

## 介绍

Orange Pi Kunpeng Pro 开发板是香橙派联合华为精心打造的高性能开发板，其搭载了鲲鹏处理器，可提供 8TOPS INT8 计算能力，提供了 8GB 和 16GB 两种内存版本。Kunpeng Pro 开发板结合了鲲鹏全栈根技术，全面使能高校计算机系统教学和原生开发。同时支持 FPGA+ARM，从体系结构、数字逻辑设计、操作系统和编译，再到嵌入式开发，可以基于同一套体系结构和一套开发板实现贯穿打通。

## 注意事项

开发板硬件规格![img](C:\Users\21925\Desktop\2.jpg)

开发板接口![img](C:\Users\21925\Desktop\4.jpg)

拨码开关使用![img](C:\Users\21925\Desktop\5.jpg)

## 制卡

下载官方文件（主要下载制卡工具和用户手册）

[Orange Pi - Orangepi](http://www.orangepi.cn/html/hardWare/computerAndMicrocontrollers/service-and-support/Orange-Pi-kunpeng.html)

烧录镜像（时间比较长）

SD卡插入开发板

## 开发板登录

### 在线登录

连接鼠标，键盘，显示器，最后接上电源![img](C:\Users\21925\Desktop\6.jpg)

登陆后显示屏出现 即为成功![img](C:\Users\21925\Desktop\7.jpg)

默认登录密码是：openEuler，登录成功后显示![img](C:\Users\21925\Desktop\8.jpg)

### 串口登录

MobaXterm下载

MobaXterm free Xserver and tabbed SSH client for Windows (mobatek.net)](https://mobaxterm.mobatek.net/)

使用安卓线连接开发板![img](C:\Users\21925\Desktop\9.jpg)

利用MobaXterm串口连接开发板![img](C:\Users\21925\Desktop\aa35aac2-e4dc-45b6-85d0-c3245303d0b5-21810177.jpg)![img](C:\Users\21925\Desktop\10.jpg)![img](C:\Users\21925\Desktop\11.jpg)

连接电源后使用账号密码登录（root：openEuler/openEuler：openEuler）![img](C:\Users\21925\Desktop\12.jpg)![img](C:\Users\21925\Desktop\13.jpg)

### 远程登录

以上任意一种方式连接开发板后连接热点

查看网络ip（此时ip为本机ip）

```
ip addr 
```

![img](C:\Users\21925\Desktop\14.jpg)

查看附近网络

```
nmcli dev wifi
```

连接网络

```
sudo nmcli dev wifi connect wifi_name password wifi_passwd
```

![img](C:\Users\21925\Desktop\15.jpg)

测试网络连通性

```
ping baidu.com
```

![img](C:\Users\21925\Desktop\16.jpg)

查看当前网络ip地址（此时连接网络后分配到了网络的ip）

```
ip addr show wlan0
```

![img](C:\Users\21925\Desktop\17.jpg)

MobaXterm连接Pro

通过SSH连接同一网段ip地址下的开发板![img](C:\Users\21925\Desktop\18.jpg)

关机

```
sudo poweroff
```

![`img`](C:\Users\21925\Desktop\19.jpg)