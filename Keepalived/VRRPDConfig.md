# VRRPD Config

__VRRPD配置__
```
vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 151
    priority 100
    advert_int 1
    track_interface {
        eth0
        eth1
    } 
    authentication {
        auth_type PASS
        auth_pass gearstation
    }
    virtual_ipaddress {
        192.168.200.16
        192.168.200.17 dev eth1
        192.168.200.18 dev eth2
    }
    nopreempt
    preemtp_delay 300
    notify_master "/etc/keepalived/master.sh"
    notify_backup "/etc/keepalived/backup.sh"
    notify_fault "/etc/keepalived/fault.sh"
}
```
| 配置项 | 说明 |
| ---- | ---- |
| vrrp_instance | 是VRRP实例开始的标识，后跟VRRP实例名称 |
| state | 用于指定Keepalived的角色，MASTER表示此主机是主服务器，BACKUP表示此主机是备用服务器 |
| interface | 用于指定HA监测网络的网卡 |
| lb_kind | 设置LVS实现负载均衡的机制，有NAT、TUN和DR三个模式可选 |
| virtual_router_id | 是虚拟路由标识，这个标识是一个数字，同一个vrrp实例使用唯一的标识，即在同一个vrrp_instance下，MASTER和BACKUP必须是一致的 |
| priority | 用于定义节点优先级，数字越大表示节点的优先级就越高。在一个vrrp_instance下，MASTER的优先级必须大于BACKUP的优先级 |
| advert_int | 用于设定MASTER与BACKUP主机之间同步检查的时间间隔，单位是秒 |
| track_interface | 用于设置一些额外的网络监控接口，其中任何一个网络接口出现故障， Keepalived都会进入FAULT状态 |
| authentication | 用于设定节点间通信验证类型和密码，验证类型主要有PASS和AH两种，在一个vrrp_instance下，MASTER与BACKUP必须使用相同的密码才能正常通信 |
| virtual_ipaddress | 于设置虚拟IP地址（VIP），又叫做漂移IP地址。可以设置多个虚拟IP地址，每行一个。之所以称为漂移IP地址，是因为Keepalived切换到Master状态时，这个IP地址会自动添加到系统中，而切换到BACKUP状态时，这些IP又会自动从系统中删除。Keepalived通过"ip address add"命令的形式将VIP添加进系统中。要查看系统中添加的VIP地址，可以通过"ip add"命令实现。"virtual_ipaddress" 段中添加的IP形式可以多种多样，例如可以写成 "192.168.16.189/24 dev eth1" 这样的形式，而Keepalived会使用IP命令"ip addr add 192.168.16.189/24 dev eth1"将IP信息添加到系统中。因此，这里的配置规则和IP命令的使用规则是一致的 |
| nopreempt | 设置的是高可用集群中的不抢占功能。在一个HA Cluster中，如果主节点死机了，备用节点会进行接管，主节点再次正常启动后一般会自动接管服务。这种来回切换的操作，对于实时性和稳定性要求不高的业务系统来说，还是可以接受的，而对于稳定性和实时性要求很高的业务系统来说，不建议来回切换，毕竟服务的切换存在一定的风险和不稳定性，在这种情况下，就需要设置nopreempt这个选项了。设置nopreempt可以实现主节点故障恢复后不再切回到主节点，让服务一直在备用节点工作，直到备用节点出现故障才会进行切换。在使用不抢占时，只能在"state"状态为"BACKUP"的节点上设置，而且这个节点的优先级必须高于其他节点 |
| preemtp_delay | 用于设置切换的延时时间，单位是秒。有时候系统启动或重启之后网络需要经过一段时间才能正常工作，在这种情况下进行发生主备切换是没必要的，此选项就是用来设置这种情况发生的时间间隔。在此时间内发生的故障将不会进行切换，而如果超过"preemtp_delay"指定的时间，并且网络状态异常，那么才开始进行主备切换 |
| notify_master | 指定当Keepalived进入Master状态时要执行的脚本，这个脚本可以是一个状态报警脚本，也可以是一个服务管理脚本。Keepalived允许脚本传入参数，因此灵活性很强 |
| notify_backup | 指定当Keepalived进入Backup状态时要执行的脚本，同理，这个脚本可以是一个状态报警脚本，也可以是一个服务管理脚本 |
| notify_fault | 指定当Keepalived进入Fault状态时要执行的脚本，脚本功能与前两个类似 |
| notify_stop | 指定当Keepalived程序终止时需要执行的脚本 |