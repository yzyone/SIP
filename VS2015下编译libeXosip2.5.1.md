# VS2015下编译libeXosip2.5.1 #

如果需要编译 libeXosip2.5.0 参考如下两个连接：

> libeXosip2.5.0
> 
> VS2013下编译osip和exosip的5.0版本静态库及搭建和简单例子的实现

## 一、下载： ##

> 链接：https://pan.baidu.com/s/1FNabcgatCvSKzX5fnU8EPw&shfl=sharepset
> 提取码：vpgh

其他下载方式：

```
libosip2-5.1.0.tar.gz
http://ftp.gnu.org/gnu/osip/
libexosip2-5.1.0.tar.gz
http://download.savannah.nongnu.org/releases/exosip/
c-ares-1.15.0.tar.gz
http://ftp.twaren.net/Unix/NonGNU//osip
```

## 二、解压源码 ##

解压各压缩包后，放于同一目录并改名去除版本号。

```
libexosip2-5.0.0 -> exosip
libosip2-5.0.0 -> osip
c-ares-1.13.0 -> c-ares
```

## 三、工程编译 ##

**3.1   编译错误处理**

3.1.1  #error: Macro definition of snprintf conflicts with Standard Library function declaration”：

出现 error “fatal error C1189: #error: Macro definition of snprintf conflicts with Standard Library function declaration”

**解决方案**

原因是，很多的库或者程序中将snprintf()函数定义为 _snprintf(),而在vs2015出现之前并不支持_snprintf()。

然而，vs2015定义了 snprintf()。在头文件 Windows Kits\10\Include\10.0.10240.0\ucrt\stdio.h(1927)中：

这显然就导致了snprintf（）的重定义。

解决的办法是，搜索工程中所有定义snpritf的文件中（ ，找到如下定义：osipparser2\internal.h ,\osip2\internal.h

	#define snprintf _snprintf

将其替换为

```
#if defined(_MSC_VER) && _MSC_VER<1700 
#  define snprintf _snprintf
#endif
```

3.1.2

打开exosip\platform\vsnet文件夹下的eXosip.sln,会提示升级，确定，加载

编译时会出现如下错误：

> error LNK2019:无法解析的外部符号_ares_getplatform，该符号在函数_get_DNS_Registry中被引用。 error LNK2019:无法解析的外部符号_ares_create_query,该符号在函数_ares_query中被引用。 错误 LNK2019 无法解析的外部符号 _ares_strsplit，该符号在函数 _set_search 中被引用 sipTest 错误 LNK2019 无法解析的外部符号 _ares_strsplit_free，该符号在函数 _ares_init_options 中被引用 sipTest

右键点击libcares工程  选择Add-->Exiting Item 添加以下五个文件：

```
ares_platform.h
ares_platform.c
ares_create_query.c
ares_strsplit.c
ares_strsplit.h
```

重新编译 ok.

**3.2 eXosip工程配置**

点击工程eXosip， 右键选择属性，配置属性->c/c+±>预处理器->预处理器定义，去掉 HAVE_OPENSSL_SSL_H：

去除TSCOPENSSL、TSCWINDOWS、TSCSUPPORT 、HAVEOPENSSLSSLH这些宏定义。

 

## 四、简单测试 ##

**4.1  新建测试工程**

工程名–>右击–>属性–>配置属性–>链接器 --> 输入 -->附加依赖项：增加静态库引用：

```
Dnsapi.lib;
Iphlpapi.lib;
ws2_32.lib;
eXosip.lib;
osip2.lib;
osipparser2.lib;
Qwave.lib;
libcares.lib;
delayimp.lib;
```

**4.2   配置头文件**

工程名–>右击–>属性–>配置属性–>C/C++ -->常规 -->附加包含目录: 将osip和eXosip的头文件include包含进来：

	eXosip2Lib\exosip\include eXosip2Lib\osip\include

**4.3  配置库**

工程名–>右击–>属性–>配置属性–>链接器 --> 常规 --> 加附库目录：将eXosip的库包含进来，

	exosip\platform\vsnet\v142\Win32\Debug

**4.3 测试代码**

```
#include <eXosip2/eXosip.h>
#include <winsock.h>
 
#pragma comment(lib, "Dnsapi.lib")
#pragma comment(lib, "Iphlpapi.lib")
#pragma comment(lib, "ws2_32.lib")
 
#pragma comment(lib, "eXosip.lib")
#pragma comment(lib, "libcares.lib") 
#pragma comment(lib, "osip2.lib")
#pragma comment(lib, "osipparser2.lib") 
#pragma comment(lib, "Qwave.lib")
#pragma comment(lib, "delayimp.lib")
 
int main()
{
    eXosip_t* sip = eXosip_malloc();
    if (eXosip_init(sip) == OSIP_SUCCESS)
    {
        printf( "exosip init success");
        if (eXosip_listen_addr(sip, IPPROTO_UDP, NULL, 0, AF_INET, 0) == OSIP_SUCCESS)
        {
             printf("exosip listen addr success");
        }
    }
    eXosip_quit(sip);
    system("pause");
    return 0;
} 
```

运行看到success 表示成功.

————————————————

版权声明：本文为CSDN博主「lcyw」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/machh/article/details/103159346