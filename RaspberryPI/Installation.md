# Installation

1. __设置位置和语言__
* 使用sudo raspi-config，进入第4项，Locale设置为en_US.UTF-8，Timezone设置为当前所在位置。

2. __设置固定IP__
* 使用sudo raspi-config设置WIFI连接（必须先设置国家，WIFI才能生效），sudo nano /etc/dhcpcd.conf添加以下内容：
```Shell
interface wlan0
# 指定静态IP，/24表示子网掩码为 255.255.255.0
static ip_address=192.168.1.20/24
# 路由器/网关IP地址
static routers=192.168.1.1
# 手动自定义DNS服务器
static domain_name_servers=192.168.1.1
```
* 修改完成后，按Ctrl + x 快捷键，输入y，保存退出。然后用sudo reboot重启。

3. __更新软件__
```Shell
输入sudo apt-get update更新软件信息列表
输入sudo apt-get upgrade开始更新软件
```

4. __开启SSH__
* 使用sudo raspi-config进入interface选项开启

5. __解锁root账户__
* 使用账户pi登陆，输入sudo passwd root，系统会提示输入两次密码这就是你设定的root密码。
* 输入解锁账户命令sudo passwd --unlock root。这样的话你的root账户就激活了,从普通账户切换到root账户直接输入su root就可以了。同样的道理，从root账户切换到普通账户输入su pi。
* 虽然我们激活了root账户 但是远程登录root账户的时候还是会被拒绝，这个时候是因为在/etc/ssh/sshd_config里面的内容没有修改。输入sudo nano /etc/ssh/sshd_config打开配置文件。将#Authentication后面5行前的#去掉，并将第二行修改为PermitRootLogin yes，修改好后的样子是：
```Shell
# Authentication:
LoginGraceTime 120
PermitRootLogin yes
StrictModes yes
MaxAuthTries 6
MaxSessions 10
```
* 输入sudo reboot，重启，root账号已经解锁并可以远程登录。