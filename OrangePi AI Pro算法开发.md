# OrangePi Ai Pro

## 1.电脑推视频流

###  安装ffpmeg



###  终端输入命令

```
ffmpeg -f dshow -i video="USB2.0 HD UVC WebCam" -vcodec libx264 -pix_fmt yuv420p -s 400x200 -framerate 15 -r 25 -preset:v ultrafast -tune zerolatency -f flv rtmp://119.3.211.142:9909/live/101
```

# 视频推拉流

## 1，安装环境依赖

```
sudo apt-get install gcc g++ 
sudo apt-get install libpcre3 libpcre3-dev 
apt-get install zlib1g zlib1g-dev 
安装openssl不能用这个 apt-get 指令，会直接安装成高版本,也好像是系统就自带3.0版本
apt-get install yasm
```

## 2，安装nginx和nginx-http-flv-module

### 安装

```
 cd /usr/local/
 wget http://nginx.org/download/nginx-1.19.5.tar.gz                        //下载nginx
 tar -zxvf nginx-1.19.5.tar.gz                                             //解压ng
 cd nginx-1.19.5
 wget https://github.com/winshining/nginx-http-flv-module/archive/master.zip           
 unzip nginx-http-flv-module-master.zip
 ./configure --prefix=/usr/local/nginx --add-module=./nginx-http-flv-module-master --with-http_ssl_module    //编译安装nginx，并指定上面下载的模块路径
 make && make install
```

Make make install 出现下面错误的时候 表示openssl版本过高需要下载低版本的

![img](https://bqxc5tc302q.feishu.cn/space/api/box/stream/download/asynccode/?code=YTRiNGRmYTk5MWI1ZmMyNTU5ZDI1YzIwZDIzMmEwNDdfbmR6dTJCWUVPc3RKelJNOXNmb1V1MHQ2SDc3a3N2eXBfVG9rZW46TTFCdWJFNVlGb1dLSnN4UzNGdmNJbnpXbkhmXzE3MjY5Mjg2NDI6MTcyNjkzMjI0Ml9WNA)

把原来的openssl3.0版本卸载了，然后吧1.1.0的安装到一个目录下，编译的时候指定openssl的解压目录（源文件目录）

![img](https://bqxc5tc302q.feishu.cn/space/api/box/stream/download/asynccode/?code=NjViOWJjM2NlM2VhMmMzZjUyOTJjMGYwYTdhYmMyYThfc0NXU1hvcGg0TDc3ZDVPM0hyV0VxemQ0MzIycW9RM1hfVG9rZW46UWFCZWJRUWpQbzNKVmd4SjlIMmMxMUhRbkZiXzE3MjY5Mjg2NDI6MTcyNjkzMjI0Ml9WNA)

### 修改nginx配置

```
 cd /usr/local/nginx/conf
 vim nginx.conf
```

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
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
  include /usr/local/nginx/conf/vhost/*.conf;
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

## 3，安装ffmpeg

先安装nasm

```
cd /usr/local
wget https://www.nasm.us/pub/nasm/releasebuilds/2.14/nasm-2.14.tar.gz
tar -zxvf nasm-2.14.tar.gz
cd nasm-2.14
./configure
make && make install
```

在安装×264

```
cd /usr/local
wget https://code.videolan.org/videolan/x264/-/archive/master/x264-master.zip
unzip x264-master.zip
cd x264-master
./configure --enable-static --enable-shared
make && make install
```

安装

```
cd /usr/local
wget http://www.ffmpeg.org/releases/ffmpeg-4.3.tar.gz
tar ffmpeg-4.3.tar.gz
tar -zxvf nasm-2.14.tar.gz
cd ffmpeg-4.3
./configure --prefix=/usr/local/ffmpeg  --enable-gpl --enable-libx264
make && make install
cp /usr/local/ffmpeg/bin/* /usr/bin/
ffmpeg -version
```

推流过程可能会遇到ffmpeg: error while loading shared libraries: libx264.so.157: cannot open shared object file: No such file or directory 错误

解决办法：

```
vim /etc/ld.so.conf
```

添加：

```
include /usr/local/lib/
/usr/local/lib/
```

![img](https://bqxc5tc302q.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTNlMzJhMDBmNjVkNWZhN2U4ZTQxM2I5NTdjYjk0YzVfYURIRWlpZHlCbk9wVUlwa0VxcXJIaHFFcEdBQnA3c1NfVG9rZW46SnQwb2JFRmpwbzFMZUx4VnBnZ2NiMGJUbmxnXzE3MjY5Mjg2NDI6MTcyNjkzMjI0Ml9WNA)

保存后进行重载ldconfig

## 4，推流

```
ffmpeg -re -rtsp_transport tcp -i "rtsp://admin:123456@192.168.1.114:554/Streaming/Channels/101?transportmode=unicast" -f flv -vcodec libx264 -vprofile baseline -acodec aac -strict experimental -ar 44100 -ac 2 -b:a 96k -r 25 -b:v 500k -s 640*480  -f flv  -q 10  rtmp://192.168.1.187:9909/live/101
 
服务器推本地视频流命令：
ffmpeg -re -i /usr/local/mp4/1.mp4 -f flv -vcodec libx264 -vprofile baseline -acodec aac -strict experimental -ar 44100 -ac 2 -b:a 96k -r 25 -b:v 500k -s 640*480  -f flv  -q 10  rtmp://192.168.1.187:9909/live/101
 
OBS工具推流：
推流服务器：rtmp://192.168.1.187:9909/live/
串流秘钥：101
 
对应的拉流地址：
http方式：
http://192.168.1.187:9938/live?port=9909&app=live&stream=101
rtmp方式(VLC工具可以播放视频和音频，PotPlayer工具只能播放音频)：
rtmp://192.168.1.187:9909/live/101
```