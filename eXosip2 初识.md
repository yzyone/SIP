# eXosip2 初识 #


## 基础库介绍 ##

- [eXosip2](http://savannah.nongnu.org/projects/exosip/)
- [osip2](http://www.gnu.org/software/osip/)

osip 是一个应用于 sip 开发的库，因为本身接口函数偏过于底层，eXosip 在其基础上进行了二次封装。两个库需要一起使用，缺一不可，版本随意。

linux 环境下安装步骤：

1. 下载 osip2 eXosip2 源码
1. 解压 osip2 源码包
1. 进入对应文件夹，依次执行命令 ./configure 、 make 、make install
1. 解压 eXosip2 源码包
1. 进入对应文件夹，依次执行命令 ./configure 、 make 、make install

## SIP 服务端介绍 ##

- [minisipserver](https://www.myvoipapp.com/)

(miniSIPServer Professional SIP(VoIP) PBX|server for Windows and Linux. (myvoipapp.com))

配置流程:

主要是参考网站指导 [配置说明](小型企业建立IP-PBX系统指南. (myvoipapp.com))， 补充步骤，用于局域网增加客户端账号并监控特定 ip 的 sip 报文。

打开 minisipserver 软件
分机 -> 增加 -> 分机（账号）-> 密码 -> 确定 sip 客户端可以开始进行注册，成功后分机里头像将会从灰色变成蓝色
维护 -> 跟踪ip地址 -> 输入客户端ip -> 确定 从窗口可以看到客户端发过来的 sip 报文

## Register验证流程 ##

1. 在 sip 服务端工具设置客户端账号test ，密码 abc
1. 服务端 IP 192.168.1.2；客户端 IP 192.168.1.1
1. 进入 eXosip2 源码包 tools 目录下
1. ./sip_reg -r sip:192.168.1.2:5060 -u sip:test@192.168.1.1 -d -U test -P abc
1. 没有特殊情况，将会成功登录

————————————————

版权声明：本文为CSDN博主「大华锦绣华城」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/shenjinpeng123456/article/details/120788035
