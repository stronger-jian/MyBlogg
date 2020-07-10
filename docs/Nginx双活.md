[Nginx双活](https://www.cnblogs.com/jeancheng/p/13044424.html)

1. 先在阿里云服务器[安装Nginx](./docs/Nginx/安装Nginx.md),[安装Keepalived](/docs/Nginx/安装Keepalived.md).

2. 配置nginx反向代理

   upstream web {

   ​	server 服务器地址:端口;

   }

   location / {

   ​	proxy_pass http://web;

   }

   

   

   

   