---
title: "文件系统演变: 从单机到分布式Draft"
date: 2025-07-12T09:00:00+08:00
tags: [FS]
featured_image: ""
draft: true
description: "介绍文件系统的一些关键历史、演进过程、前沿技术和未来发展趋势。希望能够帮助大家更好的入门和理解储存系统，面向当下和未来的存储技术，希望帮大家对如何维护、迭代、探索创新方面有所启发。"
---

---
其他信息
---

统一操作接口：通过 struct file_operations 定义标准文件操作（如 read、write），由具体文件系统实现。
核心概念:
- [system_file_type](https://github.com/torvalds/linux/blob/v6.15/include/linux/fs.h#L2607-L2637)
```c++
struct file_system_type {
	const char *name; // 文件系统的名字
	struct dentry *(*mount) (struct file_system_type *, int,
		       const char *, void *); // mount操作回调函数
	struct file_system_type * next; // 多个文件系统按照链表的形式进行存储 
```
- super_block:
- file & file_operations:
- inode & inode_operations:
- dentry:





open命令调用链路：

[SYSCALL_DEFINE3](https://github.com/torvalds/linux/blob/v6.15/fs/open.c#L1448-L1553)(系统调用入口)

[do_sys_open](https://github.com/torvalds/linux/blob/v6.15/include/linux/fs.h#L2798-L2799)


文件系统分类:
- 对磁盘数据做索引: fat32,nfs,ext3,ext4,...
- 分布式文件系统: TFS,GFS,FastDFS
- Fuse: 内核的一部分，读写实际文件之前做一些定制化。比如我们写日志，时间、traceID怎么加进去？一个公共层。

VFS 是什么？ Virtual File System。
Linux: 一切皆文件。
在内核里面是多个文件系统同时工作的。

操作系统的分层访问:
应用
内核 open
VFS
ext3/ext4, fuse, proc (无存储文件系统 )

/proc: linux 无存储文件系统，可以cat,ls等，但是机器重启之后又是新的了。

```shell
➜  ~ df -H
Filesystem        Size    Used   Avail Capacity iused ifree %iused  Mounted on
/dev/disk3s1s1    494G     16G    128G    11%    412k  1.3G    0%   /
devfs             204k    204k      0B   100%     690     0  100%   /dev
/dev/disk3s6      494G     20k    128G     1%       0  1.3G    0%   /System/Volumes/VM
/dev/disk3s2      494G     14G    128G    10%    1.8k  1.3G    0%   /System/Volumes/Preboot
/dev/disk3s4      494G    732M    128G     1%     346  1.3G    0%   /System/Volumes/Update
/dev/disk2s2      524M   6312k    504M     2%       1  4.9M    0%   /System/Volumes/xarts
/dev/disk2s1      524M   6070k    504M     2%      37  4.9M    0%   /System/Volumes/iSCPreboot
/dev/disk2s3      524M   2597k    504M     1%      95  4.9M    0%   /System/Volumes/Hardware
/dev/disk3s5      494G    333G    128G    73%    3.4M  1.3G    0%   /System/Volumes/Data
map auto_home       0B      0B      0B   100%       0     0     -   /System/Volumes/Data/home
/dev/disk3s3      494G   2155M    128G     2%     110  1.3G    0%   /Volumes/Recovery
/dev/disk1s1     5369M   2038M   3309M    39%      66   32M    0%   /System/Volumes/Update/SFR/mnt1
/dev/disk3s1      494G     16G    128G    11%    426k  1.3G    0%   /System/Volumes/Update/mnt1
➜  ~
```


挂载示例:
mount -t proc mountSourceDir mountDestDir

mount 是一个用户空间的命令。

mount 如何走到系统调用的？
system_mount --> file_system_type.mount


struct file_system_type {