# 快速开始 - 独立HBase
&emsp;&emsp;本部分简绍设置单节点独立的HBase。一个独立的实例包含所有的HBase守护进程--主人、区域服务器和动物园管理员--运行到本地文件系统的单个JVM。这是我们最基本的部署配置文件。我们将向你展示如何用`hbase shell`在HBase中创建表，向表中插入行，对表执行放和扫描操作，启用或禁用表，并启动和停止HBase。
## 2.1 安装JDK
## 2.2 开始HBase
### 2.2.1 下载解压
### 2.2.2 配置JAVA_HOME
### 2.2.3 HBase配置文件conf/hbase-site.xml
&emsp;&emsp;你需要指定在文件系统中HBase和ZooKeeper从哪写数据b并且承认某些风险。默认下，会在/tmp下创建新的路径。许多服务器被配置为在重启时删除/tMP的内容，所以你应该将数据存到其他地方。下列的配置将会把数据存到 `hbase`的路径。在被称为测试用户的用户的主目录中.  
&emsp;&emsp;若要在现有的HDFS实例上返回HBase，请将HbaseRooDIr设置为指向您的实例上的目录。  
&emsp;&emsp;注意，表名、行、列都必须用引号字符括起来。
### 2.2.4 启动HBase 
```
$ ./bin/start-hbase.sh
```
&emsp;&emsp;进入HBase Shell:
```
$ ./bin/hbase shell
hbase(main):001:0>
```
#### 1. 创建表
&emsp;&emsp;使用 `create` 命令创建一个新表，你必须指定表的名字以及列族 `(ColumnFamily)` 名称。
```
hbase(main):001:0> create 'test', 'cf'
0 row(s) in 0.4170 seconds

=> Hbase::Table - test
```
#### 2. 列出表中的内容
&emsp;&emsp;使用 `list` 命令验证你的表是否存在。 
```
hbase(main):002:0> list 'test'
TABLE
test
1 row(s) in 0.0180 seconds

=> ["test"]
```  
&emsp;&emsp;现在，使用 `discribe` 命令查看具体信息，包括默认的设置。
```
hbase(main):003:0> describe 'test'
Table test is ENABLED
test
COLUMN FAMILIES DESCRIPTION
{NAME => 'cf', VERSIONS => '1', EVICT_BLOCKS_ON_CLOSE => 'false', NEW_VERSION_BEHAVIOR => 'false', KEEP_DELETED_CELLS => 'FALSE', CACHE_DATA_ON_WRITE =>
'false', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', MIN_VERSIONS => '0', REPLICATION_SCOPE => '0', BLOOMFILTER => 'ROW', CACHE_INDEX_ON_WRITE => 'f
alse', IN_MEMORY => 'false', CACHE_BLOOMS_ON_WRITE => 'false', PREFETCH_BLOCKS_ON_OPEN => 'false', COMPRESSION => 'NONE', BLOCKCACHE => 'true', BLOCKSIZE
 => '65536'}
1 row(s)
Took 0.9998 seconds
```
#### 3. 向表中放入数据
&emsp;&emsp;使用 `put` 命令向表中放入数据。  
```
hbase(main):003:0> put 'test', 'row1', 'cf:a', 'value1'
0 row(s) in 0.0850 seconds

hbase(main):004:0> put 'test', 'row2', 'cf:b', 'value2'
0 row(s) in 0.0110 seconds

hbase(main):005:0> put 'test', 'row3', 'cf:c', 'value3'
0 row(s) in 0.0100 seconds
```
&emsp;&emsp;上述我们插入了3个值，第一个插入的是 `row1`，列 `cf:a`，值是 `value1`。在HBase中列包含列族前缀 `cf`。接着是冒号，然后是列限定符后缀。

#### 4. 一次扫描所有数据
&emsp;&emsp;在HBase获取数据的方式之一是扫描。使用 `scan` 命令扫描表中数据。你可以列出你的扫描。比如下面看到的：
```
hbase(main):006:0> scan 'test'
ROW                                      COLUMN+CELL
 row1                                    column=cf:a, timestamp=1421762485768, value=value1
 row2                                    column=cf:b, timestamp=1421762491785, value=value2
 row3                                    column=cf:c, timestamp=1421762496210, value=value3
3 row(s) in 0.0230 seconds
```

#### 5. 从数据中获取单行
&emsp;&emsp;从数据中一次获取单个行，使用 `get` 命令。
```
hbase(main):007:0> get 'test', 'row1'
COLUMN                                   CELL
 cf:a                                    timestamp=1421762485768, value=value1
1 row(s) in 0.0350 seconds
```
#### 6. 禁用表
&emsp;&emsp;如果你想删除表或者更改它的设置，或者在其他的情况下。你需要首先禁用表，使用 `disable` 命令。你可以再启用表使用 `enable` 命令。

#### 7. 删除表
&emsp;&emsp;删除或者丢弃表，使用 `drop`命令。
#### 8. 退出 HBase Shell
&emsp;&emsp;使用 `quit` 命令断开到群的连接，但是HBase仍然在后台运行。

### 2.2.5 停止HBase 
```
$ ./bin/stop-hbase.sh
stopping hbase....................
$
```

## 2,3 伪分布本地安装
&emsp;&emsp;完成上述独立模式后，你可以重新配置HBase，让他运行在伪分布模式。伪分布模式意味着HBase仍然运行在一个主机上，但是每个HBase守护进程（HMaster, HRegionServer, and ZooKeeper）运行在各自的线程中：在独立模式中所有的守护进程运行在一个jvm进程/实例中。默认的，除非你配置 `hbase.rootdir`属性（如上所述），不然你的数据仍然存储在 /tmp/ 中。在这次教程中，我们使用HDFS存储数据，认为你已经有可用的HDFS。你可以跳过HDFS的配置继续讲数据存储在本地文件系统中。

---
如果你要使用HDFS文件系统，而不是本地文件系统，则需要有可用的HDSF文件系统。