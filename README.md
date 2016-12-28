# IKEV-Ubuntu-CentOSBash

1.下载脚本
2.运行(如果有需要使用自己已有的根证书，请将私钥命名为ca.cert，将根证书命名为ca.cert.pem，放到脚本的相同目录下再运行该脚本，没有证书的话将自动生成自签名证书咯)：
chmod +x one-key-ikev2.sh
bash one-key-ikev2.sh
3.等待自动配置部分内容后，选择vps类型（OpenVZ还是Xen、KVM），选错将无法成功连接，请务必核实服务器的类型。输入服务器ip或者绑定的域名(连接vpn时服务器地址将需要与此保持一致)，以及证书的相关信息(C,O,CN)，使用自己的根证书的话，C,O,CN的值需要与根证书一致，为空将使用默认值(default value)，确认无误后按任意键继续

vps类型：腾讯云是1  搬瓦工是2 这是测试过的

4.输入两次pkcs12证书的密码(可以为空)

5.看到install success字样即表示安装成功。默认用户名密码将以黄字显示，可根据提示自行修改文件中的用户名密码。(WindowsPhone8.1的用户请将用户名myUserNames修改为%any ,否则可能会由于域的问题无法连接，具体参见http://quericy.me/blog/512文章中的说明)
6.将提示信息中的证书文件ca.cert.pem拷贝到客户端，修改后缀名为.cer后导入。ios设备使用Ikev1无需导入证书，而是需要在连接时输入共享密钥，共享密钥即是提示信息中的黄字PSK.

服务器重启后默认ipsec不会自启动，请自行添加，或使用命令手动开启：
ipsec start
连上服务器后无法链接外网：
vim /etc/sysctl.conf
修改net.ipv4.ip_forward=1后保存并关闭文件 然后使用以下指令刷新sysctl：
sysctl -p
如遇报错信息，请重新打开/etc/syctl并将报错的那些代码用#号注释，保存后再刷新sysctl直至不会报错为止。

脚本来自：
wget https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/master/one-key-ikev2.sh
