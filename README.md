# Hadoop-Installation-on-Mac-OS-X


# macOS 下 Hadoop 的安装和配置

### 1.下载解压 Hadoop tar 包
>下载链接: <http://mirrors.shu.edu.cn/apache/hadoop/common/hadoop-2.7.5/hadoop-2.7.5.tar.gz> 

将解压后的文件 **hadoop-2.7.5** 放在 **/User/ponta/hadoopworker/** 目录下

### 2.配置 Hadoop

#### Hadoop 环境配置
输入命令：`vim ~/.bash_profile`

在profile中添加：

```
# Hadoop
export JRE_HOME=$JAVA_HOME/jre
export HADOOP_HOME=/Users/ponta/hadoopworker/hadoop-2.7.5
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export HADOOP_HOME_WARN_SUPPRESS=1
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$HADOOP_HOME/bin:$PATH
```
检验Hadoop是否配置成功：

`Hadoop version`

运行结果：

```
Hadoop 2.7.5
Subversion https://shv@git-wip-us.apache.org/repos/asf/hadoop.git -r 18065c2b6806ed4aa6a3187d77cbe21bb3dba075
Compiled by kshvachk on 2017-12-16T01:06Z
Compiled with protoc 2.5.0
From source with checksum 9f118f95f47043332d51891e37f736e9
This command was run using /Users/ponta/hadoopworker/hadoop-2.7.5/share/hadoop/common/hadoop-common-2.7.5.jar
```

#### 修改配置文件
1.core-site.xml

```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/Users/ponta/hadoopwork/hadoop-2.7.5/data</value>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:8000</value>
    </property>
</configuration>
```

2.hdfs-site.xml
```
<configuration>
   <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```
3.mapred-site.xml

打开 mapred-site.xml.template(可以使用 文本编辑 工具)，新建 mapred-site.xml。

```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

4.yarn-site.xml

```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

### 3.SSH免密登陆
#### 1.打开远程登陆
>系统偏好设置 --> 共享 ---> 远程登陆 

输入命令判断是否开启远程登陆：

`systemsetUp -getremotelogin`

运行结果：

`You need administrator access to run this tool... exiting!`

2.生成密钥对

输入命令：

```
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
```

### 4.启动Hadoop，在hadoop-2.7.5目录下输入命令：

`sbin/start-dfs.sh  `

运行结果：

```
18/11/01 17:14:22 DEBUG util.Shell: setsid is not available on this machine. So not using it.
18/11/01 17:14:22 DEBUG util.Shell: setsid exited with exit code 0
18/11/01 17:14:22 DEBUG lib.MutableMetricsFactory: field org.apache.hadoop.metrics2.lib.MutableRate org.apache.hadoop.security.UserGroupInformation$UgiMetrics.loginSuccess with annotation @org.apache.hadoop.metrics2.annotation.Metric(always=false, about=, sampleName=Ops, type=DEFAULT, value=[Rate of successful kerberos logins and latency (milliseconds)], valueName=Time)
18/11/01 17:14:22 DEBUG lib.MutableMetricsFactory: field org.apache.hadoop.metrics2.lib.MutableRate org.apache.hadoop.security.UserGroupInformation$UgiMetrics.loginFailure with annotation @org.apache.hadoop.metrics2.annotation.Metric(always=false, about=, sampleName=Ops, type=DEFAULT, value=[Rate of failed kerberos logins and latency (milliseconds)], valueName=Time)
18/11/01 17:14:22 DEBUG lib.MutableMetricsFactory: field org.apache.hadoop.metrics2.lib.MutableRate org.apache.hadoop.security.UserGroupInformation$UgiMetrics.getGroups with annotation @org.apache.hadoop.metrics2.annotation.Metric(always=false, about=, sampleName=Ops, type=DEFAULT, value=[GetGroups], valueName=Time)
18/11/01 17:14:22 DEBUG impl.MetricsSystemImpl: UgiMetrics, User and group related metrics
18/11/01 17:14:22 DEBUG util.KerberosName: Kerberos krb5 configuration not found, setting default realm to empty
18/11/01 17:14:22 DEBUG security.Groups:  Creating new Groups object
18/11/01 17:14:22 DEBUG util.NativeCodeLoader: Trying to load the custom-built native-hadoop library...
18/11/01 17:14:22 DEBUG util.NativeCodeLoader: Failed to load native-hadoop with error: java.lang.UnsatisfiedLinkError: no hadoop in java.library.path
18/11/01 17:14:22 DEBUG util.NativeCodeLoader: java.library.path=/Users/ponta/hadoopworker/hadoop-2.7.5/lib/native
18/11/01 17:14:22 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
18/11/01 17:14:22 DEBUG util.PerformanceAdvisory: Falling back to shell based
18/11/01 17:14:22 DEBUG security.JniBasedUnixGroupsMappingWithFallback: Group mapping impl=org.apache.hadoop.security.ShellBasedUnixGroupsMapping
18/11/01 17:14:22 DEBUG security.Groups: Group mapping impl=org.apache.hadoop.security.JniBasedUnixGroupsMappingWithFallback; cacheTimeout=300000; warningDeltaMs=5000
18/11/01 17:14:22 DEBUG security.UserGroupInformation: hadoop login
18/11/01 17:14:22 DEBUG security.UserGroupInformation: hadoop login commit
18/11/01 17:14:22 DEBUG security.UserGroupInformation: using local user:UnixPrincipal: ponta
18/11/01 17:14:22 DEBUG security.UserGroupInformation: Using user: "UnixPrincipal: ponta" with name ponta
18/11/01 17:14:22 DEBUG security.UserGroupInformation: User entry: "ponta"
18/11/01 17:14:22 DEBUG security.UserGroupInformation: Assuming keytab is managed externally since logged in from subject.
18/11/01 17:14:22 DEBUG security.UserGroupInformation: UGI loginUser:ponta (auth:SIMPLE)
18/11/01 17:14:22 DEBUG security.UserGroupInformation: PrivilegedAction as:ponta (auth:SIMPLE) from:org.apache.hadoop.hdfs.tools.GetConf.run(GetConf.java:315)
Starting namenodes on [localhost]
```

修改日志文件：etc/hadoop/log4j.properties, 使直接显示错误，不显示警告。添加：
`
log4j.logger.org.apache.hadoop.util.NativeCodeLoader=ERROR
`

sbin/start-dfs.sh 运行结果：

```
PontadeMacBook-Pro:hadoop-2.7.5 ponta$ sbin/start-dfs.sh
Starting namenodes on [localhost]
Password:
localhost: starting namenode, logging to /Users/ponta/hadoopworker/hadoop-2.7.5/logs/hadoop-ponta-namenode-PontadeMacBook-Pro.local.out
Password:
localhost: starting datanode, logging to /Users/ponta/hadoopworker/hadoop-2.7.5/logs/hadoop-ponta-datanode-PontadeMacBook-Pro.local.out
Starting secondary namenodes [0.0.0.0]
Password:
0.0.0.0: starting secondarynamenode, logging to /Users/ponta/hadoopworker/hadoop-2.7.5/logs/hadoop-ponta-secondarynamenode-PontadeMacBook-Pro.local.out
```
  
在hadoop-2.7.5目录下输入命令：
  
`sbin/start-yarn.sh`

运行结果：

```
starting yarn daemons
resourcemanager running as process 46489. Stop it first.
Password:
localhost: nodemanager running as process 46652. Stop it first.
```

输入命令：`jps`
输出结果：

```
52530 SecondaryNameNode
52723 ResourceManager
52420 DataNode
52815 NodeManager
52927 Jps
```
运行成功。

尝试打开 localhost:50070 和 localhost:8088，找不到 localhost:50070 网页，localhost:8088正常。
 
关闭Hadoop, 输入命令：`sbin/stop-all.sh` 输出结果：

```
This script is Deprecated. Instead use stop-dfs.sh and stop-yarn.sh
Stopping namenodes on [localhost]
Password:
localhost: no namenode to stop
Password:
localhost: stopping datanode
Stopping secondary namenodes [0.0.0.0]
Password:
0.0.0.0: stopping secondarynamenode
stopping yarn daemons
stopping resourcemanager
Password:
localhost: stopping nodemanager
no proxyserver to stop
```
Hadoop 没有正常关闭。

解决方法：
删除所有/tmp中的内容：

`rm -Rf /app/hadoop/tmp `

格式化 namenode server：

`bin/hadoop namenode -format`

重新启动Hadoop，尝试打开 localhost:50070 正常。

参考链接：<https://stackoverflow.com/questions/33772495/no-namenode-or-datanode-or-secondary-namenode-to-stop>

