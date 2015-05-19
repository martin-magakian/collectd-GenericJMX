## Cassandra monitoring using collectd and GenericJMX
```xml
LoadPlugin java
<Plugin "java">
    JVMARG "-Djava.class.path=/usr/share/collectd/java/collectd-api.jar:/usr/share/collectd/java/generic-jmx.jar"
    LoadPlugin "org.collectd.java.GenericJMX"
    
  <Plugin "GenericJMX">
    <MBean "cassandra/classes">
      ObjectName "java.lang:type=ClassLoading"
      InstancePrefix "cassandra_java"
      #InstanceFrom ""
  
      <Value>
        Type "gauge"
        InstancePrefix "loaded_classes"
        #InstanceFrom ""
        Table false
        Attribute "LoadedClassCount"
      </Value>
    </MBean>
  
    # Time spent by the JVM compiling or optimizing.
    <MBean "cassandra/compilation">
      ObjectName "java.lang:type=Compilation"
      InstancePrefix "cassandra_java"
      #InstanceFrom ""
  
      <Value>
        Type "total_time_in_ms"
        InstancePrefix "compilation_time"
        #InstanceFrom ""
        Table false
        Attribute "TotalCompilationTime"
      </Value>
    </MBean>

    <MBean "cassandra/metrics">
      ObjectName "org.apache.cassandra.metrics:type=ClientRequest,*"
      InstancePrefix "cassandra_client_request-latency"
      #InstanceFrom ""

      <Value>
        Type "total_time_in_ms"
        #InstancePrefix "compilation_time"
        InstanceFrom "scope"
        Table false
        Attribute "Mean"
      </Value>
    </MBean>

  
    <MBean "cassandra/storage_proxy">
      ObjectName "org.apache.cassandra.db:type=StorageProxy"
      InstancePrefix "cassandra_activity_storage_proxy"
      #InstanceFrom "name"

      <Value>
        Type "counter"
        InstancePrefix "read"
#        InstanceFrom "operations"
        Table false
        Attribute "ReadOperations"
      </Value>
      <Value>
        Type "counter"
        InstancePrefix "write"
#        InstanceFrom "operations"
        Table false
        Attribute "WriteOperations"
      </Value>

    </MBean>


    <MBean "cassandra/memory">
      ObjectName "java.lang:type=Memory,*"
      InstancePrefix "cassandra_java_memory"
      #InstanceFrom "name"
      <Value>
        Type "memory"
        InstancePrefix "heap-"
        #InstanceFrom ""
        Table true
        Attribute "HeapMemoryUsage"
      </Value>
  
      <Value>
        Type "memory"
        InstancePrefix "nonheap-"
        #InstanceFrom ""
        Table true
        Attribute "NonHeapMemoryUsage"
      </Value>
    </MBean>
  
   <MBean "cassandra/memory_pool">
      ObjectName "java.lang:type=MemoryPool,*"
      InstancePrefix "cassandra_java_memory_pool-"
      InstanceFrom "name"
      <Value>
        Type "memory"
        #InstancePrefix ""
        #InstanceFrom ""
        Table true
        Attribute "Usage"
      </Value>
   </MBean>
  
   <MBean "cassandra/garbage_collector">
      ObjectName "java.lang:type=GarbageCollector,*"
      InstancePrefix "cassandra_java_gc-"
      InstanceFrom "name"
  
      <Value>
         Type "invocations"
         #InstancePrefix ""
         #InstanceFrom ""
         Table false
         Attribute "CollectionCount"
      </Value>
  
      <Value>
         Type "total_time_in_ms"
         InstancePrefix "collection_time"
         #InstanceFrom ""
         Table false
         Attribute "CollectionTime"
      </Value>
   </MBean>

   <MBean "cassandra/concurrent">
    # This was called org.apache.cassandra.concurrent in 0.6 and we want to preserve the data, so...
    ObjectName "org.apache.cassandra.internal:type=*"
    InstancePrefix "cassandra_activity_internal"
    <Value>
            Attribute "CompletedTasks"
            InstanceFrom "type"
            Type "counter"
            InstancePrefix "tasks-"
    </Value>
   </MBean>

   <MBean "cassandra/request">
        ObjectName "org.apache.cassandra.request:type=*"
        InstancePrefix "cassandra_activity_request-"
       <Value>
               Attribute "CompletedTasks"
               InstanceFrom "type"
           Type "counter"
               InstancePrefix "tasks-"
       </Value>
   </MBean>

   <MBean "cassandra/cfstats">
        ObjectName "org.apache.cassandra.db:type=ColumnFamilies,*"
        InstanceFrom "keyspace"
        InstancePrefix "cassandra_columnfamilies_stats-"
        <Value>
                Attribute "ReadCount"
                InstancePrefix "livereadcount-"
                Type "counter"
                InstanceFrom "columnfamily"
        </Value>
        <Value>
                Attribute "TotalReadLatencyMicros"
                InstancePrefix "livereadlatency-"
                Type "counter"
                InstanceFrom "columnfamily"
        </Value>
        <Value>
                Attribute "WriteCount"
                InstancePrefix "livewritecount-"
                Type "counter"
                InstanceFrom "columnfamily"
        </Value>
        <Value>
                Attribute "TotalWriteLatencyMicros"
                InstancePrefix "livewritelatency-"
                Type "counter"
                InstanceFrom "columnfamily"
        </Value>
       <Value>
               Attribute "LiveSSTableCount"
               InstancePrefix "live_sstable_count-"
               Type "gauge"
               InstanceFrom "columnfamily"
       </Value>
        <Value>
                Attribute "TotalDiskSpaceUsed"
                InstancePrefix "total_disk_space_used-"
                Type "gauge"
                InstanceFrom "columnfamily"
        </Value>

   </MBean>

   <MBean "cassandra/compaction">
        ObjectName "org.apache.cassandra.db:type=CompactionManager"
        InstancePrefix "cassandra_compaction"
        <Value>
                Attribute "PendingTasks"
                InstancePrefix "pending"
                Type "gauge"
        </Value>
   </MBean>

   <Connection>
      Host "MyHost55"
      ServiceURL "service:jmx:rmi:///jndi/rmi://localhost:7199/jmxrmi"
      Collect "cassandra/storage_proxy"
      Collect "cassandra/classes"
      Collect "cassandra/compilation"
      Collect "cassandra/memory"
      Collect "cassandra/memory_pool"
      Collect "cassandra/garbage_collector"
      Collect "cassandra/metrics"
      Collect "cassandra/concurrent"
      Collect "cassandra/cfstats"
      Collect "cassandra/request"
      Collect "cassandra/compaction"
   </Connection>
  </Plugin>
</Plugin>
```

By [Anomaly Detection](https://anomaly.io)
