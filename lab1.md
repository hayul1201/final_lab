#  change passwd
```
centos@ip-172-31-4-181 etc]$ sudo passwd centos
Changing password for user centos.
New password:
BAD PASSWORD: The password is shorter than 8 characters
Retype new password:
passwd: all authentication tokens updated successfully.
[centos@ip-172-31-4-181 etc]$
```
# sshd_config PasswordAuthentication 변경
```
[centos@ip-172-31-4-181 etc]$ sudo vi /etc/ssh/sshd_config

# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes
#PermitEmptyPasswords no
PasswordAuthentication yes

[centos@ip-172-31-4-181 etc]$ sudo systemctl restart sshd.service
```

# host 설정
```
sudo vi /etc/hosts

172.31.13.209    util01.cdhcluster.com  util01
172.31.7.120    master01.cdhcluster.com mn1
172.31.5.37   data01.cdhcluster.com   dn1
172.31.4.18   data02.cdhcluster.com   dn2
172.31.2.250     data03.cdhcluster.com   dn3

[centos@ip-172-31-4-181 etc]$ sudo hostnamectl set-hostname data02.cdhcluster.com
[centos@ip-172-31-4-181 etc]$ hostname -f
sudo shutdown -r now
```
# jdk 설치
```
[centos@util01 ~]$ yum list java*jdk-devel
[centos@util01 ~]$ sudo yum install java-1.8.0-openjdk-devel.x86_64

[centos@util01 ~]$ rpm -qa java*jdk-devel
java-1.8.0-openjdk-devel-1.8.0.212.b04-0.el7_6.x86_64

[centos@util01 ~]$ javac -version
javac 1.8.0_212
```
#  CM 설치
```
[centos@util01 ~]$ sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/
--2019-05-22 13:50:33--  https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
Resolving archive.cloudera.com (archive.cloudera.com)... 151.101.108.167
Connecting to archive.cloudera.com (archive.cloudera.com)|151.101.108.167|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 290 [binary/octet-stream]
Saving to: ‘/etc/yum.repos.d/cloudera-manager.repo’

100%[======================================>] 290         --.-K/s   in 0s

2019-05-22 13:50:34 (86.4 MB/s) - ‘/etc/yum.repos.d/cloudera-manager.repo’ saved [290/290]

[centos@util01 ~]$ sudo vi /etc/yum.repos.d/cloudera-manager.repo
baseurl=https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.15.2/

[centos@util01 ~]$ sudo rpm --import https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera


[centos@util01 ~]$ sudo yum install -y cloudera-manager-daemons cloudera-manager                               -server
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: ftp.neowiz.com
 * epel: d2lzkl7pfhq30w.cloudfront.net
 * extras: mirror.kakao.com
 * updates: mirror.kakao.com
cloudera-manager                                         |  951 B     00:00
cloudera-manager/primary                                   | 4.3 kB   00:00
cloudera-manager                                                            7/7
Resolving Dependencies
--> Running transaction check
---> Package cloudera-manager-daemons.x86_64 0:5.15.2-1.cm5152.p0.2.el7 will be                                installed
---> Package cloudera-manager-server.x86_64 0:5.15.2-1.cm5152.p0.2.el7 will be i                               nstalled
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package                 Arch   Version                  Repository        Size
================================================================================
Installing:
 cloudera-manager-daemons
                         x86_64 5.15.2-1.cm5152.p0.2.el7 cloudera-manager 752 M
 cloudera-manager-server x86_64 5.15.2-1.cm5152.p0.2.el7 cloudera-manager 8.5 k

Transaction Summary
================================================================================
Install  2 Packages

Total download size: 752 M
Installed size: 933 M
Downloading packages:
(1/2): cloudera-manager-server-5.15.2-1.cm5152.p0.2.el7.x8 | 8.5 kB   00:00
(2/2): cloudera-manager-daemons-5.15.2-1.cm5152.p0.2.el7.x86_64.rpm           | 752 MB  00:00:16
-----------------------------------------------------------------------------------------------------
Total                                                                 44 MB/s | 752 MB  00:00:16
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : cloudera-manager-daemons-5.15.2-1.cm5152.p0.2.el7.x86_64                          1/2
  Installing : cloudera-manager-server-5.15.2-1.cm5152.p0.2.el7.x86_64                           2/2
  Verifying  : cloudera-manager-daemons-5.15.2-1.cm5152.p0.2.el7.x86_64                          1/2
  Verifying  : cloudera-manager-server-5.15.2-1.cm5152.p0.2.el7.x86_64                           2/2

Installed:
  cloudera-manager-daemons.x86_64 0:5.15.2-1.cm5152.p0.2.el7
  cloudera-manager-server.x86_64 0:5.15.2-1.cm5152.p0.2.el7

Complete!
```

