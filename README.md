# Xray-TLS+Web搭建/管理脚本
## 目录
[1. 脚本特性](#脚本特性)

[2. 注意事项](#注意事项)

[3. 安装时长说明](#安装时长说明)

[4. 脚本使用说明](#脚本使用说明)

[5. 运行截图](#运行截图)

[6. 伪装网站说明](#伪装网站说明)

[7. 依赖列表](#依赖列表)

[8. 注](#注)
## 脚本特性
1.支持 (Xray-TCP+XTLS) + (Xray-WebSocket+TLS) + Web

2.集成 多版本bbr/锐速 安装选项
 
3.支持多种系统 (Ubuntu CentOS Debian deepin fedora ...) 

4.支持多种指令集 (x86 x86_64 arm64 ...)

5.支持ipv6only服务器 (需自行设置dns64)

6.集成删除阿里云盾和腾讯云盾功能 (仅对阿里云和腾讯云服务器有效)

7.使用Nginx作为网站服务

8.使用Xray作为前置分流器

9.使用acme.sh自动申请域名证书

10.自动更新域名证书
## 注意事项
1.此脚本需要一个解析到服务器的域名 (支持cdn)

2.有些服务器443端口被阻断，使用这个脚本搭建的无法连接

3.此脚本安装时间较长，见 **[安装时长说明](#安装时长说明)**

4.建议在纯净的系统上使用此脚本 (VPS控制台-重置系统)
## 安装时长说明
此脚本的安装时间比较长 (**[安装时长参考](#安装时长参考)**) ，原因见[这里](#为什么脚本安装时间那么长)。

此脚本适合安装一次后长期使用，不适合反复重置系统安装，这会消耗您的大量时间。如果需要更换配置和域名等，在管理界面都有相应的选项。

如果有快速安装的需求，推荐在 **[Xray-core#Installation](https://github.com/XTLS/Xray-core#Installation)** 中选择其他脚本
### 安装时长参考
安装流程：

`[升级系统组件]->[安装bbr]->[安装php]->安装Nginx->安装Xray->申请证书->配置文件->[配置伪装网站]`

其中`[]`包裹的部分是可选项。

**这是一台单核1G的服务器的平均安装时长，仅供参考：**
|项目|时长|
|-|-|
|升级已安装软件|5-10分钟|
|升级系统|10-20分钟|
|安装bbr|3-5分钟|
|安装php|Centos8(gcc8.3 4.18内核):20-60分钟|
||Ubuntu20.10(gcc10.2 5.11-rc3内核):15-20分钟|
||Debian10(gcc8.3 4.19内核):10-15分钟|
|安装Nginx|13-15分钟|
|安装Xray|<半分钟|
|申请证书|1-2分钟|
|配置文件|<100毫秒|
|配置伪装网站|Nextcloud:1-3分钟|
||Cloudreve:1-2分钟|
#### 为什么脚本安装时间那么长？
之所以时间相比别的脚本长，有三个原因：
```
1.集成了安装bbr的功能
2.集成更新系统及软件包的功能
3.(主要原因) 脚本的Nginx和php是采用源码编译的形式，其它脚本通常直接获取二进制程序
```
之所以采用编译的形式，主要考虑的原因为：
```
1.便于管理
2.便于适配多种系统
```
编译相比直接安装二进制文件的优点有：
```
1.运行效率高 (编译时采用了-O3优化)
2.软件版本新 (可以对比本脚本与其他脚本Nginx的版本)
```
缺点就是编译耗时长
## 脚本使用说明
### 1. 安装wget
Debian基系统(包括Ubuntu、Debian、deepin)：
```bash
[[ "$(type -P wget)" ]] || apt -y install wget || (apt update && apt -y install wget)
```
Red Hat基系统(包括CentOS、fedora)：
```bash
[[ "$(type -P wget)" ]] || dnf -y install wget || yum -y install wget
```
### 2. 获取/更新脚本
```bash
wget -O Xray-TLS+Web-setup.sh --no-check-certificate https://github.com/kirin10000/Xray-script/raw/main/Xray-TLS+Web-setup.sh
```
### 3. 执行脚本
```bash
bash Xray-TLS+Web-setup.sh
```
### 4. 根据脚本提示完成安装
## 运行截图
<div>
    <img width="400" src="https://github.com/kirin10000/Xray-script/blob/main/image/menu.jpg">
</div>
<div>
    <img width="600" src="https://github.com/kirin10000/Xray-script/blob/main/image/protocol.jpg">
</div>

## 伪装网站说明
### 伪装网站的作用
这个网站是用你的域名搭建的一个网站，搭建完成后可以直接在浏览器上输入你的域名访问。

你使用Xray进行代理的全部流量将伪装成访问这个网站的流量。

注意伪装网站不是万能的，据部分人的经验，只要你的月流量超过一定限度运营商就会把你封喽，不管你的伪装网站是什么。也就是说哪怕你完全不代理，只是正常访问你的网站访问了太多的流量，也会被封。
### 伪装网站的选择
因为使用Xray进行代理的流量往往比较大，所以要尽量选择能产生大流量的网站。

1. **Cloudreve和Nextcloud(个人网盘)**

使用VPS搭建起来的个人网盘，就像百度网盘那样，只不过文件和账号信息存放在你的VPS里。网盘经常上传/下载文件，产生大流量合情合理。

他们的区别是：Nextcloud功能更强大，用的人更多，但是需要安装php，安装php需要额外很多时间，同时也比Cloudreve占用更多系统资源。

2. **403页面(模拟网站后台)**

基本上大网站都有网站后台。比如哔哩哔哩的网址是`www.bilibili.com`。但是在播放视频时，提供视频文件的却是另外一个网址，在播放视频时右键点击`视频统计信息`，其中的`Video Host`就是。这类网址只有打开特定的url后缀才有内容，如果url不对，返回的就是一个错误页面。

也就是说伪装成403页面，除了你自己，没人知道你的网站到底有没有东西。

3. **自定义静态网站**

自定义的静态网站，不建议小白选择。默认是Nextcloud的登陆界面，强烈建议自行更换，因为这里Nextcloud是静态网站，没有php，无法进行交互，很容易被主动探测出来。

4. **自定义反向代理网站**

不建议选择，因为反向代理往往只是反向代理几个html和js文件，网站里面的大部分内容依然是网站后台提供的。不符合大流量特点。
## 依赖列表
脚本可能自动安装以下依赖：
|用途|Debian基系统|Red Hat基系统|
|-|-|-|
|netstat|net-tools|net-tools|
|lsb_release|lsb-release|redhat-lsb-core|
|wget/curl https|ca-certificates|ca-certificates|
|wget|wget|wget|
|unzip|unzip|unzip|
|curl|curl|curl|
|acme.sh依赖|openssl|openssl|
|acme.sh依赖|crontabs|crontabs|
|编译基础：|||
|gcc|gcc|gcc|
|g++|g++|gcc-c++|
|make|make|make|
|编译openssl：|||
|||perl-IPC-Cmd|
|||perl-Getopt-Long|
|||perl-Data-Dumper|
|编译Nginx：|||
||libpcre3-dev|pcre-devel|
||zlib1g-dev|zlib-devel|
|--with-http_xslt_module|libxml2-dev|libxml2-devel|
|--with-http_xslt_module|libxslt1-dev|libxslt-devel|
|--with-http_image_filter_module|libgd-dev|gd-devel|
|--with-google_perftools_module|libgoogle-perftools-dev|gperftools-devel|
|--with-http_geoip_module|libgeoip-dev|geoip-devel|
|--with-http_perl_module||perl-ExtUtils-Embed|
|--with-libatomic|libatomic-ops-dev|libatomic_ops-devel|
||libperl-dev|perl-devel|
|编译php：|||
||pkg-config|pkgconf-pkg-config|
||libxml2-dev|libxml2-devel|
||libsqlite3-dev|sqlite-devel|
|--with-fpm-systemd|libsystemd-dev|systemd-devel|
|--with-fpm-acl|libacl1-dev|libacl-devel|
|--with-fpm-apparmor|libapparmor-dev||
|--with-openssl|libssl-dev|openssl-devel|
|--with-kerberos|libkrb5-dev|krb5-devel|
|--with-external-pcre|libpcre2-dev|pcre2-devel|
|--with-zlib|zlib1g-dev|zlib-devel|
|--with-bz2|libbz2-dev|bzip2-devel|
|--with-curl|libcurl4-openssl-dev|libcurl-devel|
|--with-qdbm|libqdbm-dev||
|--with-gdbm||gdbm-devel|
|--with-db4|libdb-dev|libdb-devel|
|--with-tcadb|libtokyocabinet-dev|tokyocabinet-devel|
|--with-lmdb|liblmdb-dev|lmdb-devel|
|--with-enchant|libenchant-dev|enchant-devel|
|--with-ffi|libffi-dev|libffi-devel|
|--enable-gd|libpng-dev|libpng-devel|
|--with-external-gd|libgd-dev|gd-devel|
|--with-webp|libwebp-dev|libwebp-devel|
|--with-jpeg|libjpeg-dev|libjpeg-turbo-devel|
|--with-xpm|libxpm-dev|libXpm-devel|
|--with-freetype|libfreetype6-dev|freetype-devel|
|--with-gmp|libgmp-dev|gmp-devel|
|--with-imap|libc-client2007e-dev|libc-client-devel|
|--enable-intl|libicu-dev|libicu-devel|
|--with-ldap|libldap2-dev|openldap-devel|
|--with-ldap-sasl|libsasl2-dev|openldap-devel|
|--enable-mbstring|libonig-dev|oniguruma-devel|
|--with-unixODBC,--with-pdo-odbc|unixodbc-dev|unixODBC-devel|
|--with-pdo-dblib|freetds-dev|freetds-devel|
|--with-pdo-pgsql,--with-pgsql|libpq-dev|libpq-devel|
|--with-pspell|libpspell-dev|aspell-devel|
|--with-libedit|libedit-dev|libedit-devel|
|--with-mm|libmm-dev||
|--with-snmp|libsnmp-dev|net-snmp-devel|
|--with-sodium|libsodium-dev|libsodium-devel|
|--with-password-argon2|libargon2-dev|libargon2-devel|
|--with-tidy|libtidy-dev|libtidy-devel|
|--with-xsl|libxslt1-dev|libxslt-devel|
|--with-zip|libzip-dev|libzip-devel|
|编译php-imagick：|||
||autoconf|autoconf|
||git|git|
||libmagickwand-dev|ImageMagick-devel|
|Nextcloud初始化|sudo|sudo|
## 注
1.本文链接(官网)：https://github.com/kirin10000/Xray-script

2.参考教程：https://www.v2fly.org/config/overview.html https://guide.v2fly.org/ https://docs.nextcloud.com/server/20/admin_manual/installation/source_installation.html https://docs.cloudreve.org/

3.域名证书申请：https://github.com/acmesh-official/acme.sh

4.bbr脚本来自：https://github.com/teddysun/across/blob/master/bbr.sh

5.bbr2脚本来自：https://github.com/yeyingorg/bbr2.sh (Ubuntu Debian) https://github.com/jackjieYYY/bbr2 (CentOS)

6.bbrplus脚本来自：https://github.com/chiakge/Linux-NetSpeed

#### 此脚本仅供交流学习使用，请勿使用此脚本行违法之事。网络非法外之地，行非法之事，必将接受法律制裁！！
