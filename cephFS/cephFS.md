# CephFS
1. ## 建立CephFS
1. ## 使用 kernel 驱动挂载 CephFS
1. ## 使用 fuse 挂载 CephFS
1. ## 使用 fstab 使 CephFS 挂载开机启动
1. ## CephFS 的 Cient 权限
1. ## CephFS 的资源配额
CephFS 允许给系统内的任意目录设置配额，这个配额可以限制目录树中这一点以下的字节数或者文件数。

局限性
 - 配额是合作性的、非对抗性的。 CephFS 的配额功能依赖于挂载它的客户端的合作，在达到上限时要停止写入；无法阻止篡改过的或者对抗性的客户端，它们可以想写多少就写多少。在客户端完全不可信时，用配额防止多占空间是靠不住的。
 - 配额是不准确的。 在达到配额限制一小段时间后，正在写入文件系统的进程才会被停止。很难避免它们超过配置的限额、多写入一些数据。会超过配额多大幅度主要取决于时间长短，而非数据量。一般来说，超出配置的限额之后 10 秒内，写入会被停掉。
 - 内核客户端的配额功能需要 操作系统内核版本在4.17以上。 用户空间客户端（ libcephfs 、 ceph-fuse ）已经支持配额了。
 - 基于路径限制挂载时必须谨慎地配置配额。 客户端必须能够访问配置了配额的那个目录的索引节点，这样才能执行配额管理。如果某一客户端被 MDS 能力限制成了只能访问一个特定路径（如 /home/user ），并且它们无权访问配置了配额的父目录（如 /home ），这个客户端就不会按配额执行。所以，基于路径做访问控制时，最好在限制了客户端的那个目录（如 /home/user ）、或者它下面的子目录上配置配额。

与通用文件系统quota对比
 - CephFS quota是针对目录的，可限制目录下存放的文件数量和容量
 - CephFS没有一个统一的UID/GID机制，传统的基于用户和组的配额管理机制很难使用
 - CephFS一般与应用配合使用，应用自己记录用户信息，将用户关联到对应的CephFS目录

> 配额实现思路
>  1. 
> 
# 参考
1. CephFS quota的支持 . https://blog.csdn.net/younger_china/article/details/78163279
2. Kubernetes添加带Quota限额的CephFS StorageClass . https://www.cnblogs.com/ltxdzh/p/9173706.html