方法1：拿到域成员机器怎么查看域控IP

1. net time /domain  查看当前域时间
2. ping -n 2 域控机器名称

![图片](https://user-images.githubusercontent.com/118274389/216743387-aefa5d76-df85-4d2e-8a02-729cb492282c.png)

方法2：拿到域成员机器怎么查看域控IP
查看域内其他机器 net group "domain computers" /domain
ping 域内机器名称即可查看ip

进入内网后距离靶标差多少网段
ping 靶标  ping DNS 

ping通的话在同网段，Ping不通说明不在一个网段