# DB Server 설치
```
[centos@util01 ~]$ sudo yum install -y mariadb-server
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: ftp.neowiz.com
 * epel: ftp.riken.jp
 * extras: mirror.kakao.com
 * updates: mirror.kakao.com
Resolving Dependencies
--> Running transaction check
---> Package mariadb-server.x86_64 1:5.5.60-1.el7_5 will be installed
--> Processing Dependency: mariadb(x86-64) = 1:5.5.60-1.el7_5 for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: perl-DBI for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: perl-DBD-MySQL for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: perl(Data::Dumper) for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: perl(DBI) for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.4)(64bit) for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.1)(64bit) for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Processing Dependency: libaio.so.1()(64bit) for package: 1:mariadb-server-5.5.60-1.el7_5.x86_64
--> Running transaction check
---> Package libaio.x86_64 0:0.3.109-13.el7 will be installed
---> Package mariadb.x86_64 1:5.5.60-1.el7_5 will be installed
---> Package perl-DBD-MySQL.x86_64 0:4.023-6.el7 will be installed
---> Package perl-DBI.x86_64 0:1.627-4.el7 will be installed
--> Processing Dependency: perl(RPC::PlServer) >= 0.2001 for package: perl-DBI-1.627-4.el7.x86_64
--> Processing Dependency: perl(RPC::PlClient) >= 0.2000 for package: perl-DBI-1.627-4.el7.x86_64
---> Package perl-Data-Dumper.x86_64 0:2.145-3.el7 will be installed
--> Running transaction check
---> Package perl-PlRPC.noarch 0:0.2020-14.el7 will be installed
--> Processing Dependency: perl(Net::Daemon) >= 0.13 for package: perl-PlRPC-0.2020-14.el7.noarch
--> Processing Dependency: perl(Net::Daemon::Test) for package: perl-PlRPC-0.2020-14.el7.noarch
--> Processing Dependency: perl(Net::Daemon::Log) for package: perl-PlRPC-0.2020-14.el7.noarch
--> Processing Dependency: perl(Compress::Zlib) for package: perl-PlRPC-0.2020-14.el7.noarch
--> Running transaction check
---> Package perl-IO-Compress.noarch 0:2.061-2.el7 will be installed
--> Processing Dependency: perl(Compress::Raw::Zlib) >= 2.061 for package: perl-IO-Compress-2.061-2.el7.noarch
--> Processing Dependency: perl(Compress::Raw::Bzip2) >= 2.061 for package: perl-IO-Compress-2.061-2.el7.noarch
---> Package perl-Net-Daemon.noarch 0:0.48-5.el7 will be installed
--> Running transaction check
---> Package perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7 will be installed
---> Package perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================================
 Package                             Arch               Version                         Repository        Size
===============================================================================================================
Installing:
 mariadb-server                      x86_64             1:5.5.60-1.el7_5                base              11 M
Installing for dependencies:
 libaio                              x86_64             0.3.109-13.el7                  base              24 k
 mariadb                             x86_64             1:5.5.60-1.el7_5                base             8.9 M
 perl-Compress-Raw-Bzip2             x86_64             2.061-3.el7                     base              32 k
 perl-Compress-Raw-Zlib              x86_64             1:2.061-4.el7                   base              57 k
 perl-DBD-MySQL                      x86_64             4.023-6.el7                     base             140 k
 perl-DBI                            x86_64             1.627-4.el7                     base             802 k
 perl-Data-Dumper                    x86_64             2.145-3.el7                     base              47 k
 perl-IO-Compress                    noarch             2.061-2.el7                     base             260 k
 perl-Net-Daemon                     noarch             0.48-5.el7                      base              51 k
 perl-PlRPC                          noarch             0.2020-14.el7                   base              36 k

Transaction Summary
===============================================================================================================
Install  1 Package (+10 Dependent packages)

Total download size: 21 M
Installed size: 110 M
Downloading packages:
(1/11): libaio-0.3.109-13.el7.x86_64.rpm                                                |  24 kB  00:00:00
(2/11): mariadb-5.5.60-1.el7_5.x86_64.rpm                                               | 8.9 MB  00:00:00
(3/11): perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64.rpm                                  |  32 kB  00:00:00
(4/11): perl-Compress-Raw-Zlib-2.061-4.el7.x86_64.rpm                                   |  57 kB  00:00:00
(5/11): perl-DBD-MySQL-4.023-6.el7.x86_64.rpm                                           | 140 kB  00:00:00
(6/11): mariadb-server-5.5.60-1.el7_5.x86_64.rpm                                        |  11 MB  00:00:00
(7/11): perl-DBI-1.627-4.el7.x86_64.rpm                                                 | 802 kB  00:00:00
(8/11): perl-Data-Dumper-2.145-3.el7.x86_64.rpm                                         |  47 kB  00:00:00
(9/11): perl-IO-Compress-2.061-2.el7.noarch.rpm                                         | 260 kB  00:00:00
(10/11): perl-Net-Daemon-0.48-5.el7.noarch.rpm                                          |  51 kB  00:00:00
(11/11): perl-PlRPC-0.2020-14.el7.noarch.rpm                                            |  36 kB  00:00:00
---------------------------------------------------------------------------------------------------------------
Total                                                                           91 MB/s |  21 MB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : perl-Data-Dumper-2.145-3.el7.x86_64                                                        1/11
  Installing : libaio-0.3.109-13.el7.x86_64                                                               2/11
  Installing : 1:mariadb-5.5.60-1.el7_5.x86_64                                                            3/11
  Installing : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                4/11
  Installing : perl-Net-Daemon-0.48-5.el7.noarch                                                          5/11
  Installing : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                 6/11
  Installing : perl-IO-Compress-2.061-2.el7.noarch                                                        7/11
  Installing : perl-PlRPC-0.2020-14.el7.noarch                                                            8/11
  Installing : perl-DBI-1.627-4.el7.x86_64                                                                9/11
  Installing : perl-DBD-MySQL-4.023-6.el7.x86_64                                                         10/11
  Installing : 1:mariadb-server-5.5.60-1.el7_5.x86_64                                                    11/11
  Verifying  : 1:mariadb-server-5.5.60-1.el7_5.x86_64                                                     1/11
  Verifying  : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                 2/11
  Verifying  : perl-Net-Daemon-0.48-5.el7.noarch                                                          3/11
  Verifying  : perl-Data-Dumper-2.145-3.el7.x86_64                                                        4/11
  Verifying  : perl-DBD-MySQL-4.023-6.el7.x86_64                                                          5/11
  Verifying  : perl-IO-Compress-2.061-2.el7.noarch                                                        6/11
  Verifying  : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                7/11
  Verifying  : 1:mariadb-5.5.60-1.el7_5.x86_64                                                            8/11
  Verifying  : perl-DBI-1.627-4.el7.x86_64                                                                9/11
  Verifying  : libaio-0.3.109-13.el7.x86_64                                                              10/11
  Verifying  : perl-PlRPC-0.2020-14.el7.noarch                                                           11/11

Installed:
  mariadb-server.x86_64 1:5.5.60-1.el7_5

Dependency Installed:
  libaio.x86_64 0:0.3.109-13.el7                         mariadb.x86_64 1:5.5.60-1.el7_5
  perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7           perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7
  perl-DBD-MySQL.x86_64 0:4.023-6.el7                    perl-DBI.x86_64 0:1.627-4.el7
  perl-Data-Dumper.x86_64 0:2.145-3.el7                  perl-IO-Compress.noarch 0:2.061-2.el7
  perl-Net-Daemon.noarch 0:0.48-5.el7                    perl-PlRPC.noarch 0:0.2020-14.el7

Complete!
```
# my.cnf수정
```
[centos@util01 ~]$ sudo systemctl stop mariadb
[centos@util01 ~]$ sudo vi /etc/my.cnf
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
symbolic-links = 0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

key_buffer = 16M
key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space.
#Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your
#system and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

#In later versions of MariaDB, if you enable the binary log and do not set
#a server_id, MariaDB will not start. The server_id must be unique within
#the replicating group.
server_id=1

binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

```
```
[centos@util01 ~]$ sudo systemctl start mariadb

[centos@util01 ~]$ sudo /usr/bin/mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

```
[centos@util01 ~]$ sudo rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
[centos@util01 ~]$ sudo wget "https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz"
--2019-05-22 14:03:35--  https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz
Resolving dev.mysql.com (dev.mysql.com)... 137.254.60.11
Connecting to dev.mysql.com (dev.mysql.com)|137.254.60.11|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz [following]
--2019-05-22 14:03:36--  https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz
Resolving cdn.mysql.com (cdn.mysql.com)... 23.35.197.183
Connecting to cdn.mysql.com (cdn.mysql.com)|23.35.197.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4434926 (4.2M) [application/x-tar-gz]
Saving to: ‘mysql-connector-java-5.1.46.tar.gz’

100%[=====================================================================>] 4,434,926   17.1MB/s   in 0.2s

2019-05-22 14:03:36 (17.1 MB/s) - ‘mysql-connector-java-5.1.46.tar.gz’ saved [4434926/4434926]

[centos@util01 ~]$ tar zxvf mysql-connector-java-5.1.46.tar.gz

[centos@util01 ~]$ sudo mkdir -p /usr/share/java/
[centos@util01 ~]$ cd mysql-connector-java-5.1.46
[centos@util01 mysql-connector-java-5.1.46]$ sudo cp mysql-connector-java-5.1.46-bin.jar /usr/share/java/mysql-connector-java.jar
[centos@util01 mysql-connector-java-5.1.46]$


MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| amon               |
| cdh                |
| hive               |
| hue                |
| metastore          |
| mysql              |
| nav                |
| navms              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
| sentry             |
+--------------------+
14 rows in set (0.00 sec)


centos@util01 ~]$ sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm
Enter SCM password:
JAVA_HOME=/usr/lib/jvm/java-openjdk
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/lib/jvm/java-openjdk/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/java/postgresql-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
[                          main] DbCommandExecutor              INFO  Successfully connected to database.
All done, your SCM database is configured correctly!
```
