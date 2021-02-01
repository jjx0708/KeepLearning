## 2. Nginx 的安装

### 0. Linux 系统软件安装的几种方式：

在安装 nginx 之前，我们先来复习一个小知识点，在 Linux 系统上安装软件几种常用方法：

1. 直接使用 rpm 安装
2. 使用 yum 安装
3. 直接解压安装
4. 编译后再安装

**下面我们拓展说一下 rpm 安装方式和 yum 安装方式： **

#### 1. 一般 Linux 软件名称格式：

```shell
[abrt-addon-ccpp]-[2.1.11-19].[el7].[x86_64].rpm    ##rpm结尾的适用与redhat操作系统
[软件名称]          [软件版本]    [软件适用系统] [系统版本64位] 
```

#### 2. rpm 常用指令具体参数详情

```shell
rpm     -ivh    name.rpm          ##安装 ,-v显示过程，-h指定加密方式为hash
        -e      name              ##卸载
        -ql     name              ##查询软件生成文件
        -qlp    name.rpm          ##查询软件安装后会生成什么文件
        
        rpm -qlp wps-office-10.1.0.5672-1.a21.x86_64.rpm 
        
        -qa                       ##查询系统中安装的所有软件名称
        -qa |grep name            ##查询软件是否安装
        -q      name              ##查看
        -qp     name.rpm          ##查询软件安装包安装后的名字
        -qf     filename          ##查看filename属于那个安装包
        -ivh    name.rpm --force    ##强制安装，但不能忽略依赖性
        -ivh    name.rpm --nodeps   ##忽略依赖性并且强制安装
        -qi     name                ##查看软件信息
        
        rpm -qi wps-office-10.1.0.5672-1.a21.x86_64.rpm 
        
        -Kv     name.rpm            ##检测软件包是否被篡改
        
        rpm -Kv wps-office-10.1.0.5672-1.a21.x86_64.rpm
        
        -qp     name.rpm --scripts  ##检测软件在安装或卸载过程中执行的动作
        
```

#### 3. yum 安装与 rpm 安装的区别

1.  yum 能自动解决软件依赖问题，而 rpm 并不能！

    rpm ：只能安装已经下载到本地机器上的rpm包，无法解决软件包的依赖关系。

    yum：在线下载并安装rpm包,能更新系统,能自动处理包与包之间的依赖问题。

2. RPM管理支持事务机制。增强了程序安装卸载的管理。



而 nginx 安装就是一个相对复杂的，需要先编译再进行安装；

### 1. 先准备好需要的软件包：

这里我附上几个官网，大家可以自行去官网下载：

[nginx 下载](http://nginx.org/download/nginx-1.19.6.tar.gz)

[pcre 下载](https://ftp.pcre.org/pub/pcre/pcre-8.44.tar.gz)

[openssl 下载](https://www.openssl.org/source/openssl-1.1.1i.tar.gz)

[zlib 下载](http://zlib.net/zlib-1.2.11.tar.gz)

![image-20210201213238240](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201213238240.png)

全部下载完成可以使用 ftp 工具上传到自己的服务器上，并解压文件，接下来开始逐步安装，**一定要注意安装顺序！**

### 2. 安装 PCRE

```shell
# 解压文件
tar zxvf pcre-8.37.tar.gz
 
 # 进入文件目录
 cd pcre-8.37
 
 # 执行安装配置
 ./configure
 
 # 编译 并 安装
 make && make install
```

### 3. 安装 OPENSSL

```shell
tar -zxvf openssl-1.1.1i.tar.gz

cd openssl-1.1.1i

./config

 # 编译 并 安装
 make && make install
```

### 4. 安装 ZLIB

```shell
tar -zxvf zlib-1.2.11.tar.gz

cd zlib-1.2.11

./configure

make && make install
```

### 5. 安装 nginx

```shell
tar -zxvf nginx-1.19.6.tar.gz

./configure

cd nginx-1.19.6

make && make install
```

### 6. 检查 nginx 是否安装成功，进入软件默认的安装位置

```shell
cd /usr/local/nginx
```

![image-20210201214337018](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201214337018.png)

发现 nginx 已经安装完毕，接下来就是如何启动 nginx ；

### 7. 启动 ngnix

```shell
cd sbin

./nginx
```

在这里因为我已经启动过了，所以我先将 nginx 的服务停止，然后再重新启动。

使用指令：` ./nginx -s stop  ` 可以停止服务，使用 ` ./nginx ` 可以直接启动 nginx 服务，使用 ` ps -ef | grep nginx ` 可以查看 nginx 的进程，发现 nginx 已经成功启动。具体操作如下：

![image-20210201214659911](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201214659911.png)

好家伙，既然已经安装好了 nginx ，那我们赶紧就来试一试，nginx 的默认端口就是 80 端口，所以我们直接通过在浏览器中输入 IP 地址就可以进行访问了。

![image-20210201215156589](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201215156589.png)

这是什么情况？

难道是我们配置出问题了吗？

稍等，这波问题出在了 linux 的防火墙端口没有打开，因为使用的本地的虚拟机，而不是云服务器，所以默认的很多端口都是关闭的状态；这里我们只需要关闭防火墙就好了。

```shell
systemctl status firewalld
```

我这里目前已经关闭了，所以能看到 状态是 dead 的，如下图：

![image-20210201215628962](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201215628962.png)

如果是开启状态的话，这里先使用 ` systemctl start firewalld ` 开启防火墙。如下图：

![image-20210201215934753](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201215934753.png)

### 8. 成功

关闭防火墙之后再试一下，果然大功告成了！

![image-20210201215523546](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201215523546.png)

### 9. 反思

哎，等等，不对劲，如果我把防火墙全部关闭掉了，虽然可以访问对一个的内部服务，但是服务器没有了防火墙，等于说暴露在巨大的风险之下。不妥，换一种解决方案：激活防火墙，但是只对外开放我们需要使用的端口。

说干就干：

1. 先开启防火墙：

   ```shell
   systemctl start firewalld
   ```

2. 查看开放端口：

   ```shell
   firewall-cmd --list-ports
   ```

3. 添加 80 端口为开放端口：

   ```shell
   firewall-cmd --zone=public --add-port=80/tcp 
   # 命令含义：  --zone #作用域    --add-port=80/tcp #添加端口，格式为：端口/通讯协议    --permanent #永久生效，没有此参数重启后失效
   ```

4. 重新启动防火墙：

   ```shell
   firewall-cmd --reload
   ```

5. 大功告成，具体步骤如下图：

   ![image-20210201221327691](https://gitee.com/Kingshion/imgs/raw/master/imgs/image-20210201221327691.png)

   

