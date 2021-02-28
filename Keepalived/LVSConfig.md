# LVS Config

由于Keepalived属于LVS的扩展项。因此，Keepalived可以与LVS无缝整合，轻松搭建一套高性能的负载均衡集群系统。LVS段的配置以"virtual_server"作为开始标识，此段内容有两部分组成，分别是real_server段和健康检测段。

1. __VIRTUAL_SERVER配置__
```
virtual_server 192.168.12.200 80 {
    delay_loop 6
    lb_algo rr
    lb_kind DR
    persistence_timeout 50
    persistence_granularity  <NETMASK>
    protocol TCP
    sorry_server <IPADDR>  <PORT>
    real_server 192.168.12.132 80 {
        weight 3
        inhibit_on_failure
        notify_up  <STRING> | <QUOTED-STRING>
        notify_down <STRING> | <QUOTED-STRING>
        HEALTH_CHECK {
            ...
        }
    }
}
```
| 配置项 | 说明 |
| ---- | ---- |
| virtual_server | 设置虚拟服务器的开始，后面跟虚拟IP地址和服务端口，IP与端口之间用空格隔开 |
| delay_loop | 设置健康检查的时间间隔，单位是秒 |
| lb_algo | 设置负载调度算法，可用的调度算法有 rr、wrr、lc、wlc、lblc、sh、dh 等，常用的算法有rr和wlc |
| lb_kind | 设置LVS实现负载均衡的机制，有NAT、TUN和DR三个模式可选 |
| persistence_timeout | 会话保持时间，单位是秒。这个选项对动态网页是非常有用的，为集群系统中的session共享提供了一个很好的解决方案。有了这个会话保持功能，用户的请求会一直分发到某个服务节点，直到超过这个会话的保持时间。需要注意的是，这个会话保持时间是最大无响应超时时间，也就是说，用户在操作动态页面时，如果在50秒内没有执行任何操作，那么接下来的操作会被分发到另外的节点，但是如果用户一直在操作动态页面，则不受 50 秒的时间限制 |
| persistence_granularity | 此选项是配合persistence_timeout的，后面跟的值是子网掩码，表示持久连接的粒度。默认是255.255.255.255，也就是一个单独的客户端IP。如果将掩码修改为255.255.255.0，那么客户端IP所在的整个网段的请求都会分配到同一个real server上 |
| protocol | 指定转发协议类型，有TCP和UDP两种可选 |
| ha_suspend | 节点状态从Master到Backup切换时，暂不启用real server节点的健康检查 |
| sorry_server | 相当于一个备用节点，在所有real server失效后，这个备用节点会启用 |
| real_server | real_server	是real_server段开始的标识，用来指定real server节点，后面跟的是real server的真实IP地址和端口，IP与端口之间用空格隔开 |
| weight | 用来配置real server节点的权值。权值大小用数字表示，数字越大，权值越高。设置权值的大小可以为不同性能的服务器分配不同的负载，为性能高的服务器设置较高的权值，而为性能较低的服务器设置相对较低的权值，这样才能合理地利用和分配了系统资源 |
| inhibit_on_failure | 表示在检测到real server节点失效后，把它的"weight"值设置为0，而不是从IPVS中删除 |
| notify_up | 此选项与上面介绍过的notify_maser有相同的功能，后跟一个脚本，表示在检测到real server节点服务处于UP状态后执行的脚本 |
| notify_down | 表示在检测到real server节点服务处于DOWN状态后执行的脚本 |


2. __HEALTH_CHECK健康检测段允许多种检查方式，常见的有 HTTP_GET、SSL_GET、TCP_CHECK、SMTP_CHECK、MISC_CHECK__
* TCP_CHECK配置
```
TCP_CHECK {
    connect_port 80
    connect_timeout 3
    nb_get_retry 3
    delay_before_retry 3
}
```
| 配置项 | 说明 |
| ---- | ---- |
| connect_port | 健康检查的端口，如果无指定，默认是real_server指定的端口 |
| connect_timeout | 表示无响应超时时间，单位是秒，这里是3秒超时 |
| nb_get_retry | 表示重试次数，这里是3次 |
| delay_before_retry | 表示重试间隔，这里是间隔3秒 |

* HTTP_GET和SSL_GET配置
```
HTTP_GET | SSL_GET {
    url {
        path /index.html
        digest e6c271eb5f017f280cf97ec2f51b02d3
        status_code 200
    }
    connect_port 80
    bindto 192.168.12.80
    connect_timeout 3
    nb_get_retry 3
    delay_before_retry 2
}
```
| 配置项 | 说明 |
| ---- | ---- |
| url | 用来指定HTTP/SSL检查的URL信息，可以指定多个URL |
| path | 后跟详细的URL路径 |
| digest | SSL检查后的摘要信息，这些摘要信息可以通过genhash命令工具获取。例如：genhash -s 192.168.12.80 -p 80 -u /index.html |
| status_code | 指定HTTP检查返回正常状态码的类型，一般是200 |
| bindto | 表示通过此地址来发送请求对服务器进行健康检查 |

* MISC健康检查方式可以通过执行一个外部程序来判断real server节点的服务状态，使用非常灵活。
```
MISC_CHECK {
    misc_path /usr/local/bin/script.sh
    misc_timeout  5
    ! misc_dynamic
}
```
| 配置项 | 说明 |
| ---- | ---- |
| misc_path | 用来指定一个外部程序或者一个脚本路径 |
| misc_timeout | 设定执行脚本的超时时间 |
| misc_dynamic | 表示是否启用动态调整real server节点权重，"misc_dynamic"表示不启用，相反则表示启用。在启用这功能后，Keepalived的healthchecker进程将通过退出状态码来动态调整real server节点的"weight"值，如果返回状态码为0，表示健康检查正常，real server节点权重保持不变；如果返回状态码为1，表示健康检查失败，那么就将real server节点权重设置为0；如果返回状态码为2~255之间任意数值，表示健康检查正常，但real server节点的权重将被设置为返回状态码减2，例如返回状态码为10，real server节点权重将被设置为8 |
| delay_before_retry | 表示重试间隔，这里是间隔3秒 |