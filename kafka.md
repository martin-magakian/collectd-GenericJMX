## Kafka monitoring using collectd and GenericJMX
```xml
LoadPlugin java
<Plugin "java">

  JVMARG "-Djava.class.path=/usr/share/collectd/java/collectd-api.jar:/usr/share/collectd/java/generic-jmx.jar"
  LoadPlugin "org.collectd.java.GenericJMX"

   <Plugin "GenericJMX">

     <MBean "kafka/messageIn">
        ObjectName "kafka.server:type=BrokerTopicMetrics,name=MessagesInPerSec"
        InstancePrefix "mytopicname"
        <Value>
           Type "counter"
           Table false
           Attribute "Count"
           InstancePrefix "kafka-messages-in"
        </Value>
     </MBean>
     <MBean "kafka/requestRate">
        ObjectName "kafka.network:type=RequestMetrics,name=RequestsPerSec,request={Produce|FetchConsumer|FetchFollower}"
        InstancePrefix "mytopicname"
        <Value>
           Type "counter"
           Table false
           Attribute "Count"
           InstancePrefix "kafka-messages-in"
        </Value>
     </MBean>
    </Plugin>


    <Connection>
      Host "MyHost55"
      ServiceURL "service:jmx:rmi:///jndi/rmi://localhost:9997/jmxrmi"
      Collect "kafka/requestRate"
      Collect "kafka/messageIn"
    </Connection>
  </Plugin>
</Plugin>
```