8. Example Configurations
8.1. Basic Distributed HBase Install
Here is an example basic configuration for a distributed ten node cluster: * The nodes are named example0, example1, etc., through node example9 in this example. * The HBase Master and the HDFS NameNode are running on the node example0. * RegionServers run on nodes example1-example9. * A 3-node ZooKeeper ensemble runs on example1, example2, and example3 on the default ports. * ZooKeeper data is persisted to the directory /export/zookeeper.

Below we show what the main configuration files — hbase-site.xml, regionservers, and hbase-env.sh — found in the HBase conf directory might look like.

8.1.1. hbase-site.xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>example1,example2,example3</value>
    <description>The directory shared by RegionServers.
    </description>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/export/zookeeper</value>
    <description>Property from ZooKeeper config zoo.cfg.
    The directory where the snapshot is stored.
    </description>
  </property>
  <property>
    <name>hbase.rootdir</name>
    <value>hdfs://example0:8020/hbase</value>
    <description>The directory shared by RegionServers.
    </description>
  </property>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
    <description>The mode the cluster will be in. Possible values are
      false: standalone and pseudo-distributed setups with managed ZooKeeper
      true: fully-distributed with unmanaged ZooKeeper Quorum (see hbase-env.sh)
    </description>
  </property>
</configuration>
8.1.2. regionservers
In this file you list the nodes that will run RegionServers. In our case, these nodes are example1-example9.

example1
example2
example3
example4
example5
example6
example7
example8
example9
8.1.3. hbase-env.sh
The following lines in the hbase-env.sh file show how to set the JAVA_HOME environment variable (required for HBase) and set the heap to 4 GB (rather than the default value of 1 GB). If you copy and paste this example, be sure to adjust the JAVA_HOME to suit your environment.

# The java implementation to use.
export JAVA_HOME=/usr/java/jdk1.8.0/

# The maximum amount of heap to use. Default is left to JVM default.
export HBASE_HEAPSIZE=4G
Use rsync to copy the content of the conf directory to all nodes of the cluster.