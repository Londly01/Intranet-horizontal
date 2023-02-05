0x00 不出网有三种可能

     1.被防火墙拦截  2.被上层网络防火墙拦截   3.没有网关
     
0x01 判断方法

     ICMP协议判断可以ping  ping+域名可解析成ip，则DNS可出网
     
     telnet判断，VPS监听一个端口，nc -lvp xxxx   受害机telnet x.x.x.xxx进行连接，连接成功代表出网。

