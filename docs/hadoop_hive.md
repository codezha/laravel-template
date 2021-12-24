### 超时自动断开
```
vim /etc/ssh/sshd_config
```
找到
```
#ClientAliveInterval 0
#ClientAliveCountMax 3
```
修改为
```
ClientAliveInterval 60
ClientAliveCountMax 3
```
重启sshd
```
systemctl restart sshd
```
### 创建用户
root用户执行以下
添加用户
```
sudo adduser -g root -m hadoop
```
设置密码=hadoop
```
echo "hadoop"| passwd hadoop --stdin
```
设置权限
```
vim /etc/sudoers
```
在root下添加一行
```
root ALL = (ALL)ALL
hadoop ALL = (ALL)ALL
```

免密登陆设置
自己电脑查看公钥
```
cat ~/.ssh/id_rsa.pub
```
连上服务器
```
ssh root@xxx.xxx.xxx.xxx
ssh localhost             # 创建.ssh文件夹 会让输入yes 密码是hadoop
ssh-keygen -t rsa # 一直按回车
ssh-copy-id localhost 添加授权
cat ./id_rsa.pub >> ./authorized_keys 添加授权
cat /home/hadoop/.ssh/id_rsa.pub >> /home/hadoop/.ssh/authorized_keys
vim .ssh/authorized_keys  # 编辑文件 粘贴刚刚自己电脑的公钥进去
```

以后就可以 ssh hadoop@49.235.35.222 连接服务器了


### 环境变量添加
编辑
```
vim /etc/profile
```
添加如下
```
export  JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64
export  HADOOP_HOME=/opt/hadoop/hadoop-3.1.3
export  HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
export  HADOOP_COMMON_LIB_NATIVE_DIR=${HADOOP_HOME}/lib/native
export  HADOOP_OPTS="-Djava.library.path=${HADOOP_HOME}/lib"
export  HIVE_HOME=/opt/hive/apache-hive-3.1.2-bin
export  HIVE_CONF_DIR=${HIVE_HOME}/conf
export  CLASS_PATH=.:${JAVA_HOME}/lib:${HIVE_HOME}/lib:$CLASS_PATH
export  PATH=.:${JAVA_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:${HIVE_HOME}/bin:$PATH
```
让配置生效
```
source /etc/profile
```

### 安装 jdk1.8
```
yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel
```
或者如下命令，安装**jdk1.8.0**的所有文件
```
yum install -y java-1.8.0-openjdk*
```
安装完查看版本
```
java -version
```

### 安装 MySql 8.0

安装 mysql 官方 yum 源
```
rpm -ivh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
```
查看可用的**mysql**版本以及禁用/启用情况
```
yum repolist all | grep mysql
```
或
```
yum list |  grep mysql
```
开启 80 包
```
yum-config-manager --enable mysql80-community
```
安装 mysql
```
yum install -y mysql-community-server
yum install -y mysql-community-client # 根据需要安装（可不装）
```
管理 mysql
```
systemctl start mysqld # 启动 mysql
systemctl stop mysqld # 停止 mysql
```
查看超级账户 root 临时密码
```
grep 'temporary password' /var/log/mysqld.log
```
jglfsylIb3_%
修改超级账户 root 密码创建新用户
```
mysql  -uroot  -p
mysql> ALTER USER "root"@"localhost" IDENTIFIED BY 'iR3+dI0{jK';
mysql> create database hiveDB;
mysql> create user codezha_hive IDENTIFIED by 'cA4+rA0@gJ';
mysql> grant all privileges on *.* to 'codezha_hive'@'%' with grant option;
mysql> use mysql;
mysql> delete from user where user='root' and host!='localhost';//禁止root远程登录
mysql> CREATE DATABASE lsoft-chat DEFAULT CHARACTER SET utf8mb4COLLATE utf8mb4_general_ci;
mysql> flush privileges;
mysql> exit;
systemctl restart mysqld.service
```

### 安装 Hadoop 3.1.3


下载
```
wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.1.3/hadoop-3.1.3.tar.gz
```
新建文件夹
```
sudo mkdir -p /opt/hadoop
```
解压
```
sudo tar -zxvf hadoop-3.1.3.tar.gz -C /opt/hadoop/
```
进入目录
```
cd /opt/hadoop/hadoop-3.1.3/
```
查看版本
```
./bin/hadoop version
```
进入配置目录
```
cd /opt/hadoop/hadoop-3.1.3/etc/hadoop/
```
伪分布式需要修改2个配置文件 core-site.xml 和 hdfs-site.xml
**core-site.xml**修改如下：
```
vim /opt/hadoop/hadoop-3.1.3/etc/hadoop/core-site.xml
```
```
<configuration>
<property>
       <name>hadoop.tmp.dir</name>
       <value>file:/opt/hadoop/hadoop-3.1.3/tmp</value>
</property>
<property>
       <name>fs.defaultFS</name>
      <value>hdfs://localhost:9000</value>
</property>
<property>
        <name>hadoop.proxyuser.hadoop.hosts</name>
        <value>*</value>
</property>
<property>
        <name>hadoop.proxyuser.hadoop.groups</name>
        <value>*</value>
</property>
</configuration>
```
**hdfs-site.xml**修改如下：
```
vim /opt/hadoop/hadoop-3.1.3/etc/hadoop/hdfs-site.xml
```
```
<configuration>
<property>
       <name>dfs.replication</name>
       <value>1</value>
</property>
<property>
       <name>dfs.namenode.name.dir</name>
       <value>file:/opt/hadoop/hadoop-3.1.3/tmp/dfs/name</value>
       <final>true</final>
</property>
<property>
       <name>dfs.datanode.data.dir</name>
       <value>file:/opt/hadoop/hadoop-3.1.3/tmp/dfs/data</value>
       <final>true</final>
</property>
<property>
        <name>dfs.permissions</name>
        <value>false</value>
</property>
</configuration>
```

