7. Default Configuration
7.1. hbase-site.xml and hbase-default.xml
Just as in Hadoop where you add site-specific HDFS configuration to the hdfs-site.xml file, for HBase, site specific customizations go into the file conf/hbase-site.xml. For the list of configurable properties, see hbase default configurations below or view the raw hbase-default.xml source file in the HBase source code at src/main/resources.

Not all configuration options make it out to hbase-default.xml. Configuration that it is thought rare anyone would change can exist only in code; the only way to turn up such configurations is via a reading of the source code itself.

Currently, changes here will require a cluster restart for HBase to notice the change.

7.2. HBase Default Configuration
The documentation below is generated using the default hbase configuration file, hbase-default.xml, as source.

hbase.tmp.dir
Description
Temporary directory on the local filesystem. Change this setting to point to a location more permanent than '/tmp', the usual resolve for java.io.tmpdir, as the '/tmp' directory is cleared on machine restart.

Default
${java.io.tmpdir}/hbase-${user.name}

hbase.rootdir
Description
The directory shared by region servers and into which HBase persists. The URL should be 'fully-qualified' to include the filesystem scheme. For example, to specify the HDFS directory '/hbase' where the HDFS instance’s namenode is running at namenode.example.org on port 9000, set this value to: hdfs://namenode.example.org:9000/hbase. By default, we write to whatever ${hbase.tmp.dir} is set too — usually /tmp — so change this configuration or else all data will be lost on machine restart.

Default
${hbase.tmp.dir}/hbase

hbase.fs.tmp.dir
Description
A staging directory in default file system (HDFS) for keeping temporary data.

Default
/user/${user.name}/hbase-staging

hbase.cluster.distributed
Description
The mode the cluster will be in. Possible values are false for standalone mode and true for distributed mode. If false, startup will run all HBase and ZooKeeper daemons together in the one JVM.

Default
false

hbase.zookeeper.quorum
Description
Comma separated list of servers in the ZooKeeper ensemble (This config. should have been named hbase.zookeeper.ensemble). For example, "host1.mydomain.com,host2.mydomain.com,host3.mydomain.com". By default this is set to localhost for local and pseudo-distributed modes of operation. For a fully-distributed setup, this should be set to a full list of ZooKeeper ensemble servers. If HBASE_MANAGES_ZK is set in hbase-env.sh this is the list of servers which hbase will start/stop ZooKeeper on as part of cluster start/stop. Client-side, we will take this list of ensemble members and put it together with the hbase.zookeeper.clientPort config. and pass it into zookeeper constructor as the connectString parameter.

Default
localhost

zookeeper.recovery.retry.maxsleeptime
Description
Max sleep time before retry zookeeper operations in milliseconds, a max time is needed here so that sleep time won’t grow unboundedly

Default
60000

hbase.local.dir
Description
Directory on the local filesystem to be used as a local storage.

Default
${hbase.tmp.dir}/local/

hbase.master.port
Description
The port the HBase Master should bind to.

Default
16000

hbase.master.info.port
Description
The port for the HBase Master web UI. Set to -1 if you do not want a UI instance run.

Default
16010

hbase.master.info.bindAddress
Description
The bind address for the HBase Master web UI

Default
0.0.0.0

hbase.master.logcleaner.plugins
Description
A comma-separated list of BaseLogCleanerDelegate invoked by the LogsCleaner service. These WAL cleaners are called in order, so put the cleaner that prunes the most files in front. To implement your own BaseLogCleanerDelegate, just put it in HBase’s classpath and add the fully qualified class name here. Always add the above default log cleaners in the list.

Default
org.apache.hadoop.hbase.master.cleaner.TimeToLiveLogCleaner

hbase.master.logcleaner.ttl
Description
Maximum time a WAL can stay in the .oldlogdir directory, after which it will be cleaned by a Master thread.

Default
600000

hbase.master.hfilecleaner.plugins
Description
A comma-separated list of BaseHFileCleanerDelegate invoked by the HFileCleaner service. These HFiles cleaners are called in order, so put the cleaner that prunes the most files in front. To implement your own BaseHFileCleanerDelegate, just put it in HBase’s classpath and add the fully qualified class name here. Always add the above default log cleaners in the list as they will be overwritten in hbase-site.xml.

Default
org.apache.hadoop.hbase.master.cleaner.TimeToLiveHFileCleaner

hbase.master.infoserver.redirect
Description
Whether or not the Master listens to the Master web UI port (hbase.master.info.port) and redirects requests to the web UI server shared by the Master and RegionServer.

Default
true

hbase.regionserver.port
Description
The port the HBase RegionServer binds to.

Default
16020

hbase.regionserver.info.port
Description
The port for the HBase RegionServer web UI Set to -1 if you do not want the RegionServer UI to run.

Default
16030

hbase.regionserver.info.bindAddress
Description
The address for the HBase RegionServer web UI

Default
0.0.0.0

hbase.regionserver.info.port.auto
Description
Whether or not the Master or RegionServer UI should search for a port to bind to. Enables automatic port search if hbase.regionserver.info.port is already in use. Useful for testing, turned off by default.

Default
false

hbase.regionserver.handler.count
Description
Count of RPC Listener instances spun up on RegionServers. Same property is used by the Master for count of master handlers.

Default
30

hbase.ipc.server.callqueue.handler.factor
Description
Factor to determine the number of call queues. A value of 0 means a single queue shared between all the handlers. A value of 1 means that each handler has its own queue.

Default
0.1

hbase.ipc.server.callqueue.read.ratio
Description
Split the call queues into read and write queues. The specified interval (which should be between 0.0 and 1.0) will be multiplied by the number of call queues. A value of 0 indicate to not split the call queues, meaning that both read and write requests will be pushed to the same set of queues. A value lower than 0.5 means that there will be less read queues than write queues. A value of 0.5 means there will be the same number of read and write queues. A value greater than 0.5 means that there will be more read queues than write queues. A value of 1.0 means that all the queues except one are used to dispatch read requests. Example: Given the total number of call queues being 10 a read.ratio of 0 means that: the 10 queues will contain both read/write requests. a read.ratio of 0.3 means that: 3 queues will contain only read requests and 7 queues will contain only write requests. a read.ratio of 0.5 means that: 5 queues will contain only read requests and 5 queues will contain only write requests. a read.ratio of 0.8 means that: 8 queues will contain only read requests and 2 queues will contain only write requests. a read.ratio of 1 means that: 9 queues will contain only read requests and 1 queues will contain only write requests.

Default
0

hbase.ipc.server.callqueue.scan.ratio
Description
Given the number of read call queues, calculated from the total number of call queues multiplied by the callqueue.read.ratio, the scan.ratio property will split the read call queues into small-read and long-read queues. A value lower than 0.5 means that there will be less long-read queues than short-read queues. A value of 0.5 means that there will be the same number of short-read and long-read queues. A value greater than 0.5 means that there will be more long-read queues than short-read queues A value of 0 or 1 indicate to use the same set of queues for gets and scans. Example: Given the total number of read call queues being 8 a scan.ratio of 0 or 1 means that: 8 queues will contain both long and short read requests. a scan.ratio of 0.3 means that: 2 queues will contain only long-read requests and 6 queues will contain only short-read requests. a scan.ratio of 0.5 means that: 4 queues will contain only long-read requests and 4 queues will contain only short-read requests. a scan.ratio of 0.8 means that: 6 queues will contain only long-read requests and 2 queues will contain only short-read requests.

