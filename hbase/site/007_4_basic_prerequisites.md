4. Basic Prerequisites
This section lists required services and some required system configuration.

Table 2. Java
HBase Version	JDK 7	JDK 8
2.0
Not Supported
yes
1.3
yes
yes
1.2
yes
yes
1.1
yes
Running with JDK 8 will work but is not well tested.
HBase will neither build nor compile with Java 6.
You must set JAVA_HOME on each node of your cluster. hbase-env.sh provides a handy mechanism to do this.
Operating System Utilities
ssh
HBase uses the Secure Shell (ssh) command and utilities extensively to communicate between cluster nodes. Each server in the cluster must be running ssh so that the Hadoop and HBase daemons can be managed. You must be able to connect to all nodes via SSH, including the local node, from the Master as well as any backup Master, using a shared key rather than a password. You can see the basic methodology for such a set-up in Linux or Unix systems at "Procedure: Configure Passwordless SSH Access". If your cluster nodes use OS X, see the section, SSH: Setting up Remote Desktop and Enabling Self-Login on the Hadoop wiki.

DNS
HBase uses the local hostname to self-report its IP address. Both forward and reverse DNS resolving must work in versions of HBase previous to 0.92.0. The hadoop-dns-checker tool can be used to verify DNS is working correctly on the cluster. The project README file provides detailed instructions on usage.

Loopback IP
Prior to hbase-0.96.0, HBase only used the IP address 127.0.0.1 to refer to localhost, and this could not be configured. See Loopback IP for more details.

NTP
The clocks on cluster nodes should be synchronized. A small amount of variation is acceptable, but larger amounts of skew can cause erratic and unexpected behavior. Time synchronization is one of the first things to check if you see unexplained problems in your cluster. It is recommended that you run a Network Time Protocol (NTP) service, or another time-synchronization mechanism, on your cluster, and that all nodes look to the same service for time synchronization. See the Basic NTP Configuration at The Linux Documentation Project (TLDP) to set up NTP.
Limits on Number of Files and Processes (ulimit)
Apache HBase is a database. It requires the ability to open a large number of files at once. Many Linux distributions limit the number of files a single user is allowed to open to 1024 (or 256 on older versions of OS X). You can check this limit on your servers by running the command ulimit -n when logged in as the user which runs HBase. See the Troubleshooting section for some of the problems you may experience if the limit is too low. You may also notice errors such as the following:

2010-04-06 03:04:37,542 INFO org.apache.hadoop.hdfs.DFSClient: Exception increateBlockOutputStream java.io.EOFException
2010-04-06 03:04:37,542 INFO org.apache.hadoop.hdfs.DFSClient: Abandoning block blk_-6935524980745310745_1391901
It is recommended to raise the ulimit to at least 10,000, but more likely 10,240, because the value is usually expressed in multiples of 1024. Each ColumnFamily has at least one StoreFile, and possibly more than six StoreFiles if the region is under load. The number of open files required depends upon the number of ColumnFamilies and the number of regions. The following is a rough formula for calculating the potential number of open files on a RegionServer.

Calculate the Potential Number of Open Files
(StoreFiles per ColumnFamily) x (regions per RegionServer)
For example, assuming that a schema had 3 ColumnFamilies per region with an average of 3 StoreFiles per ColumnFamily, and there are 100 regions per RegionServer, the JVM will open 3 * 3 * 100 = 900 file descriptors, not counting open JAR files, configuration files, and others. Opening a file does not take many resources, and the risk of allowing a user to open too many files is minimal.

Another related setting is the number of processes a user is allowed to run at once. In Linux and Unix, the number of processes is set using the ulimit -u command. This should not be confused with the nproc command, which controls the number of CPUs available to a given user. Under load, a ulimit -u that is too low can cause OutOfMemoryError exceptions. See Jack Levin’s major HDFS issues thread on the hbase-users mailing list, from 2011.

Configuring the maximum number of file descriptors and processes for the user who is running the HBase process is an operating system configuration, rather than an HBase configuration. It is also important to be sure that the settings are changed for the user that actually runs HBase. To see which user started HBase, and that user’s ulimit configuration, look at the first line of the HBase log for that instance. A useful read setting config on your hadoop cluster is Aaron Kimball’s Configuration Parameters: What can you just ignore?

Example 5. ulimit Settings on Ubuntu
To configure ulimit settings on Ubuntu, edit /etc/security/limits.conf, which is a space-delimited file with four columns. Refer to the man page for limits.conf for details about the format of this file. In the following example, the first line sets both soft and hard limits for the number of open files (nofile) to 32768 for the operating system user with the username hadoop. The second line sets the number of processes to 32000 for the same user.

hadoop  -       nofile  32768
hadoop  -       nproc   32000
The settings are only applied if the Pluggable Authentication Module (PAM) environment is directed to use them. To configure PAM to use these limits, be sure that the /etc/pam.d/common-session file contains the following line:

