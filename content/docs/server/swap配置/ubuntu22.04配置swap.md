---
date: 2023-10-28
title: swap配置
linkTitle: ubuntu22.04配置swap 交换区
featured: true

series:
  - 服务器
categories:
  - 服务器
tags:
  - Linux
  - swap
  - server
---

* 检查当前的交换空间状态：运行以下命令查看当前交换空间的情况：

  `sudo swapon --show`

## 方式一

- 创建交换文件（如果没有交换空间）：运行以下命令以创建一个交换文件（通常以 swapfile 命名）：

  `sudo fallocate -l <size> /swapfile`

    - 其中 <size> 是您想要分配给交换空间的大小，例如 1G 表示 1 GB 的交换空间。


- 设置交换文件权限：运行以下命令以设置交换文件的权限：

  `sudo chmod 600 /swapfile`


- 格式化交换文件：运行以下命令以格式化交换文件：

  `sudo mkswap /swapfile`

- 启用交换空间：运行以下命令以启用交换空间：

  `sudo swapon /swapfile`

- 更新 /etc/fstab 文件：打开 /etc/fstab 文件并添加以下行以在启动时自动挂载交换空间：

  `/swapfile swap swap defaults  0 0`

## 方式二

#### 使用dd创建swap文件/data/swapfile，大小为1G

`dd if=/dev/zero of=/data/swapfile bs=1M count=1024`

- 交换文件格式化为swap分区

  `mkswap /data/swapfile`

- 设置权限

  `chmod 600 /data/swapfile`

- 启用swap分区

  `swapon /data/swapfile`

- 设置开机自动启用swap分区

  `vi /etc/fstab`

- 添加一行

  `/data/swapfile swap swap defaults 0 0`

---

- 验证交换空间：再次运行 `sudo swapon --show` 命令，确认交换空间已成功启用。

    - 请注意，使用交换空间可能会对系统性能产生一定的影响，因为交换空间的读写速度远低于物理内存。因此，建议在具有足够物理内存的情况下，根据您的实际需求和系统负载来决定是否启用交换空间。

---

### 查看是否需设置 swap使用优先级

```
#查看优先级设置，0不使用swap分区，100尽可能使用swap分区，根据需求设置一个中间值即可
cat /proc/sys/vm/swappiness
#临时设置优先级
sysctl vm.swappiness=50
#设置开机自动生效
echo "vm.swappiness = 50"  >>  /etc/sysctl.conf
```
