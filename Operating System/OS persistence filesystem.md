# What

Filesystem是一套管理分区数据组织，文件名，目录，权限，空闲空间，读写映射的规则+驱动代码。

Filesystem可是作为可加载模块（LKM），也可以编译进内核静态自带。Filesystem运行在内核态不用进行频繁的用户态和内核态切换，性能&安全管控更强。

## Filesystem

1. Linux交换分区的实现可以是一块没有格式化的硬盘分区（swap分区），DMA直接进行块IO读取，不用经过文件系统索引，inode，日志等开销；也可是文件系统内的一个大体积文件，格式化为交换格式，kernel把它当作虚拟内存交换空间是使用。
<ul>
<ol>
<li>mkswap /dev/sda2</li>
<li>swapon /dev/sda2 </li>
</ol>
Or: 
<ol>
<li>mkswap /swapfile</li>
<li>swapon /swapfile </li>
</ol>
</ul>

2. NAS（Network Attached Storage)就是把存储设备放在LAN/Wi-Fi中，云存储是连接服务商的机房集群，通过公网连接。
3. RAID：Redundant Array of Independent Disks独立磁盘冗余阵列，把多块物理硬盘/SSD组合成一个逻辑盘，通过条带化，冗余校验实现。Storage Array存储阵列是把多个物理介质聚合，统一管理对外提供存储服务的硬件/系统。Storage Array包含RAID（最经典），企业中高端集中式存储阵列，分布式存储阵列，云后端存储阵列。

