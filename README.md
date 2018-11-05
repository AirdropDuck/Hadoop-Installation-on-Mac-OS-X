# Hadoop-Installation-on-Mac-OS-X

### 1.Dowload and Decompress the Hadoop.tar
>Download Link: <http://mirrors.shu.edu.cn/apache/hadoop/common/hadoop-2.7.5/hadoop-2.7.5.tar.gz> 

Put **hadoop-2.7.5** into  **/User/ponta/hadoopworker/** 

### 2 The Configuration Of Hadoop 

#### Hadoop Enviroment
Input command：`vim ~/.bash_profile`

Insert the following statements to the profile:

```
# Hadoop
export JRE_HOME=$JAVA_HOME/jre
export HADOOP_HOME=/Users/ponta/hadoopworker/hadoop-2.7.5
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export HADOOP_HOME_WARN_SUPPRESS=1
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$HADOOP_HOME/bin:$PATH
```
Check if Hadoop has been installed, input command:

`Hadoop version`

Execute result:

```
Hadoop 2.7.5
Subversion https://shv@git-wip-us.apache.org/repos/asf/hadoop.git -r 18065c2b6806ed4aa6a3187d77cbe21bb3dba075
Compiled by kshvachk on 2017-12-16T01:06Z
Compiled with protoc 2.5.0
From source with checksum 9f118f95f47043332d51891e37f736e9
This command was run using /Users/ponta/hadoopworker/hadoop-2.7.5/share/hadoop/common/hadoop-common-2.7.5.jar
```

#### Update Configuration Files
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

Open mapred-site.xml.template(By TextEdit Or...)，create mapred-site.xml:

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

### 3.SSH Login Without Password
#### 1.Open Remote Login
>System Preferences --> Sharing ---> Remote Login

Check if open Remote Login:

`systemsetUp -getremotelogin`

Execute result：

`You need administrator access to run this tool... exiting!`

2.Generate a key pair

Input command：

```
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
```

### 4.Start Hadoop, input command in the directory hadoop-2.7.5 

`sbin/start-dfs.sh  `

Execute result：

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

Update log file：etc/hadoop/log4j.properties to make terminal report error only(Not included warning). 
Insert the following statement to log file：
`
log4j.logger.org.apache.hadoop.util.NativeCodeLoader=ERROR
`
Restart Hadoop:
`sbin/start-dfs.sh`

Execute result：

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
  
Input command in the directory hadoop-2.7.5:
  
`sbin/start-yarn.sh`

Execute result：

```
starting yarn daemons
resourcemanager running as process 46489. Stop it first.
Password:
localhost: nodemanager running as process 46652. Stop it first.
```

Input command：`jps`
Execute result：

```
52530 SecondaryNameNode
52723 ResourceManager
52420 DataNode
52815 NodeManager
52927 Jps
```
It means execute successfully.

Try to open localhost:50070 and localhost:8088 in browser. localhost:50070 not found ，but localhost:8088 runs normally.
 
Stop Hadoop, input command：`sbin/stop-all.sh` 

Execute result：

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
Hadoop can't not be stopped. There are some issues.

The solution：Delete all content from /tmp:

`rm -Rf /app/hadoop/tmp `

 Format the namenode server namenode server：

`bin/hadoop namenode -format`

Restart Hadoop, localhost:50070 runs normally.。

Reference Link：<https://stackoverflow.com/questions/33772495/no-namenode-or-datanode-or-secondary-namenode-to-stop>

