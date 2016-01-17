# 一键搭建适用于Ubuntu/CentOS的IKEV2/L2TP的VPN

------
使用bash脚本一键搭建Ikev2的vpn

说明
=============
> * 服务端要求：Ubuntu或者CentOS-6.*
> * 客户端：
 - iOS=>ikev1
 - Andriod=>ikev1
 - WindowsPhone=>ikev2
 - 其他Windows平台=>ikev2
> * 可使用自己的私钥和根证书，也可自动生成
> * 证书可绑定域名或ip
> * 要是图方便可一路回车

使用方法
==========
1.下载脚本:
```shell
 wget https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/master/one-key-ikev2.sh
```
2.运行(**如果有需要使用自己的根证书请将私钥命名为ca.pem，将根证书命名为ca.cert.pem，放到脚本的相同目录下再运行该脚本**)：
```shell
chmod +x one-key-ikev2.sh
bash one-key-ikev2.sh
```

3.等待自动配置部分内容后，选择vps类型（OpenVZ还是Xen、KVM），**选错将无法成功连接，请务必核实服务器的类型**。输入服务器ip或者绑定的域名(连接vpn时服务器地址将需要与此保持一致)，以及证书的相关信息(C,O,CN)，为空将使用默认值(default value)，确认无误后按任意键继续
      (1、使用virt-what
      
      >apt-get install virt-what
      或者
      >yum install virt-what
      >virt-what
      
      就会显示结果。
      
      2、ls /proc/命令，一般Xen的VPS，/proc目录下面会有xen的目录，openvz的会有vz目录。
      
      3、free -m命令看内存，openvz的没有swap，当然也有xen的没有swap，但是xen的是可以加的，openvz不行。
      
      4、uname -a命令，有些xen的VPS里面会显示有xen。
      
      5、ifconfig命令查看网卡，openvz的一般都是venet0 ，xen的一般都是eth*。
     )
4.输入两次pkcs12证书的密码(可以为空)

5.看到install success字样即表示安装成功。默认用户名密码将以黄字显示，可根据提示自行修改文件中的用户名密码。

6.将提示信息中的证书文件ca.cert.pem拷贝到客户端，修改后缀名为.cer后导入。ios设备使用Ikev1无需导入证书，而是需要在连接时输入共享密钥，共享密钥即是提示信息中的黄字PSK.

PS:
======
* 服务器重启后默认ipsec不会自启动，请自行添加，或使用命令手动开启：
```bash
ipsec start
```
* 连上服务器后无法链接外网：
```bash
vim /etc/sysctl
```
修改net.ipv4.ip_forward=1后保存并关闭文件
然后使用以下指令刷新sysctl：
```bash
sysctl -p
```
如遇报错信息，请重新打开/etc/syctl并将报错的那些代码用#号注释，保存后再刷新sysctl直至不会报错为止。

如有其他疑问请戳本人博客：[http://quericy.me/blog/699](http://quericy.me/blog/699)
