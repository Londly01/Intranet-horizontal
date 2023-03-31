主动扫描

    ping命令扫描内网中的存活主机
        优点:方便，一般不会引起流量检测设备的报警
        缺点：扫描速度慢，目标开了防火墙会导致结果不准
    nmap扫描存活主机(icmp扫描)
        nmap -sn -PE -n -v -oN 1.txt 目标ip
        参数： -sn 不进行端口扫描;-PE 进行icmp echo扫描;-n 不进行反向解析;-v 输出调试信息;-oN输出
    nmap 扫描存活主机(arp扫描)
        nmap -sn -PR -n -v 目标IP
        参数：-PR代表arp扫描，在内网中arp扫描速度最快且准确率高
    使用netdiscover扫描(arp扫描工具，既可以主动扫描也可以被动嗅探)
        netdiscover -i eth0 -r 目标IP
        
 扫描主机存在的CVE漏洞

    nmap --script=vuln 目标IP
    
    
 进入内网后跨网段扫描
 
    推荐使用R3start师傅的工具 https://github.com/r35tart/GetIPinfo
    
 进入内网对当前网段进行密码喷洒
    可躲避IDS，IPS检测 KALI自带也可以下载下面的
 
    推荐项目 https://github.com/Porchetta-Industries/CrackMapExec
    使用指南 https://fdlucifer.github.io/2020/12/14/crackmapexec/
    
 内网扫描fscan
    项目地址：https://github.com/shadow1ng/fscan
  
 可达网段扫描
 
    项目地址：https://github.com/shmilylty/netspy
    
 进入内网后碰见防火墙扫描
 
    项目地址:https://github.com/ybdt/post-hub/tree/main/%E8%BF%87%E9%98%B2%E7%81%AB%E5%A2%99%E7%AB%AF%E5%8F%A3%E6%89%AB%E6%8F%8F%E5%99%A8
   

 
    


