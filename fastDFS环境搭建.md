### fastfds安装步骤：

1、安装gcc环境

```
yum  install  make
yum  install   gcc 
yum  install  cmake
yum  install  gcc-c++
```

2、安装fastfds公有库

```
git clone https://github.com/happyfish100/libfastcommon.git
cd libfastcommon
./make.sh
./make.sh install
```
libfastcommon默认安装到了/usr/lib64/目录下，而Fastdfs程序使用,/usr/local/lib/,所以创建文件链接

3、创建链接

```
 mkdir /usr/local/lib/
 ln -s /usr/lib64/libfastcommon.so /usr/local/lib/libfastcommon.so
 ln -s /usr/lib64/libfastcommon.so /usr/lib/libfastcommon.so
 ```
 
 4、安装FastDFS
 
 ```
git clone https://github.com/happyfish100/fastdfs.git
cd fastdfs
./make.sh
./make.sh install
 ```

服务脚本：

```
/etc/init.d/fdfs_storaged
/etc/init.d/fdfs_trackerd
```

配置文件目录：/etc/fdfs/

5、配置tracker

```
cd /etc/fdfs/
cp tracker.conf.sample tracker.conf
// 修改tracker绑定IP
bind_addr=192.168.0.142
// 修改base_path，并确保base_path目录存在
base_path=/home/yanshujie/fastdfs/tracker
```

6、启动tracker

```
/etc/init.d/fdfs_trackerd start
```

7、修改Storage配置

```
cp storage.conf.sample storage.conf

# bind an address of this host
# empty for bind all addresses of this host
bind_addr=192.168.0.142

# the base path to store data and log files
base_path=/home/yanshujie/fastdfs/server

# store_path#, based 0, if store_path0 not exists, it's value is base_path
# the paths must be exist
store_path0=/home/yanshujie/fastdfs/server

# tracker_server can ocur more than once, and tracker_server format is
#  "host:port", host can be hostname or ip address
tracker_server=192.168.0.142:22122

# the port of the web server on this storage server
http.server_port=8888

```

8、启动storage服务

```
/etc/init.d/fdfs_storaged start
```
9、客户端配置

```
# the base path to store log files
base_path=/home/yanshujie/fastdfs/client

# tracker_server can ocur more than once, and tracker_server format is
#  "host:port", host can be hostname or ip address
tracker_server=192.168.0.142:22122

```

10、使用客户端测试服务

```
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf ./fastfds.md
// output:
// group1/M00/00/00/wKgAjlsLdxWAN9MPAAAIcGRl7rM9366.md
// 验证文件
ll /home/yanshujie/fastdfs/server/data/00/00/
```
