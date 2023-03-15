
参考文章
https://www.secpulse.com/archives/180106.html

坑记录
agscript更改如下
java -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -Djava.awt.headless=true -classpath ./cobaltstrike.jar aggressor.headless.Start $*


这样运行 ./agscript 106.15.40.123 50050 neo33 wwww PushPlus.cna 微信才能接收到通知
