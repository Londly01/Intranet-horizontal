
	CS微信提示设置agscript为如下脚本

java -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -Djava.awt.headless=true -classpath ./cobaltstrike.jar aggressor.headless.Start $*
