# Xray-TLS+Web搭建/管理脚本
## 目录
[1. 脚本特性](#脚本特性)

[2. 注意事项](#注意事项)

[3. 安装时长说明](#安装时长说明)

[4. 脚本使用说明](#脚本使用说明)

[5. 运行截图](#运行截图)

[6. 注](#注)
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
此脚本的安装时间比较长，根据VPS的配置以及安装时的选项不同，安装时长在 **5-120分钟** 不等(详见 **[安装时长参考](#安装时长参考)**)

所以本脚本不适合反复重置系统安装，这会消耗您的大量时间。

本脚本适合安装一次后长期使用，如果需要更换配置和域名等，在管理界面都有相应的选项。
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
|安装php|Centos8(gcc8.3 4.19内核):20-60分钟|
||Ubuntu20.10(gcc10.2 5.11-rc3内核):15-20分钟|
||Debian10(gcc8.3 4.19内核):10-15分钟|
|安装Nginx|13-15分钟|
|安装Xray|<半分钟|
|申请证书|1-2分钟|
|配置文件|<100毫秒|
|配置伪装网站|Nextcloud:1-3分钟|
||Cloudreve:1-2分钟|
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

## 注
1.本文链接(官网)：https://github.com/kirin10000/Xray-script

2.参考教程：https://www.v2fly.org/config/overview.html https://guide.v2fly.org/ https://docs.nextcloud.com/server/20/admin_manual/installation/source_installation.html https://docs.cloudreve.org/

3.域名证书申请：https://github.com/acmesh-official/acme.sh

4.bbr脚本来自：https://github.com/teddysun/across/blob/master/bbr.sh

5.bbr2脚本来自：https://github.com/yeyingorg/bbr2.sh (Ubuntu Debian) https://github.com/jackjieYYY/bbr2 (CentOS)

6.bbrplus脚本来自：https://github.com/chiakge/Linux-NetSpeed

#### 此脚本仅供交流学习使用，请勿使用此脚本行违法之事。网络非法外之地，行非法之事，必将接受法律制裁！！
