---
post_title: CentOS 7 + Nginx 配置支持IPV6
post_name: centos7-nginx-ipv6
author: 唐小波
layout: post
link: https://devops.vpclub.cn/centos7-nginx-ipv6/
published: true
tags:
  - ipv6
  - nginx
categories:
  - 运维
---

# CentOS 7 + Nginx配置支持IPV6

**作者：** 唐小波

此文档针对局域网网络环境IPV6的支持测试，根据公司业务IPv6改造需求进行渠道运营部IPv6改造计划，作出相对应的内部面向客户互联网系统IPv6改造。

## CentOS 7 主机系统配置

默认CentOS7安装将选择最小安装，完后是没有任何配置的，需要手动配置需要的各种工具，同时dhcp需要手动修改为静态IP地址，添加路由及网关、DNS等。

输入命令 /etc/sysconfig/network-scripts/ifcfg-ens160进行查看：

```bash
TYPE=Ethernet
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens160
UUID=4fcebce4-376c-4dd9-8ec3-0a0f0449728d
DEVICE=ens160
IPADDR=172.16.14.9
PREFIX=16
GATEWAY=172.16.0.1
DNS1=172.16.20.254
ONBOOT=yes
PEERDNS=yes
PEERROUTES=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
```

接下来进行文本配置

1.编辑 /etc/modprobe.d/disable_ipv6.conf文件(需要root权限)

```bash
mkdir -p /etc/modprobe.d/disable_ipv6.conf
```

添加以下配置

```bash
alias net-pf-10 off
options ipv6 disable=0
```

结果输出为

```bash
cat /etc/modprobe.d/disable_ipv6.conf
alias net-pf-10 off
options ipv6 disable=0
```

2.编辑 /etc/sysconfig/network文件(需要root权限)

添加以下配置

```bash
NETWORKING_IPV6=yes
GATEWAY=172.16.14.9
```

结果输出为

```bash
cat /etc/sysconfig/network
# Created by anaconda
PEERNTP=no
NETWORKING_IPV6=yes
GATEWAY=172.16.14.9
```

然后执行service network restart重启

3.编辑 /etc/sysctl.conf 文件(需要root权限)

添加以下配置

```bash
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

结果输出为

```bash
cat /etc/sysctl.conf
# sysctl settings are defined through files in
# /usr/lib/sysctl.d/, /run/sysctl.d/, and /etc/sysctl.d/.
#
# Vendors settings live in /usr/lib/sysctl.d/.
# To override a whole file, create a new file with the same in
# /etc/sysctl.d/ and put new settings there. To override
# only specific settings, add a file with a lexically later
# name in /etc/sysctl.d/ and put new settings there.
#
# For more information, see sysctl.conf(5) and sysctl.d(5).
fs.file-max=502400
net.core.somaxconn=20480
net.ipv4.tcp_max_syn_backlog=20480
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

然后运行sysctl -p 的命令，启用IPv6 (需要root权限)

4.验证是否开启IPV6

执行命令ip addr查看是否已经有 inet6 的地址，如果有说明已经开启ipv6

结果输出为

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
    link/ether 00:50:56:93:50:08 brd ff:ff:ff:ff:ff:ff
    inet 172.16.14.9/16 brd 172.16.255.255 scope global ens160
       valid_lft forever preferred_lft forever
    inet6 fe80::9cf8:8a38:b858:6a9a/64 scope link tentative dadfailed
       valid_lft forever preferred_lft forever
    inet6 fe80::8d22:f74:515a:e59e/64 scope link tentative dadfailed
       valid_lft forever preferred_lft forever
    inet6 fe80::2e94:c0d7:86d5:418c/64 scope link tentative dadfailed
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN
    link/ether 02:42:fc:78:03:c8 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
