# Etcd

  环境：centos7

### 安装
下载(地址：https://github.com/coreos/etcd/releases )并解压，拷贝etcd，etcdctl到/usr/local/bin下

### 创建配置文件（/etc/etcd/etcd.conf.yml）
配置文件模板：https://github.com/coreos/etcd/blob/master/etcd.conf.yml.sample

### 添加并启动etcd服务
- 创建/usr/lib/systemd/system/etcd.service文件

```
[Unit]
Description=Etcd Server
After=network.target
After=network-online.target
Wants=network-online.target
Documentation=https://github.com/coreos

[Service]
Type=notify
ExecStart=/bin/bash -c "GOMAXPROCS=$(nproc) /usr/local/bin/etcd --config-file=/etc/etcd/etcd.conf.yml"
Restart=on-failure
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

- 添加自启动：systemctl enable etcd.service
- 启动服务：systemctl start etcd

### 防火墙开启端口
```
firewall-cmd --zone=public --add-port=2379/tcp --permanent
firewall-cmd --zone=public --add-port=2380/tcp --permanent
firewall-cmd --reload
```
### 集群创建
- 文档：https://coreos.com/etcd/docs/latest/v2/clustering.html

### 命令
在安装配置flannel时，k8s集群会共享网络配置，所以网络基本配置保存在etcd集群中。
使用到的命令有：
```
etcdctl mkdir 创建目录
etcdctl mk 创建键值对
etcdctl cluster-health 检查集群是否正常
etcdctl ls 获取目录中的所有键
etcdctl get 通过键获取对应的值
```
