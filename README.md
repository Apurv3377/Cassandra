# Cassandra
Performance Tests

# For Executing Stress Tests :

cassandra-stress user profile=/root/stress.yaml n=100000 ops\(insert=1\) -graph file=test.html title=test revision=test1

# References :
https://www.datastax.com/dev/blog/improved-cassandra-2-1-stress-tool-benchmark-any-schema
https://docs.datastax.com/en/cassandra/3.0/cassandra/tools/toolsCStress.html

# How Stress Tests Works :
https://www.instaclustr.com/deep-diving-cassandra-stress-part-3-using-yaml-profiles/

# Write Tests :
Start the Cassandra node using the write_survey option:

    Cassandra package installations: Add the following option to cassandra-env.sh file:

    JVM_OPTS="$JVM_OPTS -Dcassandra.write_survey=true

    Cassandra tarball installations: Start Cassandra with this option:

    cd install_location
    $ sudo bin/dse cassandra -Dcassandra.write_survey=true #Starts DataStax Enterprise

# Ref :
https://docs.datastax.com/en/cassandra/3.0/cassandra/operations/opsTestCompactCompress.html


# Additonal Optimisations Queries:
Optimizations :


ALTER TABLE cassandra_test1 
WITH compaction = {'compaction_window_size': '1', 
    			   'compaction_window_unit': 'MINUTES', 
    			   'class': 'org.apache.cassandra.db.compaction.TimeWindowCompactionStrategy'};


ALTER TABLE cassandra_test1 with bloom_filter_fp_chance=0.01;


alter table cassandra_test1 with compression={'class' : 'LZ4Compressor','chunk_length_in_kb' : 4};


CREATE TABLE perf.cassandra_test2 (
    log_id text,
    dtb text,
    logtype text,
    pid int,
    rip text,
    rsp text,
    syscall_name text,
    syscall_nr text,
    value int,
    vmid text,
    PRIMARY KEY(log_id, vmid)
) WITH bloom_filter_fp_chance = 0.01
    AND caching = {'keys': 'ALL', 'rows_per_partition': 'NONE'}
    AND comment = ''
    AND compaction = {'class': 'org.apache.cassandra.db.compaction.TimeWindowCompactionStrategy', 'compaction_window_size': '1', 'compaction_window_unit': 'MINUTES', 'max_threshold': '32', 'min_threshold': '4'}
    AND compression = {'chunk_length_in_kb': '4', 'class': 'org.apache.cassandra.io.compress.LZ4Compressor'}
    AND crc_check_chance = 0
    AND dclocal_read_repair_chance = 0.1
    AND default_time_to_live = 0
    AND gc_grace_seconds = 864000
    AND max_index_interval = 2048
    AND memtable_flush_period_in_ms = 0
    AND min_index_interval = 128
    AND read_repair_chance = 0.0
    AND speculative_retry = 'NONE';