```
vim /opt/hadoop/hadoop-3.1.3/etc/hadoop/hadoop-env.sh
export  JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64
```
```
vim /opt/hadoop/hadoop-3.1.3/libexec/hadoop-config.sh
export  JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64
```
```
vim /opt/hadoop/hadoop-3.1.3/etc/hadoop/yarn-site.xml
<configuration>
<property>
        <name>yarn.nodemanager.aux-servies</name>
        <value>mapreduce_shuffle</value>
</property>
</configuration>
```


新建文件夹
```
mkdir -p /opt/hadoop/hadoop-3.1.3/tmp/dfs/name
mkdir -p /opt/hadoop/hadoop-3.1.3/tmp/dfs/data
```
配置完后在hadoop目录下执行
```
./bin/hdfs namenode -format
```

启动hadoop伪分布式集群
```
sbin/start-all.sh
sbin/stop-all.sh
```

hadoop启动和关闭：
```
./sbin/start-dfs.sh      #启动
jps                      #查看进程
./sbin/stop-dfs.sh       #关闭
```
启动报错JAVAHOME什么的

chmod: changing permissions of ‘logs/’
```
chmod -R  777  logs/
```

目录
（1）bin目录：存放对Hadoop相关服务（HDFS,YARN）进行操作的脚本

（2）etc目录：Hadoop的配置文件目录，存放Hadoop的配置文件

（3）lib目录：存放Hadoop的本地库（对数据进行压缩解压缩功能）

（4）sbin目录：存放启动或停止Hadoop相关服务的脚本

（5）share目录：存放Hadoop的依赖jar包、文档、和官方案例


### 安装 hive 3.1.2
```
wget http://mirror.bit.edu.cn/apache/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
```
新建文件夹
```
sudo mkdir -p /opt/hive
sudo mkdir -p /opt/hive/tmp
sudo chmod -R 777 /opt/hive/tmp/
```
解压
```
sudo tar -zxvf apache-hive-3.1.2-bin.tar.gz -C /opt/hive/
```
新建hdfs目录并赋予读写权限
```
$HADOOP_HOME/bin/hadoop   fs   -mkdir   -p   /user/hive/warehouse
$HADOOP_HOME/bin/hadoop   fs   -chmod   777   /user/hive/warehouse
$HADOOP_HOME/bin/hadoop   fs   -mkdir  -p   /tmp/hive/
$HADOOP_HOME/bin/hadoop   fs   -chmod  777   /tmp/hive

$HADOOP_HOME/bin/hadoop   fs   -ls   /user/hive/   # 检查是否创建成功
$HADOOP_HOME/bin/hadoop   fs   -ls   /tmp/
```
cd到配置文件目录
```
cd /opt/hive/apache-hive-3.1.2-bin/conf
```
修改hive-site.xml
```
sudo cp hive-default.xml.template hive-site.xml
sudo vim hive-site.xml

${system:java.io.tmpdir} 都替换成 /opt/hive/tmp
${system:user.name} 都替换成 root
javax.jdo.option.ConnectionURL 的value值替换成    jdbc:mysql://49.235.35.222:3306/hive?createDatabaseIfNotExist=true

javax.jdo.option.ConnectionDriverName  的value值替换成 com.mysql.jdbc.Driver

javax.jdo.option.ConnectionUserName  的value值替换成 codezha_hive

javax.jdo.option.ConnectionPassword  的value值替换成 cA4+rA0@gJ

hive.metastore.schema.verification  的value值替换成 false
```
修改hive-env.sh
```
sudo cp hive-env.sh.template hive-env.sh
sudo vim hive-env.sh
export  HADOOP_HOME=/opt/hadoop/hadoop-3.1.3
export  HIVE_CONF_DIR=/opt/hive/apache-hive-3.1.2-bin/conf
export  HIVE_AUX_JARS_PATH=/opt/hive/apache-hive-3.1.2-bin/lib
```
下载驱动包
```
sudo wget -P $HIVE_HOME/lib https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.21/mysql-connector-java-8.0.21.jar
```
对数据库进行初始化
```
cd  /opt/hive/apache-hive-3.1.2-bin/bin
schematool   -initSchema  -dbType  mysql
```
报错SLF4J冲突删掉一个  guava删掉版本低的拷贝版本高的
```
cd /opt/hadoop/hadoop-3.1.3/share/hadoop/common/lib/
sudo mv slf4j-log4j12-1.7.25.jar slf4j-log4j12-1.7.25.jar.bak

cd /opt/hive/apache-hive-3.1.2-bin/lib
sudo mv guava-19.0.jar guava-19.0.jar.bak

sudo cp /opt/hadoop/hadoop-3.1.3/share/hadoop/common/lib/guava-27.0-jre.jar /opt/hive/apache-hive-3.1.2-bin/lib
```

启动hive
```
cd /opt/hive/apache-hive-3.1.2-bin/bin
./hive
```
