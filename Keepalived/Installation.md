# Installation

1. 解压：tar -zxvf keepalived-2.0.15.tar.gz

2. 进入解压后的目录：cd keepalived-2.0.15

3. 指定安装后主要文件目录：sudo ./configure --prefix=/usr/local/keepalived

4. 编译：sudo make & make install

5. 如果出现openssl相关错误，先安装openssl
```Shell
CentOS：yum -y install openssl-devel
RaspberryPi：sudo apt-get install -y libssl-dev
```

6. keepalived启动脚本变量引用文件，默认文件路径是/etc/sysconfig/，也可以不做软链接，直接修改启动脚本中文件路径即可（安装目录下）。如果是RaspberryPi，先创建文件夹sudo mkdir /etc/sysconfig
```Shell
cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/keepalived
```

7. 将keepalived主程序加入到环境变量（安装目录下）
```Shell
cp /usr/local/keepalived/sbin/keepalived /usr/sbin/keepalived
```

8. keepalived启动脚本（源码目录下），放到/etc/init.d/目录下就可以使用service命令便捷调用。后面的版本系统服务会自动安装，该步骤可以省略
```Shell
cp /usr/local/keepalived-2.0.15/keepalived/etc/init.d/keepalived /etc/init.d/keepalived
```

9. 将配置文件放到默认路径下
```Shell
mkdir /etc/keepalived
cp /usr/local/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf
```

10. 设置开机启动：systemctl enable keepalived.service