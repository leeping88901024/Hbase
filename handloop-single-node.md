# 搭建单节点群集
&emsp;&emsp;本文档描述如何搭建和设置单节点hadloop安装故而你可以使用Hadoop MapReduce 和 Hadoop Distributed File System(HDFS)快速的执行一些简单的操作。
## 支持平台
* GNU/Linux 支持且作为开发和生产平台。Hadoop已经在GNU/Linux集群上展示了2000个节点。
* Windows也是一个支持的平台，但是下面的步骤只针对Linux。
## 所需软件
* Java
* ssh
## 独立模式
&emsp;&emsp;默认下，Hadoop配置成运行在非分布式模式下，作为单个Java 线程。者对调试很有用。  
&emsp;&emsp;下列示例复制减压的conf文件下