## TARGET AGENT ##  
## configuration file location:  /etc/flume-ng/conf
## START Agent: flume-ng agent -c conf -f /etc/flume-ng/conf/flume-trg-agent.conf -n collector

#http://flume.apache.org/FlumeUserGuide.html#avro-source
collector.sources = AvroIn  
collector.sources.AvroIn.type = avro  
collector.sources.AvroIn.bind = 0.0.0.0  
collector.sources.AvroIn.port = 4545  
collector.sources.AvroIn.channels = mc1 mc2

## Channels ##
## Source writes to 2 channels, one for each sink
collector.channels = mc1 mc2

#http://flume.apache.org/FlumeUserGuide.html#memory-channel

collector.channels.mc1.type = memory  
collector.channels.mc1.capacity = 100

collector.channels.mc2.type = memory  
collector.channels.mc2.capacity = 100

## Sinks ##
collector.sinks = LocalOut HadoopOut

## Write copy to Local Filesystem 
#http://flume.apache.org/FlumeUserGuide.html#file-roll-sink
collector.sinks.LocalOut.type = file_roll  
collector.sinks.LocalOut.sink.directory = /var/log/flume-ng  
collector.sinks.LocalOut.sink.rollInterval = 0  
collector.sinks.LocalOut.channel = mc1


## Write to HDFS
#http://flume.apache.org/FlumeUserGuide.html#hdfs-sink
collector.sinks.HadoopOut.type = hdfs  
collector.sinks.HadoopOut.channel = mc2  
collector.sinks.HadoopOut.hdfs.path = /user/root/flume-channel/%{log_type}/%y%m%d  
collector.sinks.HadoopOut.hdfs.fileType = DataStream  
collector.sinks.HadoopOut.hdfs.writeFormat = Text  
collector.sinks.HadoopOut.hdfs.rollSize = 0  
collector.sinks.HadoopOut.hdfs.rollCount = 10000  
collector.sinks.HadoopOut.hdfs.rollInterval = 600
