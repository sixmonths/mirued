6. Running and Confirming Your Installation
Make sure HDFS is running first. Start and stop the Hadoop HDFS daemons by running bin/start-hdfs.sh over in the HADOOP_HOME directory. You can ensure it started properly by testing the put and get of files into the Hadoop filesystem. HBase does not normally use the MapReduce or YARN daemons. These do not need to be started.

If you are managing your own ZooKeeper, start it and confirm it’s running, else HBase will start up ZooKeeper for you as part of its start process.

Start HBase with the following command:

bin/start-hbase.sh
Run the above from the HBASE_HOME directory.

You should now have a running HBase instance. HBase logs can be found in the logs subdirectory. Check them out especially if HBase had trouble starting.

HBase also puts up a UI listing vital attributes. By default it’s deployed on the Master host at port 16010 (HBase RegionServers listen on port 16020 by default and put up an informational HTTP server at port 16030). If the Master is running on a host named master.example.org on the default port, point your browser at http://master.example.org:16010 to see the web interface.

Once HBase has started, see the shell exercises section for how to create tables, add data, scan your insertions, and finally disable and drop your tables.

To stop HBase after exiting the HBase shell enter

$ ./bin/stop-hbase.sh
stopping hbase...............
Shutdown can take a moment to complete. It can take longer if your cluster is comprised of many machines. If you are running a distributed operation, be sure to wait until HBase has shut down completely before stopping the Hadoop daemons.