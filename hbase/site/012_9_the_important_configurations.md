9. The Important Configurations
Below we list some important configurations. We’ve divided this section into required configuration and worth-a-look recommended configs.

9.1. Required Configurations
Review the os and hadoop sections.

9.1.1. Big Cluster Configurations
If you have a cluster with a lot of regions, it is possible that a Regionserver checks in briefly after the Master starts while all the remaining RegionServers lag behind. This first server to check in will be assigned all regions which is not optimal. To prevent the above scenario from happening, up the hbase.master.wait.on.regionservers.mintostart property from its default value of 1. See HBASE-6389 Modify the conditions to ensure that Master waits for sufficient number of Region Servers before starting region assignments for more detail.

9.2. Recommended Configurations
9.2.1. ZooKeeper Configuration
zookeeper.session.timeout

The default timeout is three minutes (specified in milliseconds). This means that if a server crashes, it will be three minutes before the Master notices the crash and starts recovery. You might like to tune the timeout down to a minute or even less so the Master notices failures the sooner. Before changing this value, be sure you have your JVM garbage collection configuration under control otherwise, a long garbage collection that lasts beyond the ZooKeeper session timeout will take out your RegionServer (You might be fine with this — you probably want recovery to start on the server if a RegionServer has been in GC for a long period of time).

To change this configuration, edit hbase-site.xml, copy the changed file around the cluster and restart.

We set this value high to save our having to field questions up on the mailing lists asking why a RegionServer went down during a massive import. The usual cause is that their JVM is untuned and they are running into long GC pauses. Our thinking is that while users are getting familiar with HBase, we’d save them having to know all of its intricacies. Later when they’ve built some confidence, then they can play with configuration such as this.

Number of ZooKeeper Instances

See zookeeper.

9.2.2. HDFS Configurations
dfs.datanode.failed.volumes.tolerated

This is the "…​number of volumes that are allowed to fail before a DataNode stops offering service. By default any volume failure will cause a datanode to shutdown" from the hdfs-default.xml description. You might want to set this to about half the amount of your available disks.

9.2.3. hbase.regionserver.handler.count
This setting defines the number of threads that are kept open to answer incoming requests to user tables. The rule of thumb is to keep this number low when the payload per request approaches the MB (big puts, scans using a large cache) and high when the payload is small (gets, small puts, ICVs, deletes). The total size of the queries in progress is limited by the setting hbase.ipc.server.max.callqueue.size.

It is safe to set that number to the maximum number of incoming clients if their payload is small, the typical example being a cluster that serves a website since puts aren’t typically buffered and most of the operations are gets.

