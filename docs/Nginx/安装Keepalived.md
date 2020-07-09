### [安装Keepalived](https://www.cnblogs.com/kingsonfu/p/11392470.html)

安装包：https://www.keepalived.org/software/keepalived-2.1.3.tar.gz

在生成makefile文件这一步骤时发生了错误提示：

![image-20200709132909018](D:\myproject\myblogg\MyBlogg\picture\image-20200709132909018.png)

[解决方案](http://www.mamicode.com/info-detail-3055006.html)

[后来又看这个博客](https://www.cnblogs.com/panwenbin-logs/p/11692761.html)

## 1.5 启动keepalived

启用keepalived服务，启动keepalived服务：

systemctl enable keepalived

systemctl start keepalived