Default
0

hbase.regionserver.msginterval
Description
Interval between messages from the RegionServer to Master in milliseconds.

Default
3000

hbase.regionserver.logroll.period
Description
Period at which we will roll the commit log regardless of how many edits it has.

Default
3600000

hbase.regionserver.logroll.errors.tolerated
Description
The number of consecutive WAL close errors we will allow before triggering a server abort. A setting of 0 will cause the region server to abort if closing the current WAL writer fails during log rolling. Even a small value (2 or 3) will allow a region server to ride over transient HDFS errors.

Default
2

hbase.regionserver.hlog.reader.impl
Description
The WAL file reader implementation.

Default
org.apache.hadoop.hbase.regionserver.wal.ProtobufLogReader

hbase.regionserver.hlog.writer.impl
Description
The WAL file writer implementation.

Default
org.apache.hadoop.hbase.regionserver.wal.ProtobufLogWriter

hbase.regionserver.global.memstore.size
Description
Maximum size of all memstores in a region server before new updates are blocked and flushes are forced. Defaults to 40% of heap (0.4). Updates are blocked and flushes are forced until size of all memstores in a region server hits hbase.regionserver.global.memstore.size.lower.limit. The default value in this configuration has been intentionally left empty in order to honor the old hbase.regionserver.global.memstore.upperLimit property if present.

Default
none

hbase.regionserver.global.memstore.size.lower.limit
Description
Maximum size of all memstores in a region server before flushes are forced. Defaults to 95% of hbase.regionserver.global.memstore.size (0.95). A 100% value for this value causes the minimum possible flushing to occur when updates are blocked due to memstore limiting. The default value in this configuration has been intentionally left empty in order to honor the old hbase.regionserver.global.memstore.lowerLimit property if present.

Default
none

hbase.regionserver.optionalcacheflushinterval
Description
Maximum amount of time an edit lives in memory before being automatically flushed. Default 1 hour. Set it to 0 to disable automatic flushing.

Default
3600000

hbase.regionserver.dns.interface
Description
The name of the Network Interface from which a region server should report its IP address.

Default
default

hbase.regionserver.dns.nameserver
Description
The host name or IP address of the name server (DNS) which a region server should use to determine the host name used by the master for communication and display purposes.

Default
default

hbase.regionserver.region.split.policy
Description
A split policy determines when a region should be split. The various other split policies that are available currently are BusyRegionSplitPolicy, ConstantSizeRegionSplitPolicy, DisabledRegionSplitPolicy, DelimitedKeyPrefixRegionSplitPolicy, and KeyPrefixRegionSplitPolicy. DisabledRegionSplitPolicy blocks manual region splitting.

Default
org.apache.hadoop.hbase.regionserver.SteppingSplitPolicy

hbase.regionserver.regionSplitLimit
Description
Limit for the number of regions after which no more region splitting should take place. This is not hard limit for the number of regions but acts as a guideline for the regionserver to stop splitting after a certain limit. Default is set to 1000.

Default
1000

zookeeper.session.timeout
Description
ZooKeeper session timeout in milliseconds. It is used in two different ways. First, this value is used in the ZK client that HBase uses to connect to the ensemble. It is also used by HBase when it starts a ZK server and it is passed as the 'maxSessionTimeout'. See http://hadoop.apache.org/zookeeper/docs/current/zookeeperProgrammers.html#ch_zkSessions. For example, if an HBase region server connects to a ZK ensemble that’s also managed by HBase, then the session timeout will be the one specified by this configuration. But, a region server that connects to an ensemble managed with a different configuration will be subjected that ensemble’s maxSessionTimeout. So, even though HBase might propose using 90 seconds, the ensemble can have a max timeout lower than this and it will take precedence. The current default that ZK ships with is 40 seconds, which is lower than HBase’s.

Default
90000

zookeeper.znode.parent
Description
Root ZNode for HBase in ZooKeeper. All of HBase’s ZooKeeper files that are configured with a relative path will go under this node. By default, all of HBase’s ZooKeeper file paths are configured with a relative path, so they will all go under this directory unless changed.

Default
/hbase

zookeeper.znode.acl.parent
Description
Root ZNode for access control lists.

Default
acl

hbase.zookeeper.dns.interface
Description
The name of the Network Interface from which a ZooKeeper server should report its IP address.

Default
default

hbase.zookeeper.dns.nameserver
Description
The host name or IP address of the name server (DNS) which a ZooKeeper server should use to determine the host name used by the master for communication and display purposes.

Default
default

hbase.zookeeper.peerport
Description
Port used by ZooKeeper peers to talk to each other. See http://hadoop.apache.org/zookeeper/docs/r3.1.1/zookeeperStarted.html#sc_RunningReplicatedZooKeeper for more information.

Default
2888

hbase.zookeeper.leaderport
Description
Port used by ZooKeeper for leader election. See http://hadoop.apache.org/zookeeper/docs/r3.1.1/zookeeperStarted.html#sc_RunningReplicatedZooKeeper for more information.

Default
3888

hbase.zookeeper.property.initLimit
Description
Property from ZooKeeper’s config zoo.cfg. The number of ticks that the initial synchronization phase can take.

Default
10

hbase.zookeeper.property.syncLimit
Description
Property from ZooKeeper’s config zoo.cfg. The number of ticks that can pass between sending a request and getting an acknowledgment.

Default
5

hbase.zookeeper.property.dataDir
Description
Property from ZooKeeper’s config zoo.cfg. The directory where the snapshot is stored.

Default
${hbase.tmp.dir}/zookeeper

hbase.zookeeper.property.clientPort
Description
Property from ZooKeeper’s config zoo.cfg. The port at which the clients will connect.

Default
2181

hbase.zookeeper.property.maxClientCnxns
Description
Property from ZooKeeper’s config zoo.cfg. Limit on number of concurrent connections (at the socket level) that a single client, identified by IP address, may make to a single member of the ZooKeeper ensemble. Set high to avoid zk connection issues running standalone and pseudo-distributed.

Default
300

hbase.client.write.buffer
Description
Default size of the HTable client write buffer in bytes. A bigger buffer takes more memory — on both the client and server side since server instantiates the passed write buffer to process it — but a larger buffer size reduces the number of RPCs made. For an estimate of server-side memory-used, evaluate hbase.client.write.buffer * hbase.regionserver.handler.count

Default
2097152

hbase.client.pause
Description
General client pause value. Used mostly as value to wait before running a retry of a failed get, region lookup, etc. See hbase.client.retries.number for description of how we backoff from this initial pause amount and how this pause works w/ retries.

Default
100

hbase.client.pause.cqtbe
Description
Whether or not to use a special client pause for CallQueueTooBigException (cqtbe). Set this property to a higher value than hbase.client.pause if you observe frequent CQTBE from the same RegionServer and the call queue there keeps full

Default
none

