# Orange Pi Kunpeng Pro

## 介绍

Orange Pi Kunpeng Pro 开发板是香橙派联合华为精心打造的高性能开发板，其搭载了鲲鹏处理器，可提供 8TOPS INT8 计算能力，提供了 8GB 和 16GB 两种内存版本。Kunpeng Pro 开发板结合了鲲鹏全栈根技术，全面使能高校计算机系统教学和原生开发。同时支持 FPGA+ARM，从体系结构、数字逻辑设计、操作系统和编译，再到嵌入式开发，可以基于同一套体系结构和一套开发板实现贯穿打通。

## 注意事项

- 开发板硬件规格![img](https://api2.mubu.com/v3/document_image/bd7bd404-99f2-4597-bbb6-2fe4f631c342-21810177.jpg)
- 开发板接口![img](https://api2.mubu.com/v3/document_image/3131cb05-c7f0-409e-8ff2-5f08dc3377cb-21810177.jpg)
- 拨码开关使用![img](https://api2.mubu.com/v3/document_image/463a7a1c-7de2-444d-a9a5-079020ed995e-21810177.jpg)

## 制卡

下载官方文件（主要下载制卡工具和用户手册）

[Orange Pi - Orangepi](http://www.orangepi.cn/html/hardWare/computerAndMicrocontrollers/service-and-support/Orange-Pi-kunpeng.html)

烧录镜像（时间比较长）

SD卡插入开发板

## 开发板登录

### 在线登录

连接鼠标，键盘，显示器，最后接上电源![img](https://api2.mubu.com/v3/document_image/3e135200-7373-4076-9c02-25f1d55f2098-21810177.jpg)

登陆后显示屏出现 即为成功![img](https://api2.mubu.com/v3/document_image/c371f11a-0d64-45ec-8cab-8fb71759ec04-21810177.jpg)

默认登录密码是：openEuler，登录成功后显示![img](https://api2.mubu.com/v3/document_image/27ebedbb-372e-4078-b54f-f4c013b89dce-21810177.jpg)

### 串口登录

MobaXterm下载

MobaXterm free Xserver and tabbed SSH client for Windows (mobatek.net)](https://mobaxterm.mobatek.net/)

使用安卓线连接开发板![img](https://api2.mubu.com/v3/document_image/76e3c13c-4777-44f1-8161-4c5785a01c97-21810177.jpg)

利用MobaXterm串口连接开发板![img](https://api2.mubu.com/v3/document_image/aa35aac2-e4dc-45b6-85d0-c3245303d0b5-21810177.jpg)![img](https://api2.mubu.com/v3/document_image/4a187168-82a1-4972-ac06-19e6e41dae2b-21810177.jpg)![img](https://api2.mubu.com/v3/document_image/4d45b4f3-2f73-4c1d-838b-c9b51370e2e6-21810177.jpg)

连接电源后使用账号密码登录（root：openEuler/openEuler：openEuler）![img](https://api2.mubu.com/v3/document_image/5805ad45-dd00-4944-a47b-0b47c88ed0b8-21810177.jpg)![img](https://api2.mubu.com/v3/document_image/a930c07f-f361-4911-a7f2-8445c074ec30-21810177.jpg)

### 远程登录

以上任意一种方式连接开发板后连接热点

查看网络ip（此时ip为本机ip）

```
ip addr 
```

![img](https://api2.mubu.com/v3/document_image/0cf87217-fef5-4469-8a57-579c2760d526-21810177.jpg)

查看附近网络

```
nmcli dev wifi
```

连接网络

```
sudo nmcli dev wifi connect wifi_name password wifi_passwd
```

![img](https://api2.mubu.com/v3/document_image/0540de10-de15-4ee9-ae07-c13534939f2f-21810177.jpg)

测试网络连通性

```
ping baidu.com
```

![img](https://api2.mubu.com/v3/document_image/2769556d-11fe-4486-8f55-9bb4808afac7-21810177.jpg)

查看当前网络ip地址（此时连接网络后分配到了网络的ip）

```
ip addr show wlan0
```

![img](https://api2.mubu.com/v3/document_image/4140688c-8294-4a07-ba05-212696856137-21810177.jpg)

MobaXterm连接Pro

通过SSH连接同一网段ip地址下的开发板![img](https://api2.mubu.com/v3/document_image/65cb2b13-b436-4f1e-8948-052e5d173897-21810177.jpg)

关机

```
sudo poweroff
```

![img](https://api2.mubu.com/v3/document_image/f320c609-2069-4763-85c3-a3cece5a7487-21810177.jpg)