## Karaf monitoring using collectd and GenericJMX
```xml
LoadPlugin java
<Plugin "java">
    JVMARG "-Djava.class.path=/usr/share/collectd/java/collectd-api.jar:/usr/share/collectd/java/generic-jmx.jar"
    LoadPlugin "org.collectd.java.GenericJMX"
  <Plugin "GenericJMX">
    <MBean "karaf/classes">
      ObjectName "java.lang:type=ClassLoading"
      InstancePrefix "karaf_java"
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
    <MBean "karaf/compilation">
      ObjectName "java.lang:type=Compilation"
      InstancePrefix "karaf_java"
      #InstanceFrom ""
 
      <Value>
        Type "total_time_in_ms"
        InstancePrefix "compilation_time"
        #InstanceFrom ""
        Table false
        Attribute "TotalCompilationTime"
      </Value>
    </MBean>
 
    <MBean "karaf/memory">
      ObjectName "java.lang:type=Memory,*"
      InstancePrefix "karaf_java_memory"
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
 
    <MBean "karaf/memory_pool">
       ObjectName "java.lang:type=MemoryPool,*"
       InstancePrefix "karaf_java_memory_pool-"
       InstanceFrom "name"
       <Value>
         Type "memory"
         #InstancePrefix ""
         #InstanceFrom ""
         Table true
         Attribute "Usage"
       </Value>
    </MBean> 
 
    # Garbage collector information
    <MBean "karaf/garbage_collector">
      ObjectName "java.lang:type=GarbageCollector,*"
      InstancePrefix "karaf_java_gc-"
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
   
   <Connection>
      Host "MyHost55"
      ServiceURL "service:jmx:rmi://localhost:44444/jndi/rmi://localhost:1099/karaf-root"
      User "jmxuser"
      Password "jmxpassword"
      Collect "karaf/classes"
      Collect "karaf/compilation"
      Collect "karaf/memory"
      Collect "karaf/memory_pool"
      Collect "karaf/garbage_collector"
   </Connection>
  </Plugin>
</Plugin>

```