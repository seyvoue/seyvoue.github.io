---
title: '@在 CentOS Linux 中安装 Hadoop-1.2.1 ( single-node cluster)'
comments: true
categories: tutorial
abbrlink: 17188a01
date: 2017-07-21 22:00:23
updated: 2018-01-18 03:05:03
tags: hadoop
keywords:
description:
---

本文介绍如何在 CentOS7 中以伪分布式模式 ( Pseudo-Distributed Mode ) 运行 Hadoop-1.2.1。


## 前期准备
### 安装 Linux 系统

可参考这篇“[@Vmware Funsion8.5中安装 CentOS 7(macOS Sierra)](http://www.jianshu.com/p/706281de41c2)”

建议在虚拟机下安装 Linux 系统，并善用虚拟机的 snapshot 功能，可以节省很多时间。


---

### 安装 JDK1.8 并设置环境变量

可参考这篇"[@Linux 系统下 JDK 安装与配置](http://www.jianshu.com/p/aae90f93b67b)"

---

## Hadoop 的安装及配置

> Hadoop 版本有很多，建议安装 `hadoop.1.2.1` 或 hadoop 的最新版本。本文以 `hadoop-1.2.1` 为例。另外，[Hadoop Wiki](https://wiki.apache.org/hadoop) 上有更系统更权威的资料。

### step1：下载安装 Hadoop

- 在 [Apache Download Mirrors](http://www.apache.org/dyn/closer.cgi/hadoop/common) 下载 [hadoop-1.2.1.tar.gz](http://ftp.cuhk.edu.hk/pub/packages/apache.org/hadoop/common/hadoop-1.2.1/) ( 或者下载 [hadoop-2.8.0.tar.gz](http://ftp.cuhk.edu.hk/pub/packages/apache.org/hadoop/common/hadoop-2.8.0/) )
- 将下载的包解压缩到 `/usr`，如下：

```shell
# 切换到 root 身份
[zodas@localhost ~]$ su
# 将下载到桌面上的包 hadoop-1.2.1.tar.gz 拷贝到 /opt/installpackages 目录下
[root@localhost zodas]# cp ./Desktop/hadoop-1.2.1.tar.gz /opt/installpackages
# 切换工作目录到 /opt
[root@localhost zodas]# cd /opt
# 将 hadoop-1.2.1.tar.gz 解压到 /opt 目录下
[root@localhost opt]# tar -xvzf /opt/installpackages/hadoop-1.2.1.tar.gz

```
### step2：修改环境变量 /etc/profile

- 打开终端输入`vi /etc/profile`，增加以下内容，配置 Hadoop 环境变量：

```shell
# Hadoop 
export HADOOP_HOME=/opt/hadoop-1.2.1
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

```
- 打开终端输入`source /etc/profile`，使新添加的环境变量立即生效
- 打开终端输入`hadoop version`，验证环境变量是否配置成功

_注：由于是在 /etc/profile 中配置的环境变量，所以建议配置完后重启一次系统，让环境变量永久生效_ 

### step3：配置hadoop
`Hadoop-1.2.1` 的配置文件默认是放在其 `conf/` 目录下，我们只需要配置其中的4个文件即可，分别是：`hadoop-env.sh`, `core-site.xml`, `hdfs-site.xml`, `mapred-site.xml`。

#### hadoop-env.sh
在该文件中我们只需要修改 `JAVA_HOME` 这一变量的值为 JDK 安装的真实路径即可，如果按照笔者的教程操作的话，`JAVA_HOME=/opt/jdk1.8.0`，修改内容如下：

Change hadoop-env.sh 

```
# The java implementation to use.  Required.
# export JAVA_HOME=/usr/lib/j2sdk1.5-sun

```

to

```
# The java implementation to use.  Required.
export JAVA_HOME=/opt/jdk1.8.0_131

```

#### conf/*-site.xml

- `conf/core-site.xom` 修改为：

```xml
<configuration>
	<!-- hadoop.tmp.dir 是hadoop文件系统依赖的基础配置，很多路径都依赖它。如果 hdfs-site.xml 中不配置namenode和datanode的存放位置，默认就放在这个路径中-->
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/usr/workspace/hadoop/hdfs/tmp</value>
	</property>
	
	<!--  fs.default.name 这是一个描述集群中NameNode结点的URI(包括协议、主机名称、端口号)，集群里面的每一台机器都需要知道NameNode的地址。DataNode结点会先在NameNode上注册，这样它们的数据才可以被使用。独立的客户端程序通过这个URI跟DataNode交互，以取得文件的块列表。-->
	<property>
		<name>fs.default.name</name>
		<value>hdfs://localhost:9000</value>
	</property>	
</configuration>
```

- `conf/mapred-site.xml` 修改为：

```xml
<configuration>
	<property>
		<name>mapred.job.tracker</name>
		<value>localhost:9001</value>
	</property>
</configuration>
```

- `conf/hdfs-site.xml` 修改为：

```xml
<configuration>
	<!-- dfs.replication 它决定着 系统里面的文件块的数据备份个数。对于一个实际的应用，它应该被设为3（这个数字并没有上限，但更多的备份可能并没有作用，而且会占用更多的空间）。少于三个的备份，可能会影响到数据的可靠性(系统故障时，也许会造成数据丢失)-->
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
	
	<!-- dfs.data.dir 这是DataNode结点被指定要存储数据的本地文件系统路径。DataNode结点上的这个路径没有必要完全相同，因为每台机器的环境很可能是不一样的。但如果每台机器上的这个路径都是统一配置的话，会使工作变得简单一些。默认的情况下，它的值hadoop.tmp.dir, 这个路径只能用于测试的目的，因为，它很可能会丢失掉一些数据。所以，这个值最好还是被覆盖-->
	<property>
		<name>dfs.data.dir</name>
		<value>/usr/workspace/hadoop/hdfs/data</value>
	</property>
	
	<!-- dfs.name.dir 这是NameNode结点存储hadoop文件系统信息的本地系统路径。这个值只对NameNode有效，DataNode并不需要使用到它。上面对于/temp类型的警告，同样也适用于这里。在实际应用中，它最好被覆盖掉。-->
	<property>
		<name>dfs.name.dir</name>
		<value>/usr/workspace/hadoop/hdfs/name</value>
	</property>
</configuration>
```

### step4：修改 Hadoop 工作目录的 owner 和 group
建议创建一个新的群组，并将用户添加到该新群组中，否则，在 `hadoop namenode -format` 可能会报 `Cannot create directory /usr/workspace/hadoop/hdfs/name/current` 的错误，这是因为新创建的 `workspace` 目录其`owner=root, group=root`，所以 Hadoop 在 `-format` 无法自动创建需要的目录，解决办法是修改 `workspace/hadoop/` 及其子目录的 owner 和 group。

-  创建一个新的群组`hadoop`(名字随意)

```shell
# 创建一个新的群组 hadoop
[zodas@localhost ~]$ sudo groupadd hadoop

# 将用户 zodas 添加到新的群组中 
[zodas@localhost ~]$ sudo usermod -G hadoop zodas
```

- 修改工作目录的权限

```shell

[zodas@localhost ~]$ cd /usr
[zodas@localhost usr]$ sudo mkdir -p /usr/workspace/hadoop/hdfs
# 修改 workspace 目录及其子目录的 owner=zodas, group=hadoop 
[zodas@localhost usr]$ sudo chown -R zodas:hadoop workspace/

# 修改 workspace 目录及其子目录的 权限为 rwxrwxr-x(即775)
[zodas@localhost usr]$ sudo chmod -R g+w workspace/

```

- 同上，修改 `/opt/hadoop-1.2.1` 的拥有者及群组为 `zodas:hadoop`

```shell
[zodas@localhost ~]$ sudo chown -R zodas:hadoop /opt/hadoop-1.2.1
[zodas@localhost ~]$ sudo chmod -R g+x /opt/hadoop-1.2.1
```

---

### step5：配置 SSH 免密码登录
不了解 SSH 的，可参考阮一峰的这篇博文"[SSH原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)"

- CentOS 默认是已经安装了 `OpenSSH`的，可在终端中输入`ssh -V`，查看已安装的`OpenSSH`版本
- 使用 `ssh-keygen` 生成公私钥：

```shell
# 生成公私钥。会在 ~/.ssh 目录下生成两个文件：id_rsa.pub 和 id_rsa
[zodas@localhost ~]$ ssh-keygen -t rsa -P ""

```

_tips：`-P` 表示密码，`-P ""` 就表示空密码，也可以不用 `-P` 参数，这样就要三次回车，用 `-P` 就一次回车。_  

- 再输入下面的命令，将公钥传送到远程主机host上面：

```shell

[zodas@localhost ~]$ ssh-copy-id zodas@localhost

```

好了，从此你再登录，就不需要输入密码了。

- 如果还是不行，就打开 `/etc/ssh/sshd_config` 这个文件，检查下面几行前面"#"注释是否取掉

```
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

```

- 重启远程主机的ssh服务。

```shell
# ubuntu系统
service ssh restart
# debian系统
/etc/init.d/ssh restart

```

- 打开终端输入`ssh zodas@localhost`，若能不输入密码即可登录主机 localhost，则表示 SSH 免密码登录配置成功

__另外，__  
Ubuntu 默认是没有安装 `openssh-server`，这也是为什么在`/etc/ssh` 目录下找不到 `sshd_config` 文件。解决方案执行`sudo apt-get install openssh-server` 安装 openssh-server

---

## 启动 Hadoop

### Formatting the HDFS filesystem via the namenode

运行:

```shell
[zodas@localhost ~]hadoop namenode -format

```

输出类似于以下：

```
[zodas@localhost ~]$ hadoop namenode -format
Warning: $HADOOP_HOME is deprecated.

17/07/10 11:13:31 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = ./192.168.12.184
STARTUP_MSG:   args = [-format]
STARTUP_MSG:   version = 1.2.1
STARTUP_MSG:   build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-1.2 -r 1503152; compiled by 'mattf' on Mon Jul 22 15:23:09 PDT 2013
STARTUP_MSG:   java = 1.8.0_131
************************************************************/
17/07/10 11:13:31 INFO util.GSet: Computing capacity for map BlocksMap
17/07/10 11:13:31 INFO util.GSet: VM type       = 64-bit
17/07/10 11:13:31 INFO util.GSet: 2.0% max memory = 1013645312
17/07/10 11:13:31 INFO util.GSet: capacity      = 2^21 = 2097152 entries
17/07/10 11:13:31 INFO util.GSet: recommended=2097152, actual=2097152
17/07/10 11:13:31 INFO namenode.FSNamesystem: fsOwner=zodas
17/07/10 11:13:31 INFO namenode.FSNamesystem: supergroup=supergroup
17/07/10 11:13:31 INFO namenode.FSNamesystem: isPermissionEnabled=true
17/07/10 11:13:31 INFO namenode.FSNamesystem: dfs.block.invalidate.limit=100
17/07/10 11:13:31 INFO namenode.FSNamesystem: isAccessTokenEnabled=false accessKeyUpdateInterval=0 min(s), accessTokenLifetime=0 min(s)
17/07/10 11:13:31 INFO namenode.FSEditLog: dfs.namenode.edits.toleration.length = 0
17/07/10 11:13:31 INFO namenode.NameNode: Caching file names occuring more than 10 times 
17/07/10 11:13:32 ERROR namenode.NameNode: java.io.IOException: Cannot create directory /usr/workspace/hadoop/hdfs/name/current
	at org.apache.hadoop.hdfs.server.common.Storage$StorageDirectory.clearDirectory(Storage.java:294)
	at org.apache.hadoop.hdfs.server.namenode.FSImage.format(FSImage.java:1337)
	at org.apache.hadoop.hdfs.server.namenode.FSImage.format(FSImage.java:1356)
	at org.apache.hadoop.hdfs.server.namenode.NameNode.format(NameNode.java:1261)
	at org.apache.hadoop.hdfs.server.namenode.NameNode.createNameNode(NameNode.java:1467)
	at org.apache.hadoop.hdfs.server.namenode.NameNode.main(NameNode.java:1488)

17/07/10 11:13:32 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at ./192.168.12.184
************************************************************/

```

### Starting single-node cluster
运行：

```shell
[zodas@localhost ~]$ start-all.sh

```

输出：

```
[zodas@ ~]$ start-all.sh 
Warning: $HADOOP_HOME is deprecated.

starting namenode, logging to /opt/hadoop-1.2.1/libexec/../logs/hadoop-zodas-namenode-..out
localhost: Warning: $HADOOP_HOME is deprecated.
localhost: 
localhost: starting datanode, logging to /opt/hadoop-1.2.1/libexec/../logs/hadoop-zodas-datanode-..out
localhost: Warning: $HADOOP_HOME is deprecated.
localhost: 
localhost: starting secondarynamenode, logging to /opt/hadoop-1.2.1/libexec/../logs/hadoop-zodas-secondarynamenode-..out
starting jobtracker, logging to /opt/hadoop-1.2.1/libexec/../logs/hadoop-zodas-jobtracker-..out
localhost: Warning: $HADOOP_HOME is deprecated.
localhost: 
localhost: starting tasktracker, logging to /opt/hadoop-1.2.1/libexec/../logs/hadoop-zodas-tasktracker-..out

```

- 用 `jps` 命令查看 java 进程，验证伪分布式集群是否安装成功

运行：

```shell
[zodas@localhost ~]$ jps

```

输出：

```
[zodas@localhost ~]$ jps
7431 Jps
7192 TaskTracker
6858 DataNode
6989 SecondaryNameNode
7069 JobTracker
6735 NameNode


```

### Stoping single-node cluster

运行：

```shell
[zodas@localhost ~]$ stop-all.sh

```

输出：

```
[zodas@localhost ~]$ stop-all.sh 
Warning: $HADOOP_HOME is deprecated.

stopping jobtracker
localhost: Warning: $HADOOP_HOME is deprecated.
localhost: 
localhost: stopping tasktracker
stopping namenode
localhost: Warning: $HADOOP_HOME is deprecated.
localhost: 
localhost: stopping datanode
localhost: Warning: $HADOOP_HOME is deprecated.
localhost: 
localhost: stopping secondarynamenode

```

---



## 参考
1. [Hadoop DOC](http://hadoop.apache.org/docs/)
2. [SSH 原理与运用(一)--阮一峰](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
3. [Running Hadoop on Ubuntu Linux (Single-Node Cluster)--Michael G. Noll](http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/#configuration)
4. [Hadoop 的安装--brain9999](http://blog.csdn.net/lizhe_dashuju/article/details/13502659)
5. [linux的 CP 命令--董俊杰](http://www.cnblogs.com/xd502djj/archive/2011/11/25/2263562.html)
6. [rpm命令--linux 命令大全](http://man.linuxde.net/rpm)
7. [linux 下的 tar压缩解压缩命令详解](http://www.cnblogs.com/qq78292959/archive/2011/07/06/2099427.html) 
8. [SSH远程登录配置文件sshd_config详解](http://blog.csdn.net/field_yang/article/details/51568861)
9. [Linux添加/删除用户和用户组--董俊杰](http://www.cnblogs.com/xd502djj/archive/2011/11/23/2260094.html)
