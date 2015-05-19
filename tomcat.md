## Tomcat monitoring using collectd and GenericJMX
```xml
LoadPlugin java
<Plugin "java">
  JVMARG "-Djava.class.path=/usr/share/collectd/java/collectd-api.jar:/usr/share/collectd/java/generic-jmx.jar"
  LoadPlugin "org.collectd.java.GenericJMX"

  <Plugin "GenericJMX">

   <MBean "memory">
      ObjectName "java.lang:type=Memory,*"
      InstancePrefix "java_memory"
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

    ################
    # MBean blocks #
    ################
    # Number of classes being loaded.
    <MBean "classes">
      ObjectName "java.lang:type=ClassLoading"
      InstancePrefix "java"
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
    <MBean "compilation">
      ObjectName "java.lang:type=Compilation"
      InstancePrefix "java"
      #InstanceFrom ""

      <Value>
        Type "total_time_in_ms"
        InstancePrefix "compilation_time"
        #InstanceFrom ""
        Table false
        Attribute "TotalCompilationTime"
      </Value>
    </MBean>

    # Garbage collector information
    <MBean "garbage_collector">
      ObjectName "java.lang:type=GarbageCollector,*"
      InstancePrefix "java_gc-"
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

   <MBean "memory_pool">
      ObjectName "java.lang:type=MemoryPool,*"
      InstancePrefix "java_memory_pool-"
      InstanceFrom "name"
      <Value>
        Type "memory"
        #InstancePrefix ""
        #InstanceFrom ""
        Table true
        Attribute "Usage"
      </Value>
   </MBean>

   <MBean "garbage_collector">
      ObjectName "java.lang:type=GarbageCollector,*"
      InstancePrefix "java_gc-"
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


    ### MBeans by Catalina / Tomcat ###
    # The global request processor (summary for each request processor)
    <MBean "catalina/global_request_processor">
      ObjectName "Catalina:type=GlobalRequestProcessor,*"
      InstancePrefix "catalina_request_processor-"
      InstanceFrom "name"

      <Value>
        Type "io_octets"
        InstancePrefix "global"
        #InstanceFrom ""
        Table false
        Attribute "bytesReceived"
        Attribute "bytesSent"
      </Value>

      <Value>
        Type "total_requests"
        InstancePrefix "global"
        #InstanceFrom ""
        Table false
        Attribute "requestCount"
      </Value>

      <Value>
        Type "total_time_in_ms"
        InstancePrefix "global-processing"
        #InstanceFrom ""
        Table false
        Attribute "processingTime"
      </Value>
    </MBean>

    #####################
    # Connection blocks #
    #####################
    <Connection>
      ServiceURL "service:jmx:rmi:///jndi/rmi://localhost:8086/jmxrmi"
      User "userread"
      Password "toto"
      Host "MyHost"
      Collect "classes"
      Collect "compilation"
      Collect "garbage_collector"
      Collect "memory"
      Collect "memory_pool"
      Collect "catalina/global_request_processor"
    </Connection>
  </Plugin>
</Plugin>
```


By [Anomaly Detection](https://anomaly.io)
