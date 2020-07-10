### [安装Keepalived](https://blog.csdn.net/qq_42825214/article/details/105314603)

安装包：https://www.keepalived.org/software/keepalived-2.1.3.tar.gz



[先看这个博客](https://www.cnblogs.com/kingsonfu/p/11392470.html)

[后来又看这个博客](https://www.cnblogs.com/panwenbin-logs/p/11692761.html)

[这个博客可以启动起来](https://blog.csdn.net/qq_42825214/article/details/105314603)

修改配置：/etc/keepalived/keepalived.conf

启动、关闭、重启、查看状态 

service keepalived start|stop|restart|status

你可以使用 ps -ef|grep keepalived 来看它是否真的已经启动了，用 service keepalived status可能看不出来

最后那篇博客救了我的命。