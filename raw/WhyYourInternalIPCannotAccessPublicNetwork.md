## 为什么内网IP不能访问公网?
一般来说，服务器的内网IP可以通过网关设备访问互联网。

### 内网IP访问公网示例
以以下机器为例:
```shell
~$ ip r
default via 10.34.35.254 dev bond0
10.34.35.0/24 dev bond0  proto kernel  scope link  src 10.34.35.42
~$
```
指定出口为该内网IP,请求DNS服务器, 响应OK:
```shell
~$ dig @8.8.8.8 www.google.com -b 10.34.35.42

; <<>> DiG 9.9.5-3ubuntu0.10-Ubuntu <<>> @8.8.8.8 www.google.com -b 10.34.35.42
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34590
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.google.com.			IN	A

;; ANSWER SECTION:
www.google.com.		109	IN	A	31.13.94.36

;; Query time: 8 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Fri Nov 15 17:09:06 CST 2024
;; MSG SIZE  rcvd: 48

~$
```

### 内网IP不通公网示例
一般来说内网IP可以访问公网，但不尽然。比如如下机器:
```shell
~$ ip r
default via 112.13.166.1 dev bond1
10.0.0.0/8 via 10.34.91.254 dev bond0
10.34.91.0/24 dev bond0 proto kernel scope link src 10.34.91.32
100.100.0.0/16 via 10.34.91.254 dev bond0
112.13.166.0/26 dev bond1 proto kernel scope link src 112.13.166.21
115.231.29.0/26 dev bond1 proto kernel scope link src 115.231.29.12
117.148.177.128/26 dev bond1 proto kernel scope link src 117.148.177.163
124.160.124.0/27 dev bond1 proto kernel scope link src 124.160.124.26
172.16.0.0/12 via 10.34.91.254 dev bond0
192.168.0.0/16 via 10.34.91.254 dev bond0
~$
~$ ip -4 addr show | grep inet
    inet 127.0.0.1/8 scope host lo
    inet 10.34.91.32/24 brd 10.34.91.255 scope global bond0
    inet 183.131.7.5/26 brd 183.131.7.63 scope global bond1:1
    inet 124.160.124.26/27 brd 124.160.124.31 scope global bond1:2
    inet 112.13.166.21/26 brd 112.13.166.63 scope global bond1:3
    inet 115.231.29.12/26 brd 115.231.29.63 scope global bond1:4
    inet 117.148.177.163/26 brd 117.148.177.191 scope global bond1:6
~$
```
指定出口为该内网IP,请求DNS服务器, 可见访问不通，连接超时了。
```shell
~$ dig @8.8.8.8 www.google.com -b 10.34.91.32

; <<>> DiG 9.16.1-Ubuntu <<>> @8.8.8.8 www.google.com -b 10.34.91.32
; (1 server found)
;; global options: +cmd
;; connection timed out; no servers could be reached

~$
```
### 原因分析
网络请求遵循从哪里来，就从哪返回的原则。 对于入口请求，上述2台机器都是没有问题的。  
上述2台机器的不同表现，本质上是由网络路由决定的。

第一台机器，默认路由为`10.34.35.254`, 这是一个内网IP, 一般为NAT网关地址。
机器上的内网IP能够访问公网，就是通过这个NAT设备转发了出口请求。
```shell
~$ ip r
default via 10.34.35.254 dev bond0
10.34.35.0/24 dev bond0  proto kernel  scope link  src 10.34.35.42
~$
```
第二台机器，默认路由为`183.131.7.1`, 这是一个外网IP, 这种情况该IP即为本机的一个公网出口IP，
该IP绑定在了`bond1`上面，内网IP绑定在`bond0`上。不同的网卡不互通，内网IP自然就不能访问公网了。
```shell
~$ ip r
default via 183.131.7.1 dev bond1
10.0.0.0/8 via 10.34.91.254 dev bond0
10.34.91.0/24 dev bond0 proto kernel scope link src 10.34.91.32
100.100.0.0/16 via 10.34.91.254 dev bond0
112.13.166.0/26 dev bond1 proto kernel scope link src 112.13.166.21
115.231.29.0/26 dev bond1 proto kernel scope link src 115.231.29.12
117.148.177.128/26 dev bond1 proto kernel scope link src 117.148.177.163
124.160.124.0/27 dev bond1 proto kernel scope link src 124.160.124.26
172.16.0.0/12 via 10.34.91.254 dev bond0
183.131.7.0/26 dev bond1 proto kernel scope link src 183.131.7.5
192.168.0.0/16 via 10.34.91.254 dev bond0
~$
```
注意: 这里其他公网IP, 都是绑定在了 `bond1` 上，这也正是默认路由的网络接口。
### 场景延伸
> 前端即默认路由到公网IP而非NAT的好处:
> - 作为机器各个IP路由的兜底路由, 防止路由漏网线路IP通过NAT流出, 减少NAT压力。

> 网卡bond模式:
> - bond0: 负载模式, 可以保证bond虚拟网卡和被bond的几张物理网卡拥有相同的MAC地址。
> - bond1: 主备模式, 即网卡之间能快速切换，主网卡出现故障时能快速切换到备网卡上，切换过程中上层应用几乎不受影响。
  因为bond驱动会临时接管上层应用的数据包，存放到数据缓冲区内，等备网卡启动之后再传过去。但如果切换时间过长，
  则会引起缓冲区的溢出，就会丢包。
> - bond2: 选择网卡的序号=（源MAC地址XOR目标MAC地址）%slave的数量。
> - bond3: 广播策略，数据包会被广播到所有slave卡上。
> - bond4使用动态链接聚合策略。启动时会创建一个聚合组，所有slave网卡共享同样的速率和双工设定，
  需要交换机支持IEEE 802.3ad动态链路聚合模式。支持使用ethtool工具获取每个slave网卡的速率和双工设定。
> - bond5: bond5基于每个slave网卡的速率选择传输网卡，支持使用ethtool工具获取每个slave的速率。
> - bond6: bond6包含了bond5模式，同时还支持IPV4流量接收时的负载均衡策略，而且不需要任何交换机的支持，
  支持只是用ethtool获取每个slave的速率，要求底层驱动支持设置某个网卡设备的硬件地址。
### Reference
https://developer.aliyun.com/article/1502125