The reason why it is dangerous to keep this setting high is that the aggregate size of all the puts that are currently happening in a region server may impose too much pressure on its memory, or even trigger an OutOfMemoryError. A RegionServer running on low memory will trigger its JVM’s garbage collector to run more frequently up to a point where GC pauses become noticeable (the reason being that all the memory used to keep all the requests' payloads cannot be trashed, no matter how hard the garbage collector tries). After some time, the overall cluster throughput is affected since every request that hits that RegionServer will take longer, which exacerbates the problem even more.

You can get a sense of whether you have too little or too many handlers by rpc.logging on an individual RegionServer then tailing its logs (Queued requests consume memory).

9.2.4. Configuration for large memory machines
HBase ships with a reasonable, conservative configuration that will work on nearly all machine types that people might want to test with. If you have larger machines — HBase has 8G and larger heap — you might the following configuration options helpful. TODO.

9.2.5. Compression
You should consider enabling ColumnFamily compression. There are several options that are near-frictionless and in most all cases boost performance by reducing the size of StoreFiles and thus reducing I/O.

See compression for more information.

9.2.6. Configuring the size and number of WAL files
HBase uses wal to recover the memstore data that has not been flushed to disk in case of an RS failure. These WAL files should be configured to be slightly smaller than HDFS block (by default a HDFS block is 64Mb and a WAL file is ~60Mb).

HBase also has a limit on the number of WAL files, designed to ensure there’s never too much data that needs to be replayed during recovery. This limit needs to be set according to memstore configuration, so that all the necessary data would fit. It is recommended to allocate enough WAL files to store at least that much data (when all memstores are close to full). For example, with 16Gb RS heap, default memstore settings (0.4), and default WAL file size (~60Mb), 16Gb*0.4/60, the starting point for WAL file count is ~109. However, as all memstores are not expected to be full all the time, less WAL files can be allocated.

9.2.7. Managed Splitting
HBase generally handles splitting your regions, based upon the settings in your hbase-default.xml and hbase-site.xml configuration files. Important settings include hbase.regionserver.region.split.policy, hbase.hregion.max.filesize, hbase.regionserver.regionSplitLimit. A simplistic view of splitting is that when a region grows to hbase.hregion.max.filesize, it is split. For most use patterns, most of the time, you should use automatic splitting. See manual region splitting decisions for more information about manual region splitting.

Instead of allowing HBase to split your regions automatically, you can choose to manage the splitting yourself. This feature was added in HBase 0.90.0. Manually managing splits works if you know your keyspace well, otherwise let HBase figure where to split for you. Manual splitting can mitigate region creation and movement under load. It also makes it so region boundaries are known and invariant (if you disable region splitting). If you use manual splits, it is easier doing staggered, time-based major compactions to spread out your network IO load.

Disable Automatic Splitting
To disable automatic splitting, set hbase.hregion.max.filesize to a very large value, such as 100 GB It is not recommended to set it to its absolute maximum value of Long.MAX_VALUE.

Automatic Splitting Is Recommended
If you disable automatic splits to diagnose a problem or during a period of fast data growth, it is recommended to re-enable them when your situation becomes more stable. The potential benefits of managing region splits yourself are not undisputed.
Determine the Optimal Number of Pre-Split Regions
The optimal number of pre-split regions depends on your application and environment. A good rule of thumb is to start with 10 pre-split regions per server and watch as data grows over time. It is better to err on the side of too few regions and perform rolling splits later. The optimal number of regions depends upon the largest StoreFile in your region. The size of the largest StoreFile will increase with time if the amount of data grows. The goal is for the largest region to be just large enough that the compaction selection algorithm only compacts it during a timed major compaction. Otherwise, the cluster can be prone to compaction storms where a large number of regions under compaction at the same time. It is important to understand that the data growth causes compaction storms, and not the manual split decision.

If the regions are split into too many large regions, you can increase the major compaction interval by configuring HConstants.MAJOR_COMPACTION_PERIOD. HBase 0.90 introduced org.apache.hadoop.hbase.util.RegionSplitter, which provides a network-IO-safe rolling split of all regions.

9.2.8. Managed Compactions
By default, major compactions are scheduled to run once in a 7-day period. Prior to HBase 0.96.x, major compactions were scheduled to happen once per day by default.

If you need to control exactly when and how often major compaction runs, you can disable managed major compactions. See the entry for hbase.hregion.majorcompaction in the compaction.parameters table for details.

Do Not Disable Major Compactions
Major compactions are absolutely necessary for StoreFile clean-up. Do not disable them altogether. You can run major compactions manually via the HBase shell or via the Admin API.
For more information about compactions and the compaction file selection process, see compaction

9.2.9. Speculative Execution
Speculative Execution of MapReduce tasks is on by default, and for HBase clusters it is generally advised to turn off Speculative Execution at a system-level unless you need it for a specific case, where it can be configured per-job. Set the properties mapreduce.map.speculative and mapreduce.reduce.speculative to false.

9.3. Other Configurations
9.3.1. Balancer
The balancer is a periodic operation which is run on the master to redistribute regions on the cluster. It is configured via hbase.balancer.period and defaults to 300000 (5 minutes).

See master.processes.loadbalancer for more information on the LoadBalancer.

9.3.2. Disabling Blockcache
Do not turn off block cache (You’d do it by setting hfile.block.cache.size to zero). Currently we do not do well if you do this because the RegionServer will spend all its time loading HFile indices over and over again. If your working set is such that block cache does you no good, at least size the block cache such that HFile indices will stay up in the cache (you can get a rough idea on the size you need by surveying RegionServer UIs; you’ll see index block size accounted near the top of the webpage).

9.3.3. Nagle’s or the small package problem
If a big 40ms or so occasional delay is seen in operations against HBase, try the Nagles' setting. For example, see the user mailing list thread, Inconsistent scan performance with caching set to 1 and the issue cited therein where setting notcpdelay improved scan speeds. You might also see the graphs on the tail of HBASE-7008 Set scanner caching to a better default where our Lars Hofhansl tries various data sizes w/ Nagle’s on and off measuring the effect.

9.3.4. Better Mean Time to Recover (MTTR)
This section is about configurations that will make servers come back faster after a fail. See the Deveraj Das and Nicolas Liochon blog post Introduction to HBase Mean Time to Recover (MTTR) for a brief introduction.

The issue HBASE-8354 forces Namenode into loop with lease recovery requests is messy but has a bunch of good discussion toward the end on low timeouts and how to effect faster recovery including citation of fixes added to HDFS. Read the Varun Sharma comments. The below suggested configurations are Varun’s suggestions distilled and tested. Make sure you are running on a late-version HDFS so you have the fixes he refers too and himself adds to HDFS that help HBase MTTR (e.g. HDFS-3703, HDFS-3712, and HDFS-4791 — Hadoop 2 for sure has them and late Hadoop 1 has some). Set the following in the RegionServer.

<property>
  <name>hbase.lease.recovery.dfs.timeout</name>
  <value>23000</value>
  <description>How much time we allow elapse between calls to recover lease.
  Should be larger than the dfs timeout.</description>
</property>
<property>
  <name>dfs.client.socket-timeout</name>
  <value>10000</value>
  <description>Down the DFS timeout from 60 to 10 seconds.</description>
</property>
And on the NameNode/DataNode side, set the following to enable 'staleness' introduced in HDFS-3703, HDFS-3912.

<property>
  <name>dfs.client.socket-timeout</name>
  <value>10000</value>
  <description>Down the DFS timeout from 60 to 10 seconds.</description>
</property>
<property>
  <name>dfs.datanode.socket.write.timeout</name>
  <value>10000</value>
  <description>Down the DFS timeout from 8 * 60 to 10 seconds.</description>
</property>
<property>
  <name>ipc.client.connect.timeout</name>
  <value>3000</value>
  <description>Down from 60 seconds to 3.</description>
</property>
<property>
  <name>ipc.client.connect.max.retries.on.timeouts</name>
  <value>2</value>
  <description>Down from 45 seconds to 3 (2 == 3 retries).</description>
</property>
<property>
  <name>dfs.namenode.avoid.read.stale.datanode</name>
  <value>true</value>
  <description>Enable stale state in hdfs</description>
</property>
<property>
  <name>dfs.namenode.stale.datanode.interval</name>
  <value>20000</value>
  <description>Down from default 30 seconds</description>
</property>
<property>
  <name>dfs.namenode.avoid.write.stale.datanode</name>
  <value>true</value>
  <description>Enable stale state in hdfs</description>
</property>
9.3.5. JMX
JMX (Java Management Extensions) provides built-in instrumentation that enables you to monitor and manage the Java VM. To enable monitoring and management from remote systems, you need to set system property com.sun.management.jmxremote.port (the port number through which you want to enable JMX RMI connections) when you start the Java VM. See the official documentation for more information. Historically, besides above port mentioned, JMX opens two additional random TCP listening ports, which could lead to port conflict problem. (See HBASE-10289 for details)

As an alternative, You can use the coprocessor-based JMX implementation provided by HBase. To enable it in 0.99 or above, add below property in hbase-site.xml:

<property>
  <name>hbase.coprocessor.regionserver.classes</name>
  <value>org.apache.hadoop.hbase.JMXListener</value>
</property>
DO NOT set com.sun.management.jmxremote.port for Java VM at the same time.
Currently it supports Master and RegionServer Java VM. By default, the JMX listens on TCP port 10102, you can further configure the port using below properties:

<property>
  <name>regionserver.rmi.registry.port</name>
  <value>61130</value>
</property>
<property>
  <name>regionserver.rmi.connector.port</name>
  <value>61140</value>
</property>
The registry port can be shared with connector port in most cases, so you only need to configure regionserver.rmi.registry.port. However if you want to use SSL communication, the 2 ports must be configured to different values.

By default the password authentication and SSL communication is disabled. To enable password authentication, you need to update hbase-env.sh like below:

export HBASE_JMX_BASE="-Dcom.sun.management.jmxremote.authenticate=true                  \
                       -Dcom.sun.management.jmxremote.password.file=your_password_file   \
                       -Dcom.sun.management.jmxremote.access.file=your_access_file"

export HBASE_MASTER_OPTS="$HBASE_MASTER_OPTS $HBASE_JMX_BASE "
export HBASE_REGIONSERVER_OPTS="$HBASE_REGIONSERVER_OPTS $HBASE_JMX_BASE "
See example password/access file under $JRE_HOME/lib/management.

To enable SSL communication with password authentication, follow below steps:

#1. generate a key pair, stored in myKeyStore
keytool -genkey -alias jconsole -keystore myKeyStore

#2. export it to file jconsole.cert
keytool -export -alias jconsole -keystore myKeyStore -file jconsole.cert

#3. copy jconsole.cert to jconsole client machine, import it to jconsoleKeyStore
keytool -import -alias jconsole -keystore jconsoleKeyStore -file jconsole.cert
And then update hbase-env.sh like below:

export HBASE_JMX_BASE="-Dcom.sun.management.jmxremote.ssl=true                         \
                       -Djavax.net.ssl.keyStore=/home/tianq/myKeyStore                 \
                       -Djavax.net.ssl.keyStorePassword=your_password_in_step_1       \
                       -Dcom.sun.management.jmxremote.authenticate=true                \
                       -Dcom.sun.management.jmxremote.password.file=your_password file \
                       -Dcom.sun.management.jmxremote.access.file=your_access_file"

export HBASE_MASTER_OPTS="$HBASE_MASTER_OPTS $HBASE_JMX_BASE "
export HBASE_REGIONSERVER_OPTS="$HBASE_REGIONSERVER_OPTS $HBASE_JMX_BASE "
Finally start jconsole on the client using the key store:

jconsole -J-Djavax.net.ssl.trustStore=/home/tianq/jconsoleKeyStore
To enable the HBase JMX implementation on Master, you also need to add below property in hbase-site.xml:
<property>
  <name>hbase.coprocessor.master.classes</name>
  <value>org.apache.hadoop.hbase.JMXListener</value>
</property>
The corresponding properties for port configuration are master.rmi.registry.port (by default 10101) and master.rmi.connector.port (by default the same as registry.port)