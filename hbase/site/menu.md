 

## [Preface](./001_preface)
 
 

## [Getting Started](./002_getting_started)
 
#### [1. Introduction](./003_1_introduction)
#### [2. Quick Start - Standalone HBase](./004_2_quick_start_standalone_hbase)
 

## [Apache HBase Configuration](./005_apache_hbase_configuration)
 
#### [3. Configuration Files](./006_3_configuration_files)
#### [4. Basic Prerequisites](./007_4_basic_prerequisites)
#### [5. HBase run modes: Standalone and Distributed](./008_5_hbase_run_modes_standalone_and_distributed)
#### [6. Running and Confirming Your Installation](./009_6_running_and_confirming_your_installation)
#### [7. Default Configuration](./010_7_default_configuration)
#### [8. Example Configurations](./011_8_example_configurations)
#### [9. The Important Configurations](./012_9_the_important_configurations)
#### [10. Dynamic Configuration](./013_10_dynamic_configuration)
 

## [Upgrading](./014_upgrading)
 
#### [11. HBase version number and compatibility](./015_11_hbase_version_number_and_compatibility)
#### [12. Upgrade Paths](./016_12_upgrade_paths)
 

## [The Apache HBase Shell](./017_the_apache_hbase_shell)
 
#### [13. Scripting with Ruby](./018_13_scripting_with_ruby)
#### [14. Running the Shell in Non-Interactive Mode](./019_14_running_the_shell_in_non_interactive_mode)
#### [15. HBase Shell in OS Scripts](./020_15_hbase_shell_in_os_scripts)
#### [16. Read HBase Shell Commands from a Command File](./021_16_read_hbase_shell_commands_from_a_command_file)
#### [17. Passing VM Options to the Shell](./022_17_passing_vm_options_to_the_shell)
#### [18. Shell Tricks](./023_18_shell_tricks)
 

## [Data Model](./024_data_model)
 
#### [19. Conceptual View](./025_19_conceptual_view)
#### [20. Physical View](./026_20_physical_view)
#### [21. Namespace](./027_21_namespace)
#### [22. Table](./028_22_table)
#### [23. Row](./029_23_row)
#### [24. Column Family](./030_24_column_family)
#### [25. Cells](./031_25_cells)
#### [26. Data Model Operations](./032_26_data_model_operations)
#### [27. Versions](./033_27_versions)
#### [28. Sort Order](./034_28_sort_order)
#### [29. Column Metadata](./035_29_column_metadata)
#### [30. Joins](./036_30_joins)
#### [31. ACID](./037_31_acid)
 

## [HBase and Schema Design](./038_hbase_and_schema_design)
 
#### [32. Schema Creation](./039_32_schema_creation)
#### [33. Table Schema Rules Of Thumb](./040_33_table_schema_rules_of_thumb)
 

## [RegionServer Sizing Rules of Thumb](./041_regionserver_sizing_rules_of_thumb)
 
#### [34. On the number of column families](./042_34_on_the_number_of_column_families)
#### [35. Rowkey Design](./043_35_rowkey_design)
#### [36. Number of Versions](./044_36_number_of_versions)
#### [37. Supported Datatypes](./045_37_supported_datatypes)
#### [38. Joins](./046_38_joins)
#### [39. Time To Live (TTL)](./047_39_time_to_live_ttl)
#### [40. Keeping Deleted Cells](./048_40_keeping_deleted_cells)
#### [41. Secondary Indexes and Alternate Query Paths](./049_41_secondary_indexes_and_alternate_query_paths)
#### [42. Constraints](./050_42_constraints)
#### [43. Schema Design Case Studies](./051_43_schema_design_case_studies)
#### [44. Operational and Performance Configuration Options](./052_44_operational_and_performance_configuration_options)
#### [45. Special Cases](./053_45_special_cases)
 

## [HBase and MapReduce](./054_hbase_and_mapreduce)
 
