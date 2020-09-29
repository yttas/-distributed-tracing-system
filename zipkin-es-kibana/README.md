
一  ~~~~~~~~~~~~~~~~~~~~~~~~~
zipkin 
源码Git:https://github.com/openzipkin/zipkin

1.zipkin的下载
zipkin的使用： curl -sSL https://zipkin.io/quickstart.sh | bash -s 

如果用用curl觉得下载太慢的话，可以直接copy git链接
https://repo1.maven.org/maven2/io/zipkin/zipkin-server/2.21.7/zipkin-server-2.21.7-exec.jar

历史版本：https://repo1.maven.org/maven2/io/zipkin/zipkin-server/

二 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

背景：
一开始使用的zipkin2.21.6可以实现持续化存储，但是没法调出依赖图，之后使用了zipkin-server的基础源码是可以调出依赖图但是不能持久化存储
9.29主要是需要找出这个问题是为什么

问题解决方法：
关于dependence和持续化不能兼顾的问题：
https://blog.csdn.net/songhaifengshuaige/article/details/79205047
https://www.jianshu.com/p/ae4ee523b87c
还有一种方法：https://bbs.huaweicloud.com/blogs/121787（不太行这种方法）

解决方法：
根据zipkingit里面的
Note: This store requires a job to aggregate dependency links.

大概原因是为了实现持久化，把dependencies跟zipkin-server隔离开进行存储操作。

dependencies的Git链接
zipkin-dependence:https://github.com/openzipkin/zipkin-dependencies

快速开始：curl -sSL https://zipkin.io/quickstart.sh | bash -s io.zipkin.dependencies:zipkin-dependencies:LATEST zipkin-dependencies.jar
跟上面一样觉得慢的话，在linux下执行完这个命令会有一个链接；

下载完之后：
执行(这个官方git上的提示是必须在jdk1.8或者1.9环境下，我的Linux开始是11，就会报错)
STORAGE_TYPE=elasticsearch ES_HOSTS=http://localhost:9200 java -jar zipkin-dependencies.jar
会出现processing span from zipkin-span-2020-09_29
      save dependency links to zipkin-dependece-2020-09_29
然后点击zipkin的dependencies就能看到依赖图了
