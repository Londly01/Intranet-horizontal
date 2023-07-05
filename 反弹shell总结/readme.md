1、判断目标存在哪些反弹shell的命令


   受害机上执行：whereis bash nc exec telnet python php perl ruby java go gcc g++

2、判断目标机器能否通icmp流量

   ping -c 1 22.xxx.xxx.x >icmp.txt
   
   ls -alh
   
   cat icmp.txt