#### [46. HBase, MapReduce, and the CLASSPATH](./055_46_hbase,_mapreduce,_and_the_classpath)
#### [47. MapReduce Scan Caching](./056_47_mapreduce_scan_caching)
#### [48. Bundled HBase MapReduce Jobs](./057_48_bundled_hbase_mapreduce_jobs)
#### [49. HBase as a MapReduce Job Data Source and Data Sink](./058_49_hbase_as_a_mapreduce_job_data_source_and_data_sink)
#### [50. Writing HFiles Directly During Bulk Import](./059_50_writing_hfiles_directly_during_bulk_import)
#### [51. RowCounter Example](./060_51_rowcounter_example)
#### [52. Map-Task Splitting](./061_52_map_task_splitting)
#### [53. HBase MapReduce Examples](./062_53_hbase_mapreduce_examples)
#### [54. Accessing Other HBase Tables in a MapReduce Job](./063_54_accessing_other_hbase_tables_in_a_mapreduce_job)
#### [55. Speculative Execution](./064_55_speculative_execution)
#### [56. Cascading](./065_56_cascading)
 

## [Securing Apache HBase](./066_securing_apache_hbase)
 
#### [57. Using Secure HTTP (HTTPS) for the Web UI](./067_57_using_secure_http_https_for_the_web_ui)
#### [58. Using SPNEGO for Kerberos authentication with Web UIs](./068_58_using_spnego_for_kerberos_authentication_with_web_uis)
#### [59. Secure Client Access to Apache HBase](./069_59_secure_client_access_to_apache_hbase)
#### [60. Simple User Access to Apache HBase](./070_60_simple_user_access_to_apache_hbase)
#### [61. Securing Access to HDFS and ZooKeeper](./071_61_securing_access_to_hdfs_and_zookeeper)
#### [62. Securing Access To Your Data](./072_62_securing_access_to_your_data)
#### [63. Security Configuration Example](./073_63_security_configuration_example)
 

## [Architecture](./074_architecture)
 
#### [64. Overview](./075_64_overview)
#### [65. Catalog Tables](./076_65_catalog_tables)
#### [66. Client](./077_66_client)
#### [67. Client Request Filters](./078_67_client_request_filters)
#### [68. Master](./079_68_master)
#### [69. RegionServer](./080_69_regionserver)
#### [70. Regions](./081_70_regions)
#### [71. Bulk Loading](./082_71_bulk_loading)
#### [72. HDFS](./083_72_hdfs)
#### [73. Timeline-consistent High Available Reads](./084_73_timeline_consistent_high_available_reads)
#### [74. Storing Medium-sized Objects (MOB)](./085_74_storing_medium_sized_objects_mob)
 

## [Apache HBase APIs](./086_apache_hbase_apis)
 
#### [75. Examples](./087_75_examples)
 

## [Apache HBase External APIs](./088_apache_hbase_external_apis)
 
#### [76. REST](./089_76_rest)
#### [77. Thrift](./090_77_thrift)
#### [78. C/C++ Apache HBase Client](./091_78_c_c++_apache_hbase_client)
#### [79. Using Java Data Objects (JDO) with HBase](./092_79_using_java_data_objects_jdo_with_hbase)
#### [80. Scala](./093_80_scala)
#### [81. Jython](./094_81_jython)
 

## [Thrift API and Filter Language](./095_thrift_api_and_filter_language)
 
#### [82. Filter Language](./096_82_filter_language)
 

## [HBase and Spark](./097_hbase_and_spark)
 
#### [83. Basic Spark](./098_83_basic_spark)
#### [84. Spark Streaming](./099_84_spark_streaming)
#### [85. Bulk Load](./100_85_bulk_load)
#### [86. SparkSQL/DataFrames](./101_86_sparksql_dataframes)
 

## [Apache HBase Coprocessors](./102_apache_hbase_coprocessors)
 
