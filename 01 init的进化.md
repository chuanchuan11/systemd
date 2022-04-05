> # init的进化，全功能的Systemd

  内核启动的第一个用户空间进程是由init开始的，init主要有如下3个版本:

# 1) System V init, (Sys V 传统的顺序启动init, 已经过时)包含目录：/etc/inittab
  
```
     a) sysvinit 的重要优点是确定的执行顺序：脚本严格按照启动数字的大小顺序执行，一个执行完毕再执行下一个，这非常有益于错误排查。
     b) UpStart 和 systemd 支持并发启动，导致没有人可以确定地了解具体的启动顺序，排错不易
     c) 但是串行地执行脚本导致 sysvinit 运行效率较慢，在新的 IT 环境下，启动快慢成为一个重要问题。此外动态设备加载等 Linux 新特性也暴露出 sysvinit 设计的一些问题。
        针对这些问题，人们开始想办法改进 sysvinit，以便加快启动时间，并解决 sysvinit 自身的设计问题
```

# 2) Upstart, (Ubuntu使用过) 包含目录：/etc/init，包含.conf文件
  
     UpStart 解决了之前提到的 sysvinit 的缺点。采用事件驱动模型，UpStart 可以：

```
     a) 更快地启动系统
     b) 当新硬件被发现时动态启动服务
     c) 硬件被拔除时动态停止服务
     d) 这些特点使得 UpStart 可以很好地应用在桌面或者便携式系统中，处理这些系统中的动态硬件插拔特性
```

# 3) systemd, (并行按需启动，主流)包含目录：/usr/lib/systemd/; etc/systemd/
  
     相对于传统的init程序，systemd的特点有：
  
```
a) 更快的启动速度：采用了 socket / D-Bus activation 等技术启动服务
b) 能够按需启动守护进程
c) 采用 Linux 的 Cgroup 特性跟踪和管理进程的生命周期。准确干净的关闭服务, 回收资源。
d) 启动挂载点和自动挂载的管理:Systemd 内建了自动挂载服务实现 autofs 的功能,无需另外安装 autofs 服务
e) 实现事务性依赖关系管理
f) 日志服务:systemd 自带日志服务 journald, 克服现有的 syslog 服务的缺点
e) Systemd Journal 用二进制格式保存所有日志信息(内核日志和应用日志),用户只用journalctl一个命令来查看所有日志信息。日志的配置文件是/etc/systemd/journald.conf
```

参考：
Linux 系统的总管 Systemd
https://blog.csdn.net/weixin_30894389/article/details/95682927

浅析Linux初始化init系统;第3部分[Systemd]刘明(2014-7-16)
https://blog.csdn.net/younger_china/article/details/52539657

Systemd 入门教程：命令篇  阮一峰(2016-3-7)
https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

Systemd 入门教程：实战篇 阮一峰(2016-3-8)
https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html
