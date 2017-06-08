10. Dynamic Configuration
Since HBase 1.0.0, it is possible to change a subset of the configuration without requiring a server restart. In the HBase shell, there are new operators, update_config and update_all_config that will prompt a server or all servers to reload configuration.

Only a subset of all configurations can currently be changed in the running server. Here is an incomplete list: hbase.regionserver.thread.compaction.large, hbase.regionserver.thread.compaction.small, hbase.regionserver.thread.split, hbase.regionserver.thread.merge, as well as compaction policy and configurations and adjustment to offpeak hours. For the full list consult the patch attached to HBASE-12147 Porting Online Config Change from 89-fb.

