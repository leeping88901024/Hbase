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
&emsp;&emsp;下列示例复制减压缩的conf文件下...  
## 伪分布模式
&emsp;&emsp;Hadoop还可以单节点的伪分布模式，该模式Hadoop的守护进程运行在单个Java线程上。  
* 配置  
&emsp;&emsp;使用下面的配置：  
`etc/hadoop/core-site.xml`:
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
`etc/hadoop/hdfs-site.xml`:
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```  
* 设置无密码的ssh  
```
$ssh localhost
```  
* 执行  
&emsp;&emsp;下面的指令用来在本地运行**MapReduce 任务**，如果你你想在**YARN**上执行任务，请参考[YARN on Single Node](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html#YARN_on_Single_Node)  
1. 格式化文件系统：
```
$ bin/hdfs namenode -format
```
2. 开始 NameNode守护进程和 DataNode 守护进程：
```
$ sbin/start-dfs.sh
```
&emsp;&emsp;Hadoop守护进程日志输出位置在$HADOOP_LOG_DIR（默认在$HADOOP_HOME/logs）。  
3. 浏览 NameNode的网页接口。默认在http://localhost:50070有效。  
4. 制作执行MapReduce作业所需的HDFS目录：
```
$ bin/hdfs dfs -mkdir /user
$ bin/hdfs dfs -mkdir /user/<username>
```  
5. 拷贝输出文件到分布式文件系统：
```
$ bin/hdfs dfs -put etc/hadoop input
```
6. 运行提供的示例：
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.1.jar grep input output 'dfs[a-z.]+'
7. 检查输出文件：将分布式文件从分布式文件系统复制到本地文件系统并检查它们：  
```
 $ bin/hdfs dfs -get output output
 $ cat output/*
```  
或者在分布式文件系统上浏览输出文件：
```
$ bin/hdfs dfs -cat output/*
```  
8. 当你完成的时候，停止守护进程：
```
$ sbin/stop-dfs.sh
```

**总结：**  
&emsp;&emsp;需查看额外的数据，连接其基本的概念、范畴。