session required  pam_limits.so
Linux Shell
All of the shell scripts that come with HBase rely on the GNU Bash shell.

Windows
Prior to HBase 0.96, testing for running HBase on Microsoft Windows was limited. Running a on Windows nodes is not recommended for production systems.
4.1. Hadoop
The following table summarizes the versions of Hadoop supported with each version of HBase. Based on the version of HBase, you should select the most appropriate version of Hadoop. You can use Apache Hadoop, or a vendor’s distribution of Hadoop. No distinction is made here. See the Hadoop wiki for information about vendors of Hadoop.

Hadoop 2.x is recommended.
Hadoop 2.x is faster and includes features, such as short-circuit reads, which will help improve your HBase random read profile. Hadoop 2.x also includes important bug fixes that will improve your overall HBase experience. HBase does not support running with earlier versions of Hadoop. See the table below for requirements specific to different HBase versions.

Hadoop 3.x is still in early access releases and has not yet been sufficiently tested by the HBase community for production use cases.
Use the following legend to interpret this table:

Hadoop version support matrix
"S" = supported

"X" = not supported

"NT" = Not tested

HBase-1.1.x	HBase-1.2.x	HBase-1.3.x	HBase-2.0.x
Hadoop-2.0.x-alpha
X
X
X
X
Hadoop-2.1.0-beta
X
X
X
X
Hadoop-2.2.0
NT
X
X
X
Hadoop-2.3.x
NT
X
X
X
Hadoop-2.4.x
S
S
S
X
Hadoop-2.5.x
S
S
S
X
Hadoop-2.6.0
X
X
X
X
Hadoop-2.6.1+
NT
S
S
S
Hadoop-2.7.0
X
X
X
X
Hadoop-2.7.1+
NT
S
S
S
Hadoop-2.8.0
X
X
X
X
Hadoop-3.0.0-alphax
NT
NT
NT
NT
Hadoop Pre-2.6.1 and JDK 1.8 Kerberos
When using pre-2.6.1 Hadoop versions and JDK 1.8 in a Kerberos environment, HBase server can fail and abort due to Kerberos keytab relogin error. Late version of JDK 1.7 (1.7.0_80) has the problem too. Refer to HADOOP-10786 for additional details. Consider upgrading to Hadoop 2.6.1+ in this case.
Hadoop 2.6.x
Hadoop distributions based on the 2.6.x line must have HADOOP-11710 applied if you plan to run HBase on top of an HDFS Encryption Zone. Failure to do so will result in cluster failure and data loss. This patch is present in Apache Hadoop releases 2.6.1+.
Hadoop 2.7.x
Hadoop version 2.7.0 is not tested or supported as the Hadoop PMC has explicitly labeled that release as not being stable. (reference the announcement of Apache Hadoop 2.7.0.)
Hadoop 2.8.x
Hadoop version 2.8.0 is not tested or supported as the Hadoop PMC has explicitly labeled that release as not being stable. (reference the announcement of Apache Hadoop 2.8.0.)
Replace the Hadoop Bundled With HBase!
Because HBase depends on Hadoop, it bundles an instance of the Hadoop jar under its lib directory. The bundled jar is ONLY for use in standalone mode. In distributed mode, it is critical that the version of Hadoop that is out on your cluster match what is under HBase. Replace the hadoop jar found in the HBase lib directory with the hadoop jar you are running on your cluster to avoid version mismatch issues. Make sure you replace the jar in HBase everywhere on your cluster. Hadoop version mismatch issues have various manifestations but often all looks like its hung up.
4.1.1. dfs.datanode.max.transfer.threads
An HDFS DataNode has an upper bound on the number of files that it will serve at any one time. Before doing any loading, make sure you have configured Hadoop’s conf/hdfs-site.xml, setting the dfs.datanode.max.transfer.threads value to at least the following:

<property>
  <name>dfs.datanode.max.transfer.threads</name>
  <value>4096</value>
</property>
Be sure to restart your HDFS after making the above configuration.

Not having this configuration in place makes for strange-looking failures. One manifestation is a complaint about missing blocks. For example:

10/12/08 20:10:31 INFO hdfs.DFSClient: Could not obtain block
          blk_XXXXXXXXXXXXXXXXXXXXXX_YYYYYYYY from any node: java.io.IOException: No live nodes
          contain current block. Will get new block locations from namenode and retry...
See also casestudies.max.transfer.threads and note that this property was previously known as dfs.datanode.max.xcievers (e.g. Hadoop HDFS: Deceived by Xciever).

4.2. ZooKeeper Requirements
ZooKeeper 3.4.x is required. HBase makes use of the multi functionality that is only available since Zookeeper 3.4.0. The hbase.zookeeper.useMulti configuration property defaults to true. Refer to HBASE-12241 (The crash of regionServer when taking deadserver’s replication queue breaks replication) and HBASE-6775 (Use ZK.multi when available for HBASE-6710 0.92/0.94 compatibility fix) for background. The property is deprecated and useMulti is always enabled in HBase 2.0.