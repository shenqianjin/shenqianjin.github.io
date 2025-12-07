---
title: "FS Disk Testing"
date: 2025-12-07T13:00:00+08:00
draft: false
author: "shenqianjin"
authorLink: "https://shenqianjin.github.io/"
description: ""
license: ""
images: []
tags: [FS,Disk]
categories: [FS]
featuredImage: "cover.png"
featuredImagePreview: ""
---

# 磁盘的分类
有多种磁盘分类标准，按磁盘存储实现技术分类，可划分为：
- 机械硬盘：
  - 使用旋转的磁盘和移动的读写头来存储和检索数据； 
  - 其优点在于存储容量大、价格相对较低； 
  - 其劣势在于容易受到物理冲击和振动的影响，寿命有限；
- 固态硬盘： 
  - 使用闪存芯片来存储数据； 
  - 相对机械硬盘具有更快的读写速度，并且更可靠、更耐用； 
  - 价格相对机械硬盘更高；

# SATA和NVMe
SSD有两种常用的接口形式，分别是SATA （Serial Advanced Technology Attachment）和
NVMe（Non-Volatile Memory Express），两者主要区别包括： 
- 传输速度：SATA接口传输速度最大约600MB/s，NVMe可达数GB/s； 
- 读写延时：NVMe接口的读写延时远低于SATA接口；

习惯上，SSD一般指的是SATA SSD，一般称NVMe SSD为NVMe

# 磁盘的评价体系
核心评价指标： 
- 容量、尺寸、接口等静态指标； 
- IOPS-Input/Output Operations Per Second，每秒读写次数，是随机读写的评价指标； 
- BW - BandWidth，读写带宽，一般用来衡量磁盘顺序读写的性能； 
- Latency-读写延时，指发送 l/O 请求和接收响应之间的间隔时间；

# 磁盘性能测试工具_fio
fio安装:
```shell
# Ubuntu / Debian
sudo apt-get update
sudo apt-get install fio

# CentOS / RHEL / Rocky Linux
sudo yum install epel-release # 需要先启用EPEL仓库
sudo yum install fio
```
fio参数:
- --filename 指定测试对象，可以是设备（如/dev/sdb）或文件。无默认值，必须指定。
- --rw 指定读写模式。read (顺序读), write (顺序写), randread (随机读), randwrite (随机写), randrw (随机混合读写)。
- --ioengine	指定I/O引擎，即发起I/O的方式。	sync (默认)， libaio (Linux异步I/O，性能测试常用)。psync 伪同步。
- --bs	I/O的块大小，是影响性能测试的关键参数。	4k (测试IOPS常用), 128k (测试吞吐量常用)
- --size	每个线程读写的数据总量。	例如 1G, 10G。不指定则会写满整个文件或设备。
- --numjobs	并发线程或进程数，用于模拟多任务负载。	1
- --iodepth	I/O队列深度，即每个线程同时发起的I/O请求数量，对SSD等高性能设备影响显著。	1
- --runtime	测试运行时间（秒）。与 --time_based 联用时，即使未完成--size指定的数据量，也会在时间到后停止。	不设置则会运行到完成 --size 指定的数据量。
- --direct	是否使用直接I/O。设为1时绕过系统缓存，测试结果更反映磁盘真实性能。	0 (使用缓存)。性能测试时建议设为1。
- --group_reporting	当有多个任务(numjobs>1)时，汇总显示整个组的统计结果，而不是每个任务单独显示。	不启用。
- --name	为测试任务指定一个名称。	无默认值。
- --time_based	启用基于时间的运行模式，与--runtime配合使用。	不启用。
- --output	将测试结果输出到指定文件，而不是终端屏幕。	默认输出到屏幕。

# 常见性能测试场景与命令示例
#### 1. 测试时延 (Latency)
高时延意味着响应慢。测试时通常使用较小的队列深度(iodepth=1)和块大小(bs=4k)。
```shell
# 测试随机读时延
fio --filename=/dev/sdb --rw=randread --bs=4k --ioengine=libaio --iodepth=1 --direct=1 --size=1G --runtime=60 --name=latency_test --group_reporting
```
#### 2. 测试吞吐量 (Throughput/Bandwidth)
高吞吐量意味着大数据传输能力强。测试时通常使用较大的队列深度和块大小。
```shell
# 测试顺序写吞吐量
fio --filename=/dev/sdb --rw=write --bs=128k --ioengine=libaio --iodepth=32 --direct=1 --size=10G --runtime=60 --name=throughput_test --group_reporting
```
#### 3. 测试IOPS (每秒I/O操作数)
高IOPS意味着处理小文件或随机读写能力强。这是SSD的关键指标，测试时通常使用小块的随机读写。
```shell
# 测试随机写IOPS
fio --filename=/dev/sdb --rw=randwrite --bs=4k --ioengine=libaio --iodepth=32 --direct=1 --size=10G --runtime=60 --name=iops_test --group_reporting
```

# 注意事项
- 数据安全第一：切勿在系统盘(/dev/sda等)或存有重要业务数据的磁盘上进行测试。测试会破坏所有数据。建议使用全新的、未挂载的空白磁盘。
- 使用配置文件：当参数复杂或需要重复测试时，推荐将参数写入.fio配置文件，然后通过 fio config_file.fio 执行，便于管理和复用。
- 理解输出结果：重点关注结果中的 bw (带宽，KB/s, MB/s)、iops、lat (延迟，usec, msec) 以及 clat percentiles (延迟百分比分布，如99.99%的请求在多少延迟内完成) 等指标。

# fio配置文件
```text
[global]
direct=1
ioengine=psync

[write-2MB]
rw=write
bs=2m
size=1024m
filename=/xxx/fio_test_files/write-2MB
name=write-2MB
stonewall # 关键参数：确保一个任务完成后才启动下一个

[write-4MB]
rw=write
bs=4m
size=1024m
filename=/xxx/fio_test_files/write-4MB
name=write-4MB
stonewall
```

```shell
#!/bin/bash

rws=$1
block_sizes=$2
if [ -z "$rws" ]; then
  rws='read,write'
fi
if [ -z "$block_sizes" ]; then
  block_sizes='64k,2m'
fi
cur_time=$(date '+%Y%m%d%H%M%S')

echo "$rws" | tr ',' '\n' | while read rw; do
    echo "$block_sizes" | tr ',' '\n' | while read block_size; do
      printf "*** fio testing for: %-14s ***\n" "$rw-$block_size"
      fio --ioengine=psync --direct=1 --size=1024m --rw=$rw --bs=$block_size --name="$rw-$block_size" \
      --filename=/xxx/fio_test_files/"$rw-$block_size.$cur_time"
    done
done
```