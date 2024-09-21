# OrangePi Ai Pro

## 人流量监测算法开发

## 服务器拉流

### 1 安装环境依赖

```
sudo apt-get install gcc g++ 
sudo apt-get install libpcre3 libpcre3-dev 
sudo apt-get install zlib1g zlib1g-dev 
sudo apt-get install yasm
```

### 2 安装nginx和nginx-http-flv-module

```
cd /usr/local/
sudo wget http://nginx.org/download/nginx-1.19.5.tar.gz                        
sudo tar -zxvf nginx-1.19.5.tar.gz
sudo mv nginx-1.19.5 nginx
cd nginx
sudo wget https://github.com/winshining/nginx-http-flv-module/archive/master.zip           
sudo unzip nginx-http-flv-module-master.zip
sudo ./configure --prefix=/usr/local/nginx --add-module=./nginx-http-flv-module-master --with-http_ssl_module
sudo make && sudo make install
```

#### 2.1 利用Vim编辑nginx配置

```
cd /usr/local/nginx/conf
sudo vim nginx.conf
```

#### 2.2 配置内容（根据自己文件路径进行修改，开放服务器9909，9938，80，443端口，注意：Https需要服务器备案域名，配置ssl证书）

```
#user  nobody;
worker_processes  1;
 
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
 
#pid        logs/nginx.pid;
 
events {
    worker_connections  1024;
}

http {
    error_log /usr/local/nginx/logs/error.log;
    access_log /usr/local/nginx/logs/access.log;
    include       mime.types;
    default_type  application/octet-stream;
    sendfile            on;
    keepalive_timeout  65;
    server {
        listen       9938;
        server_name  localhost;
        location /live {
            flv_live on; 
            chunked_transfer_encoding  on;
            add_header 'Access-Control-Allow-Origin' * always; 
            add_header 'Access-Control-Allow-Credentials' 'true'; 
        }
    }
    server {
        listen 443 ssl;
        server_name abrillantlee.top;
        ssl_certificate /etc/letsencrypt/live/abrillantlee.top/fullchain.pem; 
        ssl_certificate_key /etc/letsencrypt/live/abrillantlee.top/privkey.pem; 
        location / {
        proxy_pass  http://119.3.211.142:9938;
        }
        location /live {   
            flv_live on;     #open flv live streaming (subscribe)
            chunked_transfer_encoding  on; #open 'Transfer-Encoding: chunked' response
            add_header 'Access-Control-Allow-Origin' * always; #add additional HTTP header
            add_header 'Access-Control-Allow-Credentials' 'true'; #add additional HTTP header
            add_header 'Access-Control-Allow-Headers' X-Requested-With;
        }
           }
      server {
      listen 80;
      server_name abrillantlee.top;
      rewrite ^(.*)$ https://$host$1 permanent;
  }
}

rtmp {
    out_queue               4096;
    out_cork                 8;
    max_streams             128;
    timeout                 15s;
    drop_idle_publisher     15s;
    log_interval 5s;
    log_size     1m;
    server {
        listen 9909;      #监听的端口号
        #server_name 127.0.0.1;
        application live {     #自定义的名字
            live on;
       }
        application hls {
            live on;
            hls on;
            hls_path /tmp/hls;
            hls_fragment 1s;
            hls_playlist_length 3s;
       }
    }
}
```

### 3 服务器安装ffmpeg

#### 3.1 安装nasm

```
cd /usr/local
sudo wget https://www.nasm.us/pub/nasm/releasebuilds/2.14/nasm-2.14.tar.gz
sudo tar -zxvf nasm-2.14.tar.gz
cd nasm-2.14
sudo ./configure
sudo make && sudo make install
```

#### 3.2 安装×264

```
cd /usr/local
sudo wget https://code.videolan.org/videolan/x264/-/archive/master/x264-master.zip
sudo unzip x264-master.zip
cd x264-master
sudo ./configure --enable-static --enable-shared
sudo make && sudo make install
```

#### 3.3 安装ffmpeg

```
cd /usr/local
sudo wget http://www.ffmpeg.org/releases/ffmpeg-4.3.tar.gz
sudo tar -zxvf ffmpeg-4.3.tar.gz
cd ffmpeg-4.3
sudo ./configure --prefix=/usr/local/ffmpeg  --enable-gpl --enable-libx264
sudo make && sudo make install
sudo cp /usr/local/ffmpeg/bin/* /usr/bin/
sudo ffmpeg -version
```

推流过程可能会遇到ffmpeg: error while loading shared libraries: libx264.so.157: cannot open shared object file: No such file or directory 错误

解决办法：

```
sudo vim /etc/ld.so.conf
```

添加：

```
include /usr/local/lib/
/usr/local/lib/
```

保存后进行重载

```
sudo ldconfig
```

## 客户端推流

### Windows

#### 1.安装ffmpeg并配置环境变量

官网：[FFmpeg](https://ffmpeg.org/)

下载后解压并添加系统环境变量

如： D:\develop\ffmpeg-master-latest-win64-gpl\bin

#### 2.终端输入命令推流到指定rtmp服务器

```
ffmpeg -f dshow -i video="USB2.0 HD UVC WebCam" -vcodec libx264 -pix_fmt yuv420p -s 400x200 -framerate 15 -r 25 -preset:v ultrafast -tune zerolatency -f flv rtmp://119.3.211.142:9909/live/101
```

### Orange Pi Ai Pro

通过python代码推流到指定rtmp服务器（安装ffmpeg并配置环境变量）

```python
if __name__ == '__main__':
    context = init_acl(DEVICE_ID)

    # 初始化模型
    det_model = YoloV5(model_path=trained_model_path)

    # 打开摄像头
    cap = cv2.VideoCapture(0)

    # 使用 FFmpeg 创建 RTSP 流
    command = f'ffmpeg -re -i pipe:0 -f flv rtmp://119.3.211.142:9909/live/101'
    process = subprocess.Popen(command.split(), stdin=subprocess.PIPE)

    while cap.isOpened():  # 在摄像头打开的情况下循环执行
        ret, frame = cap.read()  # 读取一帧图像

        if not ret:
            break

        # 前处理、推理、后处理，得到最终推理图片
        img_res, det_result_str = det_model.infer(frame)

        # 将处理后的帧转换为字节流
        _, buffer = cv2.imencode('.jpg', img_res)
        frame_bytes = buffer.tobytes()

        # 写入帧到 FFmpeg 进程
        process.stdin.write(frame_bytes)

    # 释放资源
    cap.release()
    cv2.destroyAllWindows()
    det_model.release()  # 释放模型相关资源
    deinit_acl(context, 0)  # 去初始化 ACL

    # 关闭 FFmpeg 进程
    process.stdin.close()
    process.terminate()
    process.wait()
```