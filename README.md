# 在Docker 中运行 OpenWrt 旁路网关

参考链接：
https://hub.docker.com/r/sulinggg/openwrt
https://mlapp.cn/376.html

1. 宿主机配置\
打开网卡混杂模式\
sudo ip link set eth0 promisc on

创建网络\
docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 macnet

拉取镜像，请拉取阿里云仓库中的镜像：\
docker pull registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4\
标签rpi4为arm64架构的op，armv8以上cpu全部适用

创建并启动容器openwrt\
docker run --restart always --name openwrt -d --network macnet --privileged registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4 /sbin/init

2. 进入容器并修改相关参数\
docker exec -it openwrt bash

修改网络配置\
vim /etc/config/network

重启网络\
/etc/init.d/network restart

修改root密码\
passwd



# 配置webdav非docker方式
参考链接：
https://zhuanlan.zhihu.com/p/382569744
https://github.com/hacdias/webdav/

1. 下载webdav程序，arm64版本
```
wget https://github.com/hacdias/webdav/releases/download/v4.1.1/linux-arm64-webdav.tar.gz
```

2. 解压、复制到/usr/bin目录下
```
tar -zxvf linux-arm64-webdav.tar.gz
sudo cp webdav /usr/bin/
```

3. 注册service
```
cd /etc/systemd/system
sudo vim webdav.service
```
写入如下内容
```
[Unit]
Description=WebDAV server
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/bin/webdav --config /etc/webdav/config.yaml
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

4. 建立上面文件对应的配置文件\
下面配置在81端口开启了两个共享
```
cd /etc/webdav/
sudo vim config.yaml
```
写入
```
# Server related settings
address: 0.0.0.0 
port: 81
auth: true 
tls: false 
cert: cert.pem 
key: key.pem

# Default user settings (will be merged)
scope: /mnt/share 
modify: true 
rules: []
 
users:
-   username: root
    password: root

-   username: admin
    password: admin
    scope: /www/wwwroot
```

5 开机并启动服务
```
systemctl enable webdav
systemctl start webdav
```
6 查看运行状态
```
systemctl status webdav
```

7. 不报错的话直接输入
```
http://你的ip地址:81
```

# miui11海外版play商店网络DNS问题\
自定义防火墙规则添加如下，原理很简单，阻止全部dns转发
```
iptables -I FORWARD 1 -p udp -j REJECT
```


# wto
这里编辑的是什么


# wto
这里编辑的是什么

# wto
这里编辑的是什么