#### [87. Coprocessor Overview](./103_87_coprocessor_overview)
#### [88. Types of Coprocessors](./104_88_types_of_coprocessors)
#### [89. Loading Coprocessors](./105_89_loading_coprocessors)
#### [90. Examples](./106_90_examples)
#### [91. Guidelines For Deploying A Coprocessor](./107_91_guidelines_for_deploying_a_coprocessor)
#### [92. Restricting Coprocessor Usage](./108_92_restricting_coprocessor_usage)
 

## [Apache HBase Performance Tuning](./109_apache_hbase_performance_tuning)
 
#### [93. Operating System](./110_93_operating_system)
#### [94. Network](./111_94_network)
#### [95. Java](./112_95_java)
#### [96. HBase Configurations](./113_96_hbase_configurations)
#### [97. ZooKeeper](./114_97_zookeeper)
#### [98. Schema Design](./115_98_schema_design)
#### [99. HBase General Patterns](./116_99_hbase_general_patterns)
#### [100. Writing to HBase](./117_100_writing_to_hbase)
#### [101. Reading from HBase](./118_101_reading_from_hbase)
#### [102. Deleting from HBase](./119_102_deleting_from_hbase)
#### [103. HDFS](./120_103_hdfs)
#### [104. Amazon EC2](./121_104_amazon_ec2)
#### [105. Collocating HBase and MapReduce](./122_105_collocating_hbase_and_mapreduce)
#### [106. Case Studies](./123_106_case_studies)
 

## [Troubleshooting and Debugging Apache HBase](./124_troubleshooting_and_debugging_apache_hbase)
 
#### [107. General Guidelines](./125_107_general_guidelines)
#### [108. Logs](./126_108_logs)
#### [109. Resources](./127_109_resources)
#### [110. Tools](./128_110_tools)
#### [111. Client](./129_111_client)
#### [112. MapReduce](./130_112_mapreduce)
#### [113. NameNode](./131_113_namenode)
#### [114. Network](./132_114_network)
#### [115. RegionServer](./133_115_regionserver)
#### [116. Master](./134_116_master)
#### [117. ZooKeeper](./135_117_zookeeper)
#### [118. Amazon EC2](./136_118_amazon_ec2)
#### [119. HBase and Hadoop version issues](./137_119_hbase_and_hadoop_version_issues)
#### [120. IPC Configuration Conflicts with Hadoop](./138_120_ipc_configuration_conflicts_with_hadoop)
#### [121. HBase and HDFS](./139_121_hbase_and_hdfs)
#### [122. Running unit or integration tests](./140_122_running_unit_or_integration_tests)
#### [123. Case Studies](./141_123_case_studies)
#### [124. Cryptographic Features](./142_124_cryptographic_features)
#### [125. Operating System Specific Issues](./143_125_operating_system_specific_issues)
#### [126. JDK Issues](./144_126_jdk_issues)
 

## [Apache HBase Case Studies](./145_apache_hbase_case_studies)
 
#### [127. Overview](./146_127_overview)
#### [128. Schema Design](./147_128_schema_design)
#### [129. Performance/Troubleshooting](./148_129_performance_troubleshooting)
 

## [Apache HBase Operational Management](./149_apache_hbase_operational_management)
 
#### [130. HBase Tools and Utilities](./150_130_hbase_tools_and_utilities)
#### [131. Region Management](./151_131_region_management)
#### [132. Node Management](./152_132_node_management)
#### [133. HBase Metrics](./153_133_hbase_metrics)
#### [134. HBase Monitoring](./154_134_hbase_monitoring)
#### [135. Cluster Replication](./155_135_cluster_replication)
#### [136. Running Multiple Workloads On a Single Cluster](./156_136_running_multiple_workloads_on_a_single_cluster)
#### [137. HBase Backup](./157_137_hbase_backup)
#### [138. HBase Snapshots](./158_138_hbase_snapshots)
#### [139. Storing Snapshots in Microsoft Azure Blob Storage](./159_139_storing_snapshots_in_microsoft_azure_blob_storage)
#### [140. Capacity Planning and Region Sizing](./160_140_capacity_planning_and_region_sizing)
#### [141. Table Rename](./161_141_table_rename)
#### [142. RegionServer Grouping](./162_142_regionserver_grouping)
 