hbase.client.retries.number
Description
Maximum retries. Used as maximum for all retryable operations such as the getting of a cell’s value, starting a row update, etc. Retry interval is a rough function based on hbase.client.pause. At first we retry at this interval but then with backoff, we pretty quickly reach retrying every ten seconds. See HConstants#RETRY_BACKOFF for how the backup ramps up. Change this setting and hbase.client.pause to suit your workload.

Default
35

hbase.client.max.total.tasks
Description
The maximum number of concurrent mutation tasks a single HTable instance will send to the cluster.

Default
100

hbase.client.max.perserver.tasks
Description
The maximum number of concurrent mutation tasks a single HTable instance will send to a single region server.

Default
5

hbase.client.max.perregion.tasks
Description
The maximum number of concurrent mutation tasks the client will maintain to a single Region. That is, if there is already hbase.client.max.perregion.tasks writes in progress for this region, new puts won’t be sent to this region until some writes finishes.

Default
1

hbase.client.perserver.requests.threshold
Description
The max number of concurrent pending requests for one server in all client threads (process level). Exceeding requests will be thrown ServerTooBusyException immediately to prevent user’s threads being occupied and blocked by only one slow region server. If you use a fix number of threads to access HBase in a synchronous way, set this to a suitable value which is related to the number of threads will help you. See https://issues.apache.org/jira/browse/HBASE-16388 for details.

Default
2147483647

hbase.client.scanner.caching
Description
Number of rows that we try to fetch when calling next on a scanner if it is not served from (local, client) memory. This configuration works together with hbase.client.scanner.max.result.size to try and use the network efficiently. The default value is Integer.MAX_VALUE by default so that the network will fill the chunk size defined by hbase.client.scanner.max.result.size rather than be limited by a particular number of rows since the size of rows varies table to table. If you know ahead of time that you will not require more than a certain number of rows from a scan, this configuration should be set to that row limit via Scan#setCaching. Higher caching values will enable faster scanners but will eat up more memory and some calls of next may take longer and longer times when the cache is empty. Do not set this value such that the time between invocations is greater than the scanner timeout; i.e. hbase.client.scanner.timeout.period

Default
2147483647

hbase.client.keyvalue.maxsize
Description
Specifies the combined maximum allowed size of a KeyValue instance. This is to set an upper boundary for a single entry saved in a storage file. Since they cannot be split it helps avoiding that a region cannot be split any further because the data is too large. It seems wise to set this to a fraction of the maximum region size. Setting it to zero or less disables the check.

Default
10485760

hbase.server.keyvalue.maxsize
Description
Maximum allowed size of an individual cell, inclusive of value and all key components. A value of 0 or less disables the check. The default value is 10MB. This is a safety setting to protect the server from OOM situations.

Default
10485760

hbase.client.scanner.timeout.period
Description
Client scanner lease period in milliseconds.

Default
60000

hbase.client.localityCheck.threadPoolSize
Default
2

hbase.bulkload.retries.number
Description
Maximum retries. This is maximum number of iterations to atomic bulk loads are attempted in the face of splitting operations 0 means never give up.

Default
10

hbase.master.balancer.maxRitPercent
Description
The max percent of regions in transition when balancing. The default value is 1.0. So there are no balancer throttling. If set this config to 0.01, It means that there are at most 1% regions in transition when balancing. Then the cluster’s availability is at least 99% when balancing.

Default
1.0

hbase.balancer.period
Description
Period at which the region balancer runs in the Master.

Default
300000

hbase.normalizer.period
Description
Period at which the region normalizer runs in the Master.

Default
1800000

hbase.regions.slop
Description
Rebalance if any regionserver has average + (average * slop) regions. The default value of this parameter is 0.001 in StochasticLoadBalancer (the default load balancer), while the default is 0.2 in other load balancers (i.e., SimpleLoadBalancer).

Default
0.001

hbase.server.thread.wakefrequency
Description
Time to sleep in between searches for work (in milliseconds). Used as sleep interval by service threads such as log roller.

Default
10000

hbase.server.versionfile.writeattempts
Description
How many times to retry attempting to write a version file before just aborting. Each attempt is separated by the hbase.server.thread.wakefrequency milliseconds.

Default
3

hbase.hregion.memstore.flush.size
Description
Memstore will be flushed to disk if size of the memstore exceeds this number of bytes. Value is checked by a thread that runs every hbase.server.thread.wakefrequency.

Default
134217728

hbase.hregion.percolumnfamilyflush.size.lower.bound.min
Description
If FlushLargeStoresPolicy is used and there are multiple column families, then every time that we hit the total memstore limit, we find out all the column families whose memstores exceed a "lower bound" and only flush them while retaining the others in memory. The "lower bound" will be "hbase.hregion.memstore.flush.size / column_family_number" by default unless value of this property is larger than that. If none of the families have their memstore size more than lower bound, all the memstores will be flushed (just as usual).

Default
16777216

hbase.hregion.preclose.flush.size
Description
If the memstores in a region are this size or larger when we go to close, run a "pre-flush" to clear out memstores before we put up the region closed flag and take the region offline. On close, a flush is run under the close flag to empty memory. During this time the region is offline and we are not taking on any writes. If the memstore content is large, this flush could take a long time to complete. The preflush is meant to clean out the bulk of the memstore before putting up the close flag and taking the region offline so the flush that runs under the close flag has little to do.

Default
5242880

hbase.hregion.memstore.block.multiplier
Description
Block updates if memstore has hbase.hregion.memstore.block.multiplier times hbase.hregion.memstore.flush.size bytes. Useful preventing runaway memstore during spikes in update traffic. Without an upper-bound, memstore fills such that when it flushes the resultant flush files take a long time to compact or split, or worse, we OOME.

Default
4

hbase.hregion.memstore.mslab.enabled
Description
Enables the MemStore-Local Allocation Buffer, a feature which works to prevent heap fragmentation under heavy write loads. This can reduce the frequency of stop-the-world GC pauses on large heaps.

Default
true

hbase.hregion.max.filesize
Description
Maximum HFile size. If the sum of the sizes of a region’s HFiles has grown to exceed this value, the region is split in two.

Default
10737418240

hbase.hregion.majorcompaction
Description
Time between major compactions, expressed in milliseconds. Set to 0 to disable time-based automatic major compactions. User-requested and size-based major compactions will still run. This value is multiplied by hbase.hregion.majorcompaction.jitter to cause compaction to start at a somewhat-random time during a given window of time. The default value is 7 days, expressed in milliseconds. If major compactions are causing disruption in your environment, you can configure them to run at off-peak times for your deployment, or disable time-based major compactions by setting this parameter to 0, and run major compactions in a cron job or by another external mechanism.

Default
604800000

hbase.hregion.majorcompaction.jitter
Description
A multiplier applied to hbase.hregion.majorcompaction to cause compaction to occur a given amount of time either side of hbase.hregion.majorcompaction. The smaller the number, the closer the compactions will happen to the hbase.hregion.majorcompaction interval.

Default
0.50

hbase.hstore.compactionThreshold
Description
If more than this number of StoreFiles exist in any one Store (one StoreFile is written per flush of MemStore), a compaction is run to rewrite all StoreFiles into a single StoreFile. Larger values delay compaction, but when compaction does occur, it takes longer to complete.

Default
3

hbase.hstore.flusher.count
Description
The number of flush threads. With fewer threads, the MemStore flushes will be queued. With more threads, the flushes will be executed in parallel, increasing the load on HDFS, and potentially causing more compactions.

Default
2

hbase.hstore.blockingStoreFiles
Description
If more than this number of StoreFiles exist in any one Store (one StoreFile is written per flush of MemStore), updates are blocked for this region until a compaction is completed, or until hbase.hstore.blockingWaitTime has been exceeded.

Default
10

hbase.hstore.blockingWaitTime
Description
The time for which a region will block updates after reaching the StoreFile limit defined by hbase.hstore.blockingStoreFiles. After this time has elapsed, the region will stop blocking updates even if a compaction has not been completed.

Default
90000

hbase.hstore.compaction.min
Description
The minimum number of StoreFiles which must be eligible for compaction before compaction can run. The goal of tuning hbase.hstore.compaction.min is to avoid ending up with too many tiny StoreFiles to compact. Setting this value to 2 would cause a minor compaction each time you have two StoreFiles in a Store, and this is probably not appropriate. If you set this value too high, all the other values will need to be adjusted accordingly. For most cases, the default value is appropriate. In previous versions of HBase, the parameter hbase.hstore.compaction.min was named hbase.hstore.compactionThreshold.

Default
3

hbase.hstore.compaction.max
Description
The maximum number of StoreFiles which will be selected for a single minor compaction, regardless of the number of eligible StoreFiles. Effectively, the value of hbase.hstore.compaction.max controls the length of time it takes a single compaction to complete. Setting it larger means that more StoreFiles are included in a compaction. For most cases, the default value is appropriate.

Default
10

hbase.hstore.compaction.min.size
Description
A StoreFile (or a selection of StoreFiles, when using ExploringCompactionPolicy) smaller than this size will always be eligible for minor compaction. HFiles this size or larger are evaluated by hbase.hstore.compaction.ratio to determine if they are eligible. Because this limit represents the "automatic include" limit for all StoreFiles smaller than this value, this value may need to be reduced in write-heavy environments where many StoreFiles in the 1-2 MB range are being flushed, because every StoreFile will be targeted for compaction and the resulting StoreFiles may still be under the minimum size and require further compaction. If this parameter is lowered, the ratio check is triggered more quickly. This addressed some issues seen in earlier versions of HBase but changing this parameter is no longer necessary in most situations. Default: 128 MB expressed in bytes.

Default
134217728

hbase.hstore.compaction.max.size
Description
A StoreFile (or a selection of StoreFiles, when using ExploringCompactionPolicy) larger than this size will be excluded from compaction. The effect of raising hbase.hstore.compaction.max.size is fewer, larger StoreFiles that do not get compacted often. If you feel that compaction is happening too often without much benefit, you can try raising this value. Default: the value of LONG.MAX_VALUE, expressed in bytes.

Default
9223372036854775807

hbase.hstore.compaction.ratio
Description
For minor compaction, this ratio is used to determine whether a given StoreFile which is larger than hbase.hstore.compaction.min.size is eligible for compaction. Its effect is to limit compaction of large StoreFiles. The value of hbase.hstore.compaction.ratio is expressed as a floating-point decimal. A large ratio, such as 10, will produce a single giant StoreFile. Conversely, a low value, such as .25, will produce behavior similar to the BigTable compaction algorithm, producing four StoreFiles. A moderate value of between 1.0 and 1.4 is recommended. When tuning this value, you are balancing write costs with read costs. Raising the value (to something like 1.4) will have more write costs, because you will compact larger StoreFiles. However, during reads, HBase will need to seek through fewer StoreFiles to accomplish the read. Consider this approach if you cannot take advantage of Bloom filters. Otherwise, you can lower this value to something like 1.0 to reduce the background cost of writes, and use Bloom filters to control the number of StoreFiles touched during reads. For most cases, the default value is appropriate.

Default
1.2F

hbase.hstore.compaction.ratio.offpeak
Description
Allows you to set a different (by default, more aggressive) ratio for determining whether larger StoreFiles are included in compactions during off-peak hours. Works in the same way as hbase.hstore.compaction.ratio. Only applies if hbase.offpeak.start.hour and hbase.offpeak.end.hour are also enabled.

Default
5.0F

hbase.hstore.time.to.purge.deletes
Description
The amount of time to delay purging of delete markers with future timestamps. If unset, or set to 0, all delete markers, including those with future timestamps, are purged during the next major compaction. Otherwise, a delete marker is kept until the major compaction which occurs after the marker’s timestamp plus the value of this setting, in milliseconds.

Default
0

hbase.offpeak.start.hour
Description
The start of off-peak hours, expressed as an integer between 0 and 23, inclusive. Set to -1 to disable off-peak.

Default
-1

hbase.offpeak.end.hour
Description
The end of off-peak hours, expressed as an integer between 0 and 23, inclusive. Set to -1 to disable off-peak.

Default
-1

hbase.regionserver.thread.compaction.throttle
Description
There are two different thread pools for compactions, one for large compactions and the other for small compactions. This helps to keep compaction of lean tables (such as hbase:meta) fast. If a compaction is larger than this threshold, it goes into the large compaction pool. In most cases, the default value is appropriate. Default: 2 x hbase.hstore.compaction.max x hbase.hregion.memstore.flush.size (which defaults to 128MB). The value field assumes that the value of hbase.hregion.memstore.flush.size is unchanged from the default.

Default
2684354560

hbase.regionserver.majorcompaction.pagecache.drop
Description
Specifies whether to drop pages read/written into the system page cache by major compactions. Setting it to true helps prevent major compactions from polluting the page cache, which is almost always required, especially for clusters with low/moderate memory to storage ratio.

Default
true

hbase.regionserver.minorcompaction.pagecache.drop
Description
Specifies whether to drop pages read/written into the system page cache by minor compactions. Setting it to true helps prevent minor compactions from polluting the page cache, which is most beneficial on clusters with low memory to storage ratio or very write heavy clusters. You may want to set it to false under moderate to low write workload when bulk of the reads are on the most recently written data.

Default
true

hbase.hstore.compaction.kv.max
Description
The maximum number of KeyValues to read and then write in a batch when flushing or compacting. Set this lower if you have big KeyValues and problems with Out Of Memory Exceptions Set this higher if you have wide, small rows.

Default
10

hbase.storescanner.parallel.seek.enable
Description
Enables StoreFileScanner parallel-seeking in StoreScanner, a feature which can reduce response latency under special conditions.

Default
false

hbase.storescanner.parallel.seek.threads
Description
The default thread pool size if parallel-seeking feature enabled.

Default
10

hfile.block.cache.size
Description
Percentage of maximum heap (-Xmx setting) to allocate to block cache used by a StoreFile. Default of 0.4 means allocate 40%. Set to 0 to disable but it’s not recommended; you need at least enough cache to hold the storefile indices.

Default
0.4

hfile.block.index.cacheonwrite
Description
This allows to put non-root multi-level index blocks into the block cache at the time the index is being written.

Default
false

hfile.index.block.max.size
Description
When the size of a leaf-level, intermediate-level, or root-level index block in a multi-level block index grows to this size, the block is written out and a new block is started.

Default
131072

hbase.bucketcache.ioengine
Description
Where to store the contents of the bucketcache. One of: heap, offheap, or file. If a file, set it to file:PATH_TO_FILE. See http://hbase.apache.org/book.html#offheap.blockcache for more information.

Default
none

hbase.bucketcache.combinedcache.enabled
Description
Whether or not the bucketcache is used in league with the LRU on-heap block cache. In this mode, indices and blooms are kept in the LRU blockcache and the data blocks are kept in the bucketcache.

Default
true

hbase.bucketcache.size
Description
A float that EITHER represents a percentage of total heap memory size to give to the cache (if < 1.0) OR, it is the total capacity in megabytes of BucketCache. Default: 0.0

Default
none

hbase.bucketcache.bucket.sizes
Description
A comma-separated list of sizes for buckets for the bucketcache. Can be multiple sizes. List block sizes in order from smallest to largest. The sizes you use will depend on your data access patterns. Must be a multiple of 1024 else you will run into 'java.io.IOException: Invalid HFile block magic' when you go to read from cache. If you specify no values here, then you pick up the default bucketsizes set in code (See BucketAllocator#DEFAULT_BUCKET_SIZES).

Default
none

hfile.format.version
Description
The HFile format version to use for new files. Version 3 adds support for tags in hfiles (See http://hbase.apache.org/book.html#hbase.tags). Distributed Log Replay requires that tags are enabled. Also see the configuration 'hbase.replication.rpc.codec'.

Default
3

hfile.block.bloom.cacheonwrite
Description
Enables cache-on-write for inline blocks of a compound Bloom filter.

Default
false

io.storefile.bloom.block.size
Description
The size in bytes of a single block ("chunk") of a compound Bloom filter. This size is approximate, because Bloom blocks can only be inserted at data block boundaries, and the number of keys per data block varies.

Default
131072

hbase.rs.cacheblocksonwrite
Description
Whether an HFile block should be added to the block cache when the block is finished.

Default
false

hbase.rpc.timeout
Description
This is for the RPC layer to define how long (millisecond) HBase client applications take for a remote call to time out. It uses pings to check connections but will eventually throw a TimeoutException.

Default
60000

hbase.client.operation.timeout
Description
Operation timeout is a top-level restriction (millisecond) that makes sure a blocking operation in Table will not be blocked more than this. In each operation, if rpc request fails because of timeout or other reason, it will retry until success or throw RetriesExhaustedException. But if the total time being blocking reach the operation timeout before retries exhausted, it will break early and throw SocketTimeoutException.

Default
1200000

hbase.cells.scanned.per.heartbeat.check
Description
The number of cells scanned in between heartbeat checks. Heartbeat checks occur during the processing of scans to determine whether or not the server should stop scanning in order to send back a heartbeat message to the client. Heartbeat messages are used to keep the client-server connection alive during long running scans. Small values mean that the heartbeat checks will occur more often and thus will provide a tighter bound on the execution time of the scan. Larger values mean that the heartbeat checks occur less frequently

Default
10000

hbase.rpc.shortoperation.timeout
Description
This is another version of "hbase.rpc.timeout". For those RPC operation within cluster, we rely on this configuration to set a short timeout limitation for short operation. For example, short rpc timeout for region server’s trying to report to active master can benefit quicker master failover process.

Default
10000

hbase.ipc.client.tcpnodelay
Description
Set no delay on rpc socket connections. See http://docs.oracle.com/javase/1.5.0/docs/api/java/net/Socket.html#getTcpNoDelay()

Default
true

hbase.regionserver.hostname
Description
This config is for experts: don’t set its value unless you really know what you are doing. When set to a non-empty value, this represents the (external facing) hostname for the underlying server. See https://issues.apache.org/jira/browse/HBASE-12954 for details.

Default
none

hbase.master.keytab.file
Description
Full path to the kerberos keytab file to use for logging in the configured HMaster server principal.

Default
none

hbase.master.kerberos.principal
Description
Ex. "hbase/_HOST@EXAMPLE.COM". The kerberos principal name that should be used to run the HMaster process. The principal name should be in the form: user/hostname@DOMAIN. If "_HOST" is used as the hostname portion, it will be replaced with the actual hostname of the running instance.

Default
none

hbase.regionserver.keytab.file
Description
Full path to the kerberos keytab file to use for logging in the configured HRegionServer server principal.

Default
none

hbase.regionserver.kerberos.principal
Description
Ex. "hbase/_HOST@EXAMPLE.COM". The kerberos principal name that should be used to run the HRegionServer process. The principal name should be in the form: user/hostname@DOMAIN. If "_HOST" is used as the hostname portion, it will be replaced with the actual hostname of the running instance. An entry for this principal must exist in the file specified in hbase.regionserver.keytab.file

Default
none

hadoop.policy.file
Description
The policy configuration file used by RPC servers to make authorization decisions on client requests. Only used when HBase security is enabled.

Default
hbase-policy.xml

hbase.superuser
Description
List of users or groups (comma-separated), who are allowed full privileges, regardless of stored ACLs, across the cluster. Only used when HBase security is enabled.

Default
none

hbase.auth.key.update.interval
Description
The update interval for master key for authentication tokens in servers in milliseconds. Only used when HBase security is enabled.

Default
86400000

hbase.auth.token.max.lifetime
Description
The maximum lifetime in milliseconds after which an authentication token expires. Only used when HBase security is enabled.

Default
604800000

hbase.ipc.client.fallback-to-simple-auth-allowed
Description
When a client is configured to attempt a secure connection, but attempts to connect to an insecure server, that server may instruct the client to switch to SASL SIMPLE (unsecure) authentication. This setting controls whether or not the client will accept this instruction from the server. When false (the default), the client will not allow the fallback to SIMPLE authentication, and will abort the connection.

Default
false

hbase.ipc.server.fallback-to-simple-auth-allowed
Description
When a server is configured to require secure connections, it will reject connection attempts from clients using SASL SIMPLE (unsecure) authentication. This setting allows secure servers to accept SASL SIMPLE connections from clients when the client requests. When false (the default), the server will not allow the fallback to SIMPLE authentication, and will reject the connection. WARNING: This setting should ONLY be used as a temporary measure while converting clients over to secure authentication. It MUST BE DISABLED for secure operation.

Default
false

hbase.display.keys
Description
When this is set to true the webUI and such will display all start/end keys as part of the table details, region names, etc. When this is set to false, the keys are hidden.

Default
true

hbase.coprocessor.enabled
Description
Enables or disables coprocessor loading. If 'false' (disabled), any other coprocessor related configuration will be ignored.

Default
true

hbase.coprocessor.user.enabled
Description
Enables or disables user (aka. table) coprocessor loading. If 'false' (disabled), any table coprocessor attributes in table descriptors will be ignored. If "hbase.coprocessor.enabled" is 'false' this setting has no effect.

Default
true

hbase.coprocessor.region.classes
Description
A comma-separated list of Coprocessors that are loaded by default on all tables. For any override coprocessor method, these classes will be called in order. After implementing your own Coprocessor, just put it in HBase’s classpath and add the fully qualified class name here. A coprocessor can also be loaded on demand by setting HTableDescriptor.

Default
none

hbase.rest.port
Description
The port for the HBase REST server.

Default
8080

hbase.rest.readonly
Description
Defines the mode the REST server will be started in. Possible values are: false: All HTTP methods are permitted - GET/PUT/POST/DELETE. true: Only the GET method is permitted.

Default
false

hbase.rest.threads.max
Description
The maximum number of threads of the REST server thread pool. Threads in the pool are reused to process REST requests. This controls the maximum number of requests processed concurrently. It may help to control the memory used by the REST server to avoid OOM issues. If the thread pool is full, incoming requests will be queued up and wait for some free threads.

Default
100

hbase.rest.threads.min
Description
The minimum number of threads of the REST server thread pool. The thread pool always has at least these number of threads so the REST server is ready to serve incoming requests.

Default
2

hbase.rest.support.proxyuser
Description
Enables running the REST server to support proxy-user mode.

Default
false

hbase.defaults.for.version.skip
Description
Set to true to skip the 'hbase.defaults.for.version' check. Setting this to true can be useful in contexts other than the other side of a maven generation; i.e. running in an IDE. You’ll want to set this boolean to true to avoid seeing the RuntimeException complaint: "hbase-default.xml file seems to be for and old version of HBase (\${hbase.version}), this version is X.X.X-SNAPSHOT"

Default
false

hbase.coprocessor.master.classes
Description
A comma-separated list of org.apache.hadoop.hbase.coprocessor.MasterObserver coprocessors that are loaded by default on the active HMaster process. For any implemented coprocessor methods, the listed classes will be called in order. After implementing your own MasterObserver, just put it in HBase’s classpath and add the fully qualified class name here.

Default
none

hbase.coprocessor.abortonerror
Description
Set to true to cause the hosting server (master or regionserver) to abort if a coprocessor fails to load, fails to initialize, or throws an unexpected Throwable object. Setting this to false will allow the server to continue execution but the system wide state of the coprocessor in question will become inconsistent as it will be properly executing in only a subset of servers, so this is most useful for debugging only.

Default
true

hbase.table.lock.enable
Description
Set to true to enable locking the table in zookeeper for schema change operations. Table locking from master prevents concurrent schema modifications to corrupt table state.

Default
true

hbase.table.max.rowsize
Description
Maximum size of single row in bytes (default is 1 Gb) for Get’ting or Scan’ning without in-row scan flag set. If row size exceeds this limit RowTooBigException is thrown to client.

Default
1073741824

hbase.thrift.minWorkerThreads
Description
The "core size" of the thread pool. New threads are created on every connection until this many threads are created.

Default
16

hbase.thrift.maxWorkerThreads
Description
The maximum size of the thread pool. When the pending request queue overflows, new threads are created until their number reaches this number. After that, the server starts dropping connections.

Default
1000

hbase.thrift.maxQueuedRequests
Description
The maximum number of pending Thrift connections waiting in the queue. If there are no idle threads in the pool, the server queues requests. Only when the queue overflows, new threads are added, up to hbase.thrift.maxQueuedRequests threads.

Default
1000

hbase.regionserver.thrift.framed
Description
Use Thrift TFramedTransport on the server side. This is the recommended transport for thrift servers and requires a similar setting on the client side. Changing this to false will select the default transport, vulnerable to DoS when malformed requests are issued due to THRIFT-601.

Default
false

hbase.regionserver.thrift.framed.max_frame_size_in_mb
Description
Default frame size when using framed transport, in MB

Default
2

hbase.regionserver.thrift.compact
Description
Use Thrift TCompactProtocol binary serialization protocol.

Default
false

hbase.rootdir.perms
Description
FS Permissions for the root data subdirectory in a secure (kerberos) setup. When master starts, it creates the rootdir with this permissions or sets the permissions if it does not match.

Default
700

hbase.wal.dir.perms
Description
FS Permissions for the root WAL directory in a secure(kerberos) setup. When master starts, it creates the WAL dir with this permissions or sets the permissions if it does not match.

Default
700

hbase.data.umask.enable
Description
Enable, if true, that file permissions should be assigned to the files written by the regionserver

Default
false

hbase.data.umask
Description
File permissions that should be used to write data files when hbase.data.umask.enable is true

Default
000

hbase.snapshot.enabled
Description
Set to true to allow snapshots to be taken / restored / cloned.

Default
true

hbase.snapshot.restore.take.failsafe.snapshot
Description
Set to true to take a snapshot before the restore operation. The snapshot taken will be used in case of failure, to restore the previous state. At the end of the restore operation this snapshot will be deleted

Default
true

hbase.snapshot.restore.failsafe.name
Description
Name of the failsafe snapshot taken by the restore operation. You can use the {snapshot.name}, {table.name} and {restore.timestamp} variables to create a name based on what you are restoring.

Default
hbase-failsafe-{snapshot.name}-{restore.timestamp}

hbase.server.compactchecker.interval.multiplier
Description
The number that determines how often we scan to see if compaction is necessary. Normally, compactions are done after some events (such as memstore flush), but if region didn’t receive a lot of writes for some time, or due to different compaction policies, it may be necessary to check it periodically. The interval between checks is hbase.server.compactchecker.interval.multiplier multiplied by hbase.server.thread.wakefrequency.

Default
1000

hbase.lease.recovery.timeout
Description
How long we wait on dfs lease recovery in total before giving up.

Default
900000

hbase.lease.recovery.dfs.timeout
Description
How long between dfs recover lease invocations. Should be larger than the sum of the time it takes for the namenode to issue a block recovery command as part of datanode; dfs.heartbeat.interval and the time it takes for the primary datanode, performing block recovery to timeout on a dead datanode; usually dfs.client.socket-timeout. See the end of HBASE-8389 for more.

Default
64000

hbase.column.max.version
Description
New column family descriptors will use this value as the default number of versions to keep.

Default
1

dfs.client.read.shortcircuit
Description
If set to true, this configuration parameter enables short-circuit local reads.

Default
false

dfs.domain.socket.path
Description
This is a path to a UNIX domain socket that will be used for communication between the DataNode and local HDFS clients, if dfs.client.read.shortcircuit is set to true. If the string "_PORT" is present in this path, it will be replaced by the TCP port of the DataNode. Be careful about permissions for the directory that hosts the shared domain socket; dfsclient will complain if open to other users than the HBase user.

Default
none

hbase.dfs.client.read.shortcircuit.buffer.size
Description
If the DFSClient configuration dfs.client.read.shortcircuit.buffer.size is unset, we will use what is configured here as the short circuit read default direct byte buffer size. DFSClient native default is 1MB; HBase keeps its HDFS files open so number of file blocks * 1MB soon starts to add up and threaten OOME because of a shortage of direct memory. So, we set it down from the default. Make it > the default hbase block size set in the HColumnDescriptor which is usually 64k.

Default
131072

hbase.regionserver.checksum.verify
Description
If set to true (the default), HBase verifies the checksums for hfile blocks. HBase writes checksums inline with the data when it writes out hfiles. HDFS (as of this writing) writes checksums to a separate file than the data file necessitating extra seeks. Setting this flag saves some on i/o. Checksum verification by HDFS will be internally disabled on hfile streams when this flag is set. If the hbase-checksum verification fails, we will switch back to using HDFS checksums (so do not disable HDFS checksums! And besides this feature applies to hfiles only, not to WALs). If this parameter is set to false, then hbase will not verify any checksums, instead it will depend on checksum verification being done in the HDFS client.

Default
true

hbase.hstore.bytes.per.checksum
Description
Number of bytes in a newly created checksum chunk for HBase-level checksums in hfile blocks.

Default
16384

hbase.hstore.checksum.algorithm
Description
Name of an algorithm that is used to compute checksums. Possible values are NULL, CRC32, CRC32C.

Default
CRC32C

hbase.client.scanner.max.result.size
Description
Maximum number of bytes returned when calling a scanner’s next method. Note that when a single row is larger than this limit the row is still returned completely. The default value is 2MB, which is good for 1ge networks. With faster and/or high latency networks this value should be increased.

Default
2097152

hbase.server.scanner.max.result.size
Description
Maximum number of bytes returned when calling a scanner’s next method. Note that when a single row is larger than this limit the row is still returned completely. The default value is 100MB. This is a safety setting to protect the server from OOM situations.

Default
104857600

hbase.status.published
Description
This setting activates the publication by the master of the status of the region server. When a region server dies and its recovery starts, the master will push this information to the client application, to let them cut the connection immediately instead of waiting for a timeout.

Default
false

hbase.status.publisher.class
Description
Implementation of the status publication with a multicast message.

Default
org.apache.hadoop.hbase.master.ClusterStatusPublisher$MulticastPublisher

hbase.status.listener.class
Description
Implementation of the status listener with a multicast message.

Default
org.apache.hadoop.hbase.client.ClusterStatusListener$MulticastListener

hbase.status.multicast.address.ip
Description
Multicast address to use for the status publication by multicast.

Default
226.1.1.3

hbase.status.multicast.address.port
Description
Multicast port to use for the status publication by multicast.

Default
16100

hbase.dynamic.jars.dir
Description
The directory from which the custom filter JARs can be loaded dynamically by the region server without the need to restart. However, an already loaded filter/co-processor class would not be un-loaded. See HBASE-1936 for more details. Does not apply to coprocessors.

Default
${hbase.rootdir}/lib

hbase.security.authentication
Description
Controls whether or not secure authentication is enabled for HBase. Possible values are 'simple' (no authentication), and 'kerberos'.

Default
simple

hbase.rest.filter.classes
Description
Servlet filters for REST service.

Default
org.apache.hadoop.hbase.rest.filter.GzipFilter

hbase.master.loadbalancer.class
Description
Class used to execute the regions balancing when the period occurs. See the class comment for more on how it works http://hbase.apache.org/devapidocs/org/apache/hadoop/hbase/master/balancer/StochasticLoadBalancer.html It replaces the DefaultLoadBalancer as the default (since renamed as the SimpleLoadBalancer).

Default
org.apache.hadoop.hbase.master.balancer.StochasticLoadBalancer

hbase.master.normalizer.class
Description
Class used to execute the region normalization when the period occurs. See the class comment for more on how it works http://hbase.apache.org/devapidocs/org/apache/hadoop/hbase/master/normalizer/SimpleRegionNormalizer.html

Default
org.apache.hadoop.hbase.master.normalizer.SimpleRegionNormalizer

hbase.rest.csrf.enabled
Description
Set to true to enable protection against cross-site request forgery (CSRF)

Default
false

hbase.rest-csrf.browser-useragents-regex
Description
A comma-separated list of regular expressions used to match against an HTTP request’s User-Agent header when protection against cross-site request forgery (CSRF) is enabled for REST server by setting hbase.rest.csrf.enabled to true. If the incoming User-Agent matches any of these regular expressions, then the request is considered to be sent by a browser, and therefore CSRF prevention is enforced. If the request’s User-Agent does not match any of these regular expressions, then the request is considered to be sent by something other than a browser, such as scripted automation. In this case, CSRF is not a potential attack vector, so the prevention is not enforced. This helps achieve backwards-compatibility with existing automation that has not been updated to send the CSRF prevention header.

Default
Mozilla.,Opera.

hbase.security.exec.permission.checks
Description
If this setting is enabled and ACL based access control is active (the AccessController coprocessor is installed either as a system coprocessor or on a table as a table coprocessor) then you must grant all relevant users EXEC privilege if they require the ability to execute coprocessor endpoint calls. EXEC privilege, like any other permission, can be granted globally to a user, or to a user on a per table or per namespace basis. For more information on coprocessor endpoints, see the coprocessor section of the HBase online manual. For more information on granting or revoking permissions using the AccessController, see the security section of the HBase online manual.

Default
false

hbase.procedure.regionserver.classes
Description
A comma-separated list of org.apache.hadoop.hbase.procedure.RegionServerProcedureManager procedure managers that are loaded by default on the active HRegionServer process. The lifecycle methods (init/start/stop) will be called by the active HRegionServer process to perform the specific globally barriered procedure. After implementing your own RegionServerProcedureManager, just put it in HBase’s classpath and add the fully qualified class name here.

Default
none

hbase.procedure.master.classes
Description
A comma-separated list of org.apache.hadoop.hbase.procedure.MasterProcedureManager procedure managers that are loaded by default on the active HMaster process. A procedure is identified by its signature and users can use the signature and an instant name to trigger an execution of a globally barriered procedure. After implementing your own MasterProcedureManager, just put it in HBase’s classpath and add the fully qualified class name here.

Default
none

hbase.coordinated.state.manager.class
Description
Fully qualified name of class implementing coordinated state manager.

Default
org.apache.hadoop.hbase.coordination.ZkCoordinatedStateManager

hbase.regionserver.storefile.refresh.period
Description
The period (in milliseconds) for refreshing the store files for the secondary regions. 0 means this feature is disabled. Secondary regions sees new files (from flushes and compactions) from primary once the secondary region refreshes the list of files in the region (there is no notification mechanism). But too frequent refreshes might cause extra Namenode pressure. If the files cannot be refreshed for longer than HFile TTL (hbase.master.hfilecleaner.ttl) the requests are rejected. Configuring HFile TTL to a larger value is also recommended with this setting.

Default
0

hbase.region.replica.replication.enabled
Description
Whether asynchronous WAL replication to the secondary region replicas is enabled or not. If this is enabled, a replication peer named "region_replica_replication" will be created which will tail the logs and replicate the mutations to region replicas for tables that have region replication > 1. If this is enabled once, disabling this replication also requires disabling the replication peer using shell or ReplicationAdmin java class. Replication to secondary region replicas works over standard inter-cluster replication.

Default
false

hbase.http.filter.initializers
Description
A comma separated list of class names. Each class in the list must extend org.apache.hadoop.hbase.http.FilterInitializer. The corresponding Filter will be initialized. Then, the Filter will be applied to all user facing jsp and servlet web pages. The ordering of the list defines the ordering of the filters. The default StaticUserWebFilter add a user principal as defined by the hbase.http.staticuser.user property.

Default
org.apache.hadoop.hbase.http.lib.StaticUserWebFilter

hbase.security.visibility.mutations.checkauths
Description
This property if enabled, will check whether the labels in the visibility expression are associated with the user issuing the mutation

Default
false

hbase.http.max.threads
Description
The maximum number of threads that the HTTP Server will create in its ThreadPool.

Default
10

hbase.replication.rpc.codec
Description
The codec that is to be used when replication is enabled so that the tags are also replicated. This is used along with HFileV3 which supports tags in them. If tags are not used or if the hfile version used is HFileV2 then KeyValueCodec can be used as the replication codec. Note that using KeyValueCodecWithTags for replication when there are no tags causes no harm.

Default
org.apache.hadoop.hbase.codec.KeyValueCodecWithTags

hbase.replication.source.maxthreads
Description
The maximum number of threads any replication source will use for shipping edits to the sinks in parallel. This also limits the number of chunks each replication batch is broken into. Larger values can improve the replication throughput between the master and slave clusters. The default of 10 will rarely need to be changed.

Default
10

hbase.serial.replication.waitingMs
Description
By default, in replication we can not make sure the order of operations in slave cluster is same as the order in master. If set REPLICATION_SCOPE to 2, we will push edits by the order of written. This configure is to set how long (in ms) we will wait before next checking if a log can not push right now because there are some logs written before it have not been pushed. A larger waiting will decrease the number of queries on hbase:meta but will enlarge the delay of replication. This feature relies on zk-less assignment, and conflicts with distributed log replay. So users must set hbase.assignment.usezk and hbase.master.distributed.log.replay to false to support it.

Default
10000

hbase.http.staticuser.user
Description
The user name to filter as, on static web filters while rendering content. An example use is the HDFS web UI (user to be used for browsing files).

Default
dr.stack

hbase.regionserver.handler.abort.on.error.percent
Description
The percent of region server RPC threads failed to abort RS. -1 Disable aborting; 0 Abort if even a single handler has died; 0.x Abort only when this percent of handlers have died; 1 Abort only all of the handers have died.

Default
0.5

hbase.mob.file.cache.size
Description
Number of opened file handlers to cache. A larger value will benefit reads by providing more file handlers per mob file cache and would reduce frequent file opening and closing. However, if this is set too high, this could lead to a "too many opened file handlers" The default value is 1000.

Default
1000

hbase.mob.cache.evict.period
Description
The amount of time in seconds before the mob cache evicts cached mob files. The default value is 3600 seconds.

Default
3600

hbase.mob.cache.evict.remain.ratio
Description
The ratio (between 0.0 and 1.0) of files that remains cached after an eviction is triggered when the number of cached mob files exceeds the hbase.mob.file.cache.size. The default value is 0.5f.

Default
0.5f

hbase.master.mob.ttl.cleaner.period
Description
The period that ExpiredMobFileCleanerChore runs. The unit is second. The default value is one day. The MOB file name uses only the date part of the file creation time in it. We use this time for deciding TTL expiry of the files. So the removal of TTL expired files might be delayed. The max delay might be 24 hrs.

Default
86400

hbase.mob.compaction.mergeable.threshold
Description
If the size of a mob file is less than this value, it’s regarded as a small file and needs to be merged in mob compaction. The default value is 1280MB.

Default
1342177280

hbase.mob.delfile.max.count
Description
The max number of del files that is allowed in the mob compaction. In the mob compaction, when the number of existing del files is larger than this value, they are merged until number of del files is not larger this value. The default value is 3.

Default
3

hbase.mob.compaction.batch.size
Description
The max number of the mob files that is allowed in a batch of the mob compaction. The mob compaction merges the small mob files to bigger ones. If the number of the small files is very large, it could lead to a "too many opened file handlers" in the merge. And the merge has to be split into batches. This value limits the number of mob files that are selected in a batch of the mob compaction. The default value is 100.

Default
100

hbase.mob.compaction.chore.period
Description
The period that MobCompactionChore runs. The unit is second. The default value is one week.

Default
604800

hbase.mob.compactor.class
Description
Implementation of mob compactor, the default one is PartitionedMobCompactor.

Default
org.apache.hadoop.hbase.mob.compactions.PartitionedMobCompactor

hbase.mob.compaction.threads.max
Description
The max number of threads used in MobCompactor.

Default
1

hbase.snapshot.master.timeout.millis
Description
Timeout for master for the snapshot procedure execution

Default
300000

hbase.snapshot.region.timeout
Description
Timeout for regionservers to keep threads in snapshot request pool waiting

Default
300000

7.3. hbase-env.sh
Set HBase environment variables in this file. Examples include options to pass the JVM on start of an HBase daemon such as heap size and garbage collector configs. You can also set configurations for HBase configuration, log directories, niceness, ssh options, where to locate process pid files, etc. Open the file at conf/hbase-env.sh and peruse its content. Each option is fairly well documented. Add your own environment variables here if you want them read by HBase daemons on startup.

Changes here will require a cluster restart for HBase to notice the change.

7.4. log4j.properties
Edit this file to change rate at which HBase files are rolled and to change the level at which HBase logs messages.

Changes here will require a cluster restart for HBase to notice the change though log levels can be changed for particular daemons via the HBase UI.

7.5. Client configuration and dependencies connecting to an HBase cluster
If you are running HBase in standalone mode, you don’t need to configure anything for your client to work provided that they are all on the same machine.

Since the HBase Master may move around, clients bootstrap by looking to ZooKeeper for current critical locations. ZooKeeper is where all these values are kept. Thus clients require the location of the ZooKeeper ensemble before they can do anything else. Usually this the ensemble location is kept out in the hbase-site.xml and is picked up by the client from the CLASSPATH.

If you are configuring an IDE to run an HBase client, you should include the conf/ directory on your classpath so hbase-site.xml settings can be found (or add src/test/resources to pick up the hbase-site.xml used by tests).

Minimally, a client of HBase needs several libraries in its CLASSPATH when connecting to a cluster, including:

commons-configuration (commons-configuration-1.6.jar)
commons-lang (commons-lang-2.5.jar)
commons-logging (commons-logging-1.1.1.jar)
hadoop-core (hadoop-core-1.0.0.jar)
hbase (hbase-0.92.0.jar)
log4j (log4j-1.2.16.jar)
slf4j-api (slf4j-api-1.5.8.jar)
slf4j-log4j (slf4j-log4j12-1.5.8.jar)
zookeeper (zookeeper-3.4.2.jar)
An example basic hbase-site.xml for client only might look as follows:

<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>example1,example2,example3</value>
    <description>The directory shared by region servers.
    </description>
  </property>
</configuration>
7.5.1. Java client configuration
The configuration used by a Java client is kept in an HBaseConfiguration instance.

The factory method on HBaseConfiguration, HBaseConfiguration.create();, on invocation, will read in the content of the first hbase-site.xml found on the client’s CLASSPATH, if one is present (Invocation will also factor in any hbase-default.xml found; an hbase-default.xml ships inside the hbase.X.X.X.jar). It is also possible to specify configuration directly without having to read from a hbase-site.xml. For example, to set the ZooKeeper ensemble for the cluster programmatically do as follows:

Configuration config = HBaseConfiguration.create();
config.set("hbase.zookeeper.quorum", "localhost");  // Here we are running zookeeper locally
If multiple ZooKeeper instances make up your ZooKeeper ensemble, they may be specified in a comma-separated list (just as in the hbase-site.xml file). This populated Configuration instance can then be passed to an Table, and so on.