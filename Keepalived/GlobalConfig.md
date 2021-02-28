# Global Config

__Keepalived全局配置__
```
global_defs
{
    notification_email
    {
        packy@xxx.com
        gear-station@xxx.com 
    }
    notification_email_from Keepalived@localhost
    smtp_server 192.168.200.1
    smtp_connect_timeout 30
    router_id LVS_DEVEL   
}
```
| 配置项 | 说明 |
| ---- | ---- |
| notification_email | 用于设置报警邮件地址，可以设置多个，每行一个。注意，如果要开启邮件报警，需要开启本机的Sendmail服务 |
| notification_email_from | 用于设置邮件的发送地址 |
| smtp_server | 用于设置邮件的smtp server地址 |
| smtp_connect_timeout | 用于设置连接smtp server的超时时间 |
| router_id | 表示运行Keepalived服务器的一个标识，是发邮件时显示在邮件主题中的信息 |