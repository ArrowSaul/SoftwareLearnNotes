# openGauss

- openGauss安装

  - 软件包下载

    - 官网
      - [软件包 | openGauss](https://opengauss.org/zh/download/)


    - 下载
      - 选择X86架构，openGauss20.03LTS版本轻量版下载![img](https://api2.mubu.com/v3/document_image/a0caa82e-3429-4c7d-90b5-ccd5bb196b8d-21810177.jpg)


  - 环境配置

    - 官网
      - [Getting Started (osinfra.cn)](https://docs-opengauss.osinfra.cn/zh/docs/latest-lite/docs/GettingStarted/GettingStarted.html)


    - 官方安装教程
      - 版本选择latest,轻量版![img](https://api2.mubu.com/v3/document_image/479f89a8-0ed3-4283-9d1d-0f7a4cb8b339-21810177.jpg)


    - 准备硬件安装环境![img](https://api2.mubu.com/v3/document_image/6f462a4b-cad8-40a5-8d09-e6836a4f2516-21810177.jpg)


    - 准备软件安装环境![img](https://api2.mubu.com/v3/document_image/885ae5d7-9bbd-484e-aa38-1f23fd97dc85-21810177.jpg)


    - 软件依赖要求

      - yum源配置

        - [华为欧拉Euler aarch64，Error: There are no enabled repositories in “/etc/yum.repos.d“, “/etc/yum/repos.d“-CSDN博客](https://blog.csdn.net/qq_32033383/article/details/138670262?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7ECtr-1-138670262-blog-124185212.235%5Ev43%5Epc_blog_bottom_relevance_base2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7ECtr-1-138670262-blog-124185212.235%5Ev43%5Epc_blog_bottom_relevance_base2&utm_relevant_index=2)


        - vim /etc/yum.repos.d/openEuler_x86_64.repo


        - 复制文档中的内容粘贴至配置文件


        - :wq!


      - 安装两个必要依赖

        - yum install libaio-devel


        - yum install readline-devel


  - 前提条件

    - 用户/用户组配置，用户权限添加![img](https://api2.mubu.com/v3/document_image/49b77b0a-be4b-4e73-877d-a921877b4746-21810177.jpg)


    - 创建用户，用户组

      - 创建用户组opengauss
        - groupadd opengauss


      - 创建用户opengauss并添加进opengauss用户组
        - useradd -g opengauss -m opengauss


      - 给opengauss设置密码
        - passwd opengauss


      - 输入密码
        - huawei@123


      - 再次输入密码
        - huawei@123


    - 用户添加root权限

      - 进入用户配置文件
        - visudo


      - 在root下添加一条配置信息
        - opengauss   ALL=(ALL)   ALL


      - 强制保存并退出
        - :wq!


      - 刷新配置文件
        - source /etc/sudoers


    - 切换用户到openguass用户
      - su - opengauss


    - 创建openGauss文件夹
      - mkdir openGauss


  - 安装

    - 上传文件到/home/opengauss

      - 在opengauss目录下点击上传按钮![img](https://api2.mubu.com/v3/document_image/08486352-e95b-4770-8704-b2eac9c51b44-21810177.jpg)


      - 上传openGauss软件包文件![img](https://api2.mubu.com/v3/document_image/85e08b43-0632-4aa1-8bb6-11b6ce409598-21810177.jpg)


    - 解压文件到openGauss目录
      - sudo tar -zxvf openGauss-Lite-6.0.0-RC1-openEuler-x86_64.tar.gz -C ./openGauss


    - 给openGauss文件夹内添加opengauss所有操作权限
      - sudo chmod -R 777 openGauss


    - ll查看权限，如用户，用户组权限全部完成更改，即为成功![img](https://api2.mubu.com/v3/document_image/29f1f8ef-4c9a-4748-9893-c7fd3b18b639-21810177.jpg)


    - 执行安装

      - 进入openGauss目录
        - cd openGauss


      - 安装
        - sh ./install.sh --mode single -D ~/openGauss/data -R ~/openGauss/install  --start


      - 输入密码
        - huawei@123


      - 再次输入密码
        - huawei@123


      - 刷新文件
        - source /home/opengauss/.bashrc