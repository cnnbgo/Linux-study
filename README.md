# 在Docker 中运行 OpenWrt 旁路网关

参考链接：
https://hub.docker.com/r/sulinggg/openwrt
https://mlapp.cn/376.html

1.宿主机配置\
打开网卡混杂模式\
sudo ip link set eth0 promisc on

创建网络\
docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 macnet

拉取镜像，请拉取阿里云仓库中的镜像：\
docker pull registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4\
标签rpi4为arm64架构的op，armv8以上cpu全部适用

创建并启动容器openwrt\
docker run --restart always --name openwrt -d --network macnet --privileged registry.cn-shanghai.aliyuncs.com/suling/openwrt:rpi4 /sbin/init

2.进入容器并修改相关参数\
docker exec -it openwrt bash

修改网络配置\
vim /etc/config/network

重启网络\
/etc/init.d/network restart

修改root密码\
passwd



# wto
这里编辑的是什么
