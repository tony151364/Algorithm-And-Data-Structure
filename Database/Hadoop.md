## 第1章 Hadoop概述

## 第2章 Hadoop基础环境配置

### 一、Linux相关
一、设置静态Ip地址
  1.在 /etc/sysconfig/network-scripts 目录下 编辑（Vim） ifcfg-eth0
  2、完成后通过ifconfig命令查看

二、Jdk安装和配置
  1.解压下载好的文件到相应的文件夹: tar -zxvf  /xxx/xxx/jdk-7u79-linux-x64.tar.gz -C /xxx
  2.配置jdk环境变量：vim /etc/profile
  3.JAVA_HOME=/simple/jdk1.7.0_79    (jdk所在位置）
    export PATH=$JAVA_HOME/bin;$PATH (添加命令）

三、修改主机名和映射关系
  1.关联主机名和IP地址的关系：vim /etc/sysconfig/network
    该文件用于为主机起名字可用 uname -n 查看主机名

  2. vim /etc/hosts
    解析主机名和IP地址的映射关系。如果想使用计算机名称来访问对方的主机，需要把对方计算机的名称和IP地址写到本机的hosts文件中

  3.重启后即可生效：reboot

四、Linux常用命令
	配置文件后立即生效：source /etc/profile

五、在Linux下打印 HelloWorld
  javac HelloWorld.java
  java HelloWorld

六、拼音输入法安装
  1.任何目录下输入：yum list | grep ibus* 可查看ibus相关的yum软件包
  2.可查询到"ibus-pinyin.x86_64",当拼音输入发已存在通过"yum remove ibus*"进行卸载
  3.在命令终端输入命令：yum install ibus-pinyin.x86_64
  
### 二、Hadoop核心文件解释
1.SecondaryNode的作用？
  协助NameNode对资源的应用和备份，避免edit log文件过大，减少NameNode重启所用的时间，因为Secondary Node 对fsimage有自己的备份，NameNode重启时可直接用。
注：
fsimage - 它是在NameNode启动时对整个文件系统的快照
edit logs - 它是在NameNode启动后，对文件系统的改动序列

2.hdfs-env.sh
    配置Java运行环境（jdk）

3.hdfs-site.xml
    1.指定hdfs保存数据的副本数量
    2.指定hdfs中NameNode的存储位置
    3.指定hdfs中DataNode的存储位置

4.mapred-site.xml
    1.告诉hadoop以后MR(Map/Reduce)运行在YARN上

5.yarn-site.xml
    资源管理和调配
    1.nomenodeManager获取数据的方式是shuffle
    2.指定Yarn的老大(ResourceManager)的地址
    3.Yarn打印工作日志

6.core-site.xml
    1.指定NameNode的地址
    2.指定元数据存放的位置（使用Hadoop时产生的文件）
    3.设置检查点备份日志的最长时间


操作步骤（简略）：
1.下载Java和下载Hadoop

2.安装Java并配置相应的环境变量

3.解压Hadoop文件并添加Hadoop的安装目录到环境变量

4.在hadoop-env.sh配置Java运行环景

5.在core-site.xml中指定NameNode的地址，以及元数据存放的位置

6.在hdfs-site.xml中设置副本的数量，指定NameNode的存储位置和DateNode的存储位置

7.配置mapred-site.xml，指定map/Reduce运行在yarn上

8.配置yarn-site.xml，指定yarn的老大（RescourceManager）的地址和reduce获取数据的方式

9.格式化namenode：hdfs namenode --format

10.启动hadoop：start-dfs.sh

11.启动yarn：start-yarn.sh

12jps查看进程运行是否正常

13start-all.sh同时启动hdfs和yran

## 第3章 分布式存储系统HDFS

### 一、分布式存储HDFS
- 支持以流式数据访问模式来存取超大文件
- 把海量数据部署在价格低廉的节点上，通过这种方式可以解决高容错性(fault-tolerant)。
-  

### 二、hdfs shell 基本命令操作
1.查看文件：hadoop dfs -ls /
2.上传文件：hadoop dfs -put /simple/newFile.txt /
3.下载文件：hadoop dfs -get /newFile.txt
4.查看文件内容：hadoop dfs -cat /newFile.txt
5.查看进程：jps
6.查看所有hdfs shell 命令解释：hadoop dfs -help
7.查看所有管理命令：hadoop dfsadmin -help
8.在Hadoop中移动文件：hadoop dfs -mv /newFile.txt /tmp/
9.授予权限：hadoop dfs chmod -R 755(777) /


10.追加本地内容到hdfs：hadoop dfs -appendToFile /simple/word.txt /words.txt
11.循环删除hdfs系统中的目录：hadoop dfs -rmr /words.txt
12.在hdfs系统指定的目录下创建一个文件：hadoop dfs -touchz /tmp/newFile.txt
13.在hdfs中指定位置创建目录：hadoop dfs -mkdir -p /aa/bb/cc
14.将本地文件移动到hdfs：hadoop dfs -moveFromLocal /simple/eclips /
15.修改hdfs系统中指定文件或文件夹的用户所属组：hadoop dfs -chgrp -R root /
16.改变指定文件夹的权限：hadoop dfs -chmod -R 777 /
17.改变文件的所有者，用户必须是超级用户：hadoop dfs -chown -R root:supergroup /

二、Linux下安装Eclipse并导入hadoop所需的Java包

三、验证DataNode存储的块信息
hdfs默认的block大小是128MB,可在hdfs-site.xml的dfs.replication属性

## 第4章、计算系统MapReduce
### 一、概念
- 主要解决海量大数据计算的，是众多分布式计算模型中比较流行的一种，一般配合HDFS使用
- 对Hadoop来说，MapReduce是一个分布式计算框架。归纳起来就是“分而治之，迭代汇总”。就是把一个大的任务拆解开来，分成一系列小的任务并执行，使得这些任务快速解决。

- MapReduce由两个阶段组成：
    - map():任务分解
    - - reduce():结果汇总 

### 二、MapReduce基础

1.架构：MapReduce编程模型、数据处理模型、MapReduce运行环境（JobTrack和TaskTrack）

2.版本：MRV1、MRV2；MRV1的弊端是JobTrack管理所有任务，当任务过多是，JobTrack的负载过重，造成系统不能正常运行。

3.任务：
	1.MapReduce编程模型对任务抽象成map和reduce处理的任务；
	2.数据处理引擎负责任务运行的数据处理，包括数据分片，任务数据的输入输出。
	3.MapReduce运行环境为程序的运行提供支持，如节点通通信，任务分配和调度，资源管理

4.数据操作：
	数据分片：分片操作一般有一个Mapper进行处理，所谓的分片只是逻辑上的分片并不需要进行物理上的划分。具体的分片细节有InputSplitFormat指定

5.MapReduce执行过程概括：
  5.1MapReduce运行的时候通过Mapper运行的任务读取HDFS中的数据文件，然后调用自己的方法，处理数据，最后输出。Reduce任务会接受Mapper任务的输出的数据，作为自己的输入数据，调用自己的方法，最后输入到HDFS文件中
  5.2Mapper接受键值对型的数据，并处理成键值对形的数据

6.Mapper执行过程
  （1）对文件数据分片（InputSplit），每一个输入片由一个Mapper进程处理。
  （2）将输入片解析为键值对
  （3）调用Mapper类中的map方法。每个键值对调用一次map方法。
  （4）对键值对进行分区，分区的数量就是Reduce任务运行的数量
  （5）对键值对进行排序。
  （6）对数据进行规约处理（reduce）。键相等的键值对会调用一次reduce方法。

7.Reducer执行过程
  Reducer任务接受Mapper任务的输出，规约处理后写入到HDFS中。
  （1）Reduce主动从Mapper复制输入的键值对
  （2）将数据进行合并
  （3）对排序后的键值对调用reduce方法。最后把这些输出的键值对写入到HSFS中
注：我们的任务是覆盖map函数和reduce函数

2.Shuffle过程（MapReduce的核心过程）
  Shuffle过程从Mapper产生的输出数据开始，经过一系列的处理，最终成为Reduce的直接输入数据的整个过程。
  Mapper端 --> reducer端 --> Megesort

二、MapReduce编程步骤
（1）新建Mapper类，重写Mapper类中的map()方法
（2）新建Reducer类，重写Reducer类中的reduce()方法
（3）新建程序主类
（4）提交程序到Hadoop集群（在此之前，集群已启动）
  
三、MapReduce输入的处理类
1.FileInputFormat（保存job输入的文件，split对输入计算）
	验证作业输入是否规范，把输入文件分成InputSplit，提供RecordReader
2.InputSplit划分文件

----------------------------------------------------------------------------
简述MapReduce处理数据过程：
map：
1.Mapper通过Mapper运行的任务读取HDFS中的数据文件，并分片为输入片

2.Mapper将输入片解析为键值对，调用map方法对键值对进行分区、排序

3.Reduce主动从Map复制输入的键值对，对排序后的键值对调用reduce方法

4.Reduce最后把这些键值对输出写入到HDFS中

## 第5章 计算模型Yarn
### 一、Yarn 概述
- Yarn是由早期的MapReduce经过架构变换而来的，主要思路是把原有MapReduce架构分为资源资源管理和监控调度两部分，舍弃MRv1中的JobTrack和TaskTrack，采用MRAppMaster进行管理

1.组成成分：
（1）ResourceManager（守护线程）
（2）NodeManger（守护线程）
（3）ApplicantionMaster
（4）Container
（5）MapTask
（6）ReduceTask

## 第6章 数据云盘

## 第7章 协调系统Zookeeper

1.Zookeeper是一个能够高效开发的维护分布式的开放源码的用于协调服务，是Google的Chubby的开源实现。

2.Zookeeper的数据模型
	（1）类似标准的文件系统；znode唯一标示；
	（2）ephemeral 类型的节点不能有子节点。
	（3）多版本
	（4）znode是临时节点，失去联系时被删除。心跳联系session
	（5）znode的目录自动编号
	（6）znode被监控，Zookeeper的重要功能

3.Zookeeper的特征
	（1）数据模型：文件系统
	（2）会话及状态：connecting，connected
	（3）watches：是一次性触发器，Zookeeper为所有的读操作设置watch

4.Zookeeper的工作原理
	（1）通过原子广播实现个server的同步。该广播以Zab协议实现。恢复模式，广播模式。
	（2）Zookeeper服务一直维持在Broadcast的状态，知道leader崩溃或者失去大部分followers（进入恢复模式）
	（3）广播保证proposal按顺序处理
	（4）进入恢复模式后就会选leader

5.Zookeeper术语
	（1）节点有ephemeral和persistent种
	（2）角色：leader、follower、observer（为了扩展系统，提高读取速度）
	（3）顺序号
	（4）观察：watcher
	（5）选举  

6.Zookeeper文件配置：
	设置伪集群配置参数
	添加伪集群配置
	修改端口号
 
7.服务端客户端常用命令
创建节点：				create /nodel test
					create [-s] [-e] path data acl
修改已存在节点的数据：          	set /nodeltt
获得节点数据以及节点其他信息：  	get/nodel
查询子节点个数：               	 	ls /
查看当前节点概况（不显示子节点）：	stat /
查看当前节点概况（包括子节点列表）：	ls2 /
设置配额:				setquota -n 2/nodel
查看配额：				listquota /nodel
Questions：
	1. 句柄是什么？


## 第8章 Hadoop数据库Hbase
- Hbase是一个分布式的，面向列的开源数据库，可以称为Hadoop的标准数据库，也是一款比较流行的NoSQL数据库。Hbase在Hadoop之上提供了类似Bigtable的能力，主要解决非关系型数据存储问题。
- 
### 一、概述
（1）Hbase是一个可分布，可扩展的大数据存储的Hadoop数据库。适用于随机、实时读写大数据操作是使用
（2）Hbase是一个分布式的、面向列的开源数据库
（3）Hbase - Hadoop Database，是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统

## 第9章 数据仓库Hive
 内置函数：分为数学函数和聚合函数
SHOWFUNCTIONS 列出当前Hive  // 回话中所加载的所有函数名称
DESCRIBE FUNCTION contact;  // 可显示函数对应的简短介绍
UDF 可直接应用于select语句，对查询结构做格式化处理后，再输出内容。
	（4）ACL：Zookeeper使用ACL对Znode进行访问。无节点所有者概念
	（5）一致性保护：保护Zookeeper的数据读写，旧数据同样享用。
	特点：
		·一次性触发
		·事件触发后发给客户端
		·数据被设置watch
        
## 第10章 Hadoop数据采集Flume
- cloudear（云纪元（公司名））
- Flume是cloudear提供的高可用，高可靠，分布式的海量数据收集系统，从多种源数据系统采集、聚集和移动大量的数据并集中存储。

## 第11章 OTA离线数据分析平台