```

## IPV6隧道配置

1.https://tunnelbroker.net注册并登陆；

2.左侧点击Create Regular Tunnel创建一个隧道规则===>IPv4 Endpoint 需要些服务器的外网IP地址===>选择一个隧道服务器===>这里选择了“亚洲日本”

IPv4 Endpoint填写外网IP地址：

![logo](/images/operation-centos7-nginx-ipv6/md-icon.png)

选择一个隧道服务器：

![logo](/images/operation-centos7-nginx-ipv6/md-icon-gray.png)

查看Tunnel信息

![logo](/images/operation-centos7-nginx-ipv6/md-header.png)

Client IPv6 Address 就是你的IPv6 地址，之后解析域名(AAAA解析)的时候用的就是它(域名里不用写“/64”)

配置信息

![logo](/images/operation-centos7-nginx-ipv6/md-header-2.png)

重点：选择IP的方式设置（这里选择的是Linux-route2），然后将文本框中local IP需改成内网IP（这两个IP在阿里云后台都能看到），复制文本框中的内容到CentOS环境下执行。

```bash
[root@template ~]# modprobe ipv6
ip[root@template ~]# ip tunnel add he-ipv6 mode sit remote 74.82.46.6 local 172.16.14.9 ttl 255
ip link set he-ipv6 up
ip addr add 2001:470:23:60f::2/64 dev he-ipv6
ip[root@template ~]# ip link set he-ipv6 up
[root@template ~]# ip addr add 2001:470:23:60f::2/64 dev he-ipv6
[root@template ~]# ip route add ::/0 dev he-ipv6
ip -[root@template ~]# ip -f inet6 addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 state UNKNOWN qlen 1
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 state UP qlen 1000
    inet6 fe80::9cf8:8a38:b858:6a9a/64 scope link tentative dadfailed
       valid_lft forever preferred_lft forever
    inet6 fe80::8d22:f74:515a:e59e/64 scope link tentative dadfailed
       valid_lft forever preferred_lft forever
    inet6 fe80::2e94:c0d7:86d5:418c/64 scope link tentative dadfailed
       valid_lft forever preferred_lft forever
5: he-ipv6@NONE: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1480 state UNKNOWN qlen 1
    inet6 2001:470:23:60f::2/64 scope global
       valid_lft forever preferred_lft forever
    inet6 fe80::ac10:e09/128 scope link
       valid_lft forever preferred_lft forever
```

3.执行命令ping6 2001:470:23:60f::2

```bash
PING 2001:470:23:60f::2(2001:470:23:60f::2) 56 data bytes
64 bytes from 2001:470:23:60f::2: icmp_seq=1 ttl=64 time=0.027 ms
64 bytes from 2001:470:23:60f::2: icmp_seq=2 ttl=64 time=0.043 ms
64 bytes from 2001:470:23:60f::2: icmp_seq=3 ttl=64 time=0.040 ms
--- 2001:470:23:60f::2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.027/0.036/0.043/0.009 ms
```

能ping通说明已经配置成功

## Nginx配置

Nginx是一款轻量级的网页服务器、反向代理服务器。相较于Apache、lighttpd具有占有内存少，稳定性高等优势。它最常的用途是提供反向代理服务。

- 安装

在Centos下，yum源不提供nginx的安装，可以通过切换yum源的方法获取安装。也可以通过直接下载安装包的方法，**以下命令均需root权限执行**：
首先安装必要的库（nginx 中gzip模块需要 zlib 库，rewrite模块需要 pcre 库，ssl 功能需要openssl库）。选定**/usr/local**为安装目录，以下具体版本号根据实际改变。

- 安装前准备：

1.添加防火墙规则:

```bash
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
```

结果输出得到：

```bash
cat /etc/sysconfig/iptables
# sample configuration for iptables service
# you can edit this manually or use system-config-firewall
# please do not ask us to add additional ports/services to this default configuration
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```

重启下防火墙 

```bash
service iptables restart
```

2.安装gcc gcc-c++(如新环境,未安装请先安装)

```bash
yum install -y gcc gcc-c++
```

3.安装PCRE库

```bash
cd /usr/local/
wget http://jaist.dl.sourceforge.net/project/pcre/pcre/8.33/pcre-8.33.tar.gz
tar -zxvf pcre-8.33.tar.gz
cd pcre-8.33
./configure
make && make install

如报错:configure: error: You need a C++ compiler for C++ support
解决:yum install -y gcc gcc-c++
```

4.安装SSL库

```bash
cd /usr/local/
wget http://www.openssl.org/source/openssl-1.0.1j.tar.gz
tar -zxvf openssl-1.0.1j.tar.gz
cd openssl-1.0.1j
./config
make && make install
```

5.安装zlib库存

```bash
cd /usr/local/
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11/
./configure
make && make install
```

6.安装nginx

```bash
cd /usr/local/
wget http://nginx.org/download/nginx-1.8.0.tar.gz
tar -zxvf nginx-1.8.0.tar.gz
cd nginx-1.8.0/
mkdir -p /var/temp
mkdir -p /var/temp/nginx
./configure \--prefix=/usr/local/nginx \--pid-path=/var/run/nginx/nginx.pid  \--lock-path=/var/lock/nginx.lock \--error-log-path=/var/log/nginx/error.log \--http-log-path=/v ar/log/nginx/access.log \--with-http_gzip_static_module \--http-client-body-temp-path=/var/temp/n ginx/client \--http-proxy-temp-path=/var/temp/nginx/proxy \--http-fastcgi-temp-path=/var/temp/ngi nx/fastcgi \--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \--http-scgi-temp-path=/var/temp/nginx/s cgi --with-ipv6 --with-openssl=/var/temp/nginx/openssl
make && make install

```

编译完毕后，会在当前目录下创建objs目录，新的nginx执行文件将生成在该目录中，替换nginx执行文件，执行以下命令：

./nginx -s stop#关闭Nginx

cp objs/nginx /usr/local/nginx/sbin  #覆盖原有Nginx执行文件

./nginx  #启动Nginx

./nginx -V  #检查nginx是否已经支持ipv6

7.nginx配置ipv6监听，在此/usr/local/nginx/conf/nginx.conf 目录路径下配置，结果显示如下：

```bash
 server {
     listen       80;
     listen [::]80 ipv6only=on;
     server_name  2001:470:23:60f::2;

     location / {
         proxy_pass http://172.16.14.8:8000;  
     }
```

重新加载Nginx配置文件：
./nginx -s reload

## 测试IPV6

在另一台局域网服务器上部署简易HTTP文件服务器。切换当前目录，这里以临时目录为例：

```bash
touch /tmp/ipv6_test.txt
cd /tmp/
ls
ipv6_test.txt
python -m SimpleHTTPServer 8000
Serving HTTP on 0.0.0.0 port 8000 ...
```

在本地进行IPV6测试：

- 通过IPV4网络环境进行访问，测试结果如下：

```bash
curl http://172.16.14.8:8000
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN"><html>
<title>Directory listing for /</title>
<body>
<h2>Directory listing for /</h2>
<hr>
<ul>
<li><a href="ipv6_test.txt">ipv6_test.txt</a>
</ul>
<hr>
</body>
</html>
```

- 通过Nginx反向代理实现IPV6网络环境的转发访问，测试结果如下：

```bash
curl -g -6 "http://[2001:470:23:60f::2]:80/"
```

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN"><html>
<title>Directory listing for /</title>
<body>
<h2>Directory listing for /</h2>
<hr>
<ul>
<li><a href="ipv6_test.txt">ipv6_test.txt</a>
</ul>
<hr>
</body>
</html>
```

网络环境IPV4和网络环境IPV6 Nginx反向代理二者对比下，相同的当前目录文件内容说明应用服务器已具备支持IPV6的网络环境。








