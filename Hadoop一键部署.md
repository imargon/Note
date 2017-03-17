hadoop+spark+hive 安装教程

 1.将jdk-8u111-linux-x64.tar.gz,
     scala-2.12.0.tgz,
     spark-2.0.1-bin-hadoop2.7.tgz,
     hadoop-2.7.3.tar.gz 解压到/usr/local/下，
   分别重命名为/usr/local/jvm/jdk,/usr/local/scala,/usr/local/spark,/usr/local/hadoop
2.在etc/profile,中分别添加路径   


1.hadoop+spark参考教程：http://www.cnblogs.com/bovenson/p/5760856.html
 重新设置SSH 密码，然后赋权限。1.查看ssh 服务是否启动，service sshd start 。2.设置ssh密码，赋权限，以root用户启动Hadoop
 
2.hive安装教程
2.1 安装mysql
	sudo apt-get -y install mysql*，并设置密码
2.2 登录MySQL并创建数据库 hive用户名密码
 	mysql -u root -p
 	create database hive;
 	use hive;
 	create user 'hive' identified by 'hive';  
 	grant all privileges on *.* to 'hive' with grant option;  
 	flush privileges;  
 	create table user(Host char(80),User char(60),Password char(80));
 	insert into user(Host,User,Password) values("localhost","hive",password("123"));
 	FLUSH PRIVILEGES;
 	GRANT ALL PRIVILEGES ON *.*  TO 'hive'@'localhost' IDENTIFIED BY 'hive';
 	FLUSH PRIVILEGES;

3.解压hive安装包之后，配置环境

 	sudo vim /etc/profile
 	export HIVE_HOME=/usr/local/hive
 	export PATH=$PATH:$HIVE_HOME/bin

4.配置hive/conf模板文件
 
 	cp hive-env.sh.template hive-env.sh
 	cp hive-default.xml.template hive-site.xml
 
5.配置hive-env.sh文件，指定HADOOP_HOME
    HADOOP_HOME=/usr/local/hadoop

6.修改hive-site.xml文件，指定MySQL数据库驱动、数据库名、用户名及密码
	<property>
	  <name>javax.jdo.option.ConnectionURL</name>
	  <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
	  <description>JDBC connect string for a JDBC metastore</description>
	</property>

	<property>
	  <name>javax.jdo.option.ConnectionDriverName</name>
	  <value>com.mysql.jdbc.Driver</value>
	  <description>Driver class name for a JDBC metastore</description>
	</property>

	<property>
	  <name>javax.jdo.option.ConnectionUserName</name>
	  <value>hive</value>
	  <description>username to use against metastore database</description>
	</property>

	<property>
	  <name>javax.jdo.option.ConnectionPassword</name>
	  <value>hive</value>
	  <description>password to use against metastore database</description>
	</property>

	<property>
	  <name>hive.metastore.local</name>
	  <value>true</value>
	  <description></description>
	</property>

7.在hive下创建临时IO的tmp文件夹。然后将路径配置到hive-site.xml的下列参数中
     
    mkdir /usr/local/hive/iotmp
	<property>
	    <name>hive.querylog.location</name>
	    <value>/usr/local/hive/iotmp</value>
	    <description>Location of Hive run time structured log file</description>
	</property>
	  
	<property>
	    <name>hive.exec.local.scratchdir</name>
	    <value>/usr/local/hive/iotmp</value>
	    <description>Local scratch space for Hive jobs</description>
	</property>
	  
	<property>
	    <name>hive.downloaded.resources.dir</name>
	    <value>/usr/local/hive/iotmp</value>
	    <description>Temporary local directory for added resources in the remote file system.</description>
	</property>

8.修改hive/bin下的hive-config.sh文件，设置JAVA_HOME,HADOOP_HOME

	export JAVA_HOME=/usr/local/jvm/jdk
	export HADOOP_HOME=/usr/local/hadoop
	export HIVE_HOME=/usr/local/hive
	
9.下载mysql-connector-java-5.1.27-bin.jar文件，并放到$HIVE_HOME/lib目录下
10.初始化Hive 数据库脚本
11.以root 用户启动Hive