## [Building and Developing Apache HBase](./163_building_and_developing_apache_hbase)
 
#### [143. Getting Involved](./164_143_getting_involved)
#### [144. Apache HBase Repositories](./165_144_apache_hbase_repositories)
#### [145. IDEs](./166_145_ides)
#### [146. Building Apache HBase](./167_146_building_apache_hbase)
#### [147. Releasing Apache HBase](./168_147_releasing_apache_hbase)
#### [148. Voting on Release Candidates](./169_148_voting_on_release_candidates)
#### [149. Generating the HBase Reference Guide](./170_149_generating_the_hbase_reference_guide)
#### [150. Updating hbase.apache.org](./171_150_updating_hbase_apache_org)
#### [151. Tests](./172_151_tests)
#### [152. Developer Guidelines](./173_152_developer_guidelines)
 

## [Unit Testing HBase Applications](./174_unit_testing_hbase_applications)
 
#### [153. JUnit](./175_153_junit)
#### [154. Mockito](./176_154_mockito)
#### [155. MRUnit](./177_155_mrunit)
#### [156. Integration Testing with an HBase Mini-Cluster](./178_156_integration_testing_with_an_hbase_mini_cluster)
 

## [Protobuf in HBase](./179_protobuf_in_hbase)
 
#### [157. Protobuf](./180_157_protobuf)
 

## [ZooKeeper](./181_zookeeper)
 
#### [158. Using existing ZooKeeper ensemble](./182_158_using_existing_zookeeper_ensemble)
#### [159. SASL Authentication with ZooKeeper](./183_159_sasl_authentication_with_zookeeper)
 

## [Community](./184_community)
 
#### [160. Decisions](./185_160_decisions)
#### [161. Community Roles](./186_161_community_roles)
#### [162. Commit Message format](./187_162_commit_message_format)
 

## [Appendix](./188_appendix)
 
#### [Appendix A: Contributing to Documentation](./189_appendix_a_contributing_to_documentation)
#### [Appendix B: FAQ](./190_appendix_b_faq)
#### [Appendix C: hbck In Depth](./191_appendix_c_hbck_in_depth)
#### [Appendix D: Access Control Matrix](./192_appendix_d_access_control_matrix)
#### [Appendix E: Compression and Data Block Encoding In HBase](./193_appendix_e_compression_and_data_block_encoding_in_hbase)
#### [163. Enable Data Block Encoding](./194_163_enable_data_block_encoding)
#### [Appendix F: SQL over HBase](./195_appendix_f_sql_over_hbase)
#### [Appendix G: YCSB](./196_appendix_g_ycsb)
#### [Appendix H: HFile format](./197_appendix_h_hfile_format)
#### [Appendix I: Other Information About HBase](./198_appendix_i_other_information_about_hbase)
#### [Appendix J: HBase History](./199_appendix_j_hbase_history)
#### [Appendix K: HBase and the Apache Software Foundation](./200_appendix_k_hbase_and_the_apache_software_foundation)
#### [Appendix L: Apache HBase Orca](./201_appendix_l_apache_hbase_orca)
#### [Appendix M: Enabling Dapper-like Tracing in HBase](./202_appendix_m_enabling_dapper_like_tracing_in_hbase)
#### [164. Client Modifications](./203_164_client_modifications)
#### [165. Tracing from HBase Shell](./204_165_tracing_from_hbase_shell)
#### [Appendix N: 0.95 RPC Specification](./205_appendix_n_0_95_rpc_specification)
