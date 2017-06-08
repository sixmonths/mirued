3. Configuration Files
Apache HBase uses the same configuration system as Apache Hadoop. All configuration files are located in the conf/ directory, which needs to be kept in sync for each node on your cluster.

HBase Configuration File Descriptions
backup-masters
Not present by default. A plain-text file which lists hosts on which the Master should start a backup Master process, one host per line.

hadoop-metrics2-hbase.properties
Used to connect HBase Hadoop’s Metrics2 framework. See the Hadoop Wiki entry for more information on Metrics2. Contains only commented-out examples by default.

hbase-env.cmd and hbase-env.sh
Script for Windows and Linux / Unix environments to set up the working environment for HBase, including the location of Java, Java options, and other environment variables. The file contains many commented-out examples to provide guidance.

hbase-policy.xml
The default policy configuration file used by RPC servers to make authorization decisions on client requests. Only used if HBase security is enabled.

hbase-site.xml
The main HBase configuration file. This file specifies configuration options which override HBase’s default configuration. You can view (but do not edit) the default configuration file at docs/hbase-default.xml. You can also view the entire effective configuration for your cluster (defaults and overrides) in the HBase Configuration tab of the HBase Web UI.

log4j.properties
Configuration file for HBase logging via log4j.

regionservers
A plain-text file containing a list of hosts which should run a RegionServer in your HBase cluster. By default this file contains the single entry localhost. It should contain a list of hostnames or IP addresses, one per line, and should only contain localhost if each node in your cluster will run a RegionServer on its localhost interface.
Checking XML Validity
When you edit XML, it is a good idea to use an XML-aware editor to be sure that your syntax is correct and your XML is well-formed. You can also use the xmllint utility to check that your XML is well-formed. By default, xmllint re-flows and prints the XML to standard output. To check for well-formedness and only print output if errors exist, use the command xmllint -noout filename.xml.
Keep Configuration In Sync Across the Cluster
When running in distributed mode, after you make an edit to an HBase configuration, make sure you copy the content of the conf/ directory to all nodes of the cluster. HBase will not do this for you. Use rsync, scp, or another secure mechanism for copying the configuration files to your nodes. For most configuration, a restart is needed for servers to pick up changes An exception is dynamic configuration. to be described later below.