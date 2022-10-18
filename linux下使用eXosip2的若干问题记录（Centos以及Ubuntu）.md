# linux下使用eXosip2的若干问题记录（Centos以及Ubuntu） #

**问题1：**

./a.out: error while loading shared libraries: libeXosip2.so.12: cannot open shared object file: No such file or directory

解决办法：

sudo vim /etc/ld.so.conf 末尾添加 /usr/local/lib

保存退出，执行sudo ldconfig

再次执行，成功

**问题2：**

```
（Ubuntu）g++ *.cpp -losip2 -leXosip2
/usr/bin/ld: /tmp/cc27Gleh.o: undefined reference to symbol 'osip_www_authenticate_init'
//usr/local/lib/libosipparser2.so.12: 无法添加符号: DSO missing from command line
collect2: error: ld returned 1 exit status
```

解决办法:

注意//usr/local/lib/libosipparser2.so.12这句话，把这里提到的动态库链接上
-losipparser2

————————————————

版权声明：本文为CSDN博主「这个名字不知道有没有人用啊」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/weixin_43272766/article/details/97575783