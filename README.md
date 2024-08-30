:: Display Topic Information
kafka-topics.bat --describe --zookeeper localhost:2181 --topic beacon

:: Add Partitions to a Topic
kafka-topics.bat --alter --zookeeper localhost:2181 --topic beacon --partitions 3
:: WARNING: If partitions are increased for a topic that has a key, the partition logic or ordering of the messages will be affected
:: Adding partitions succeeded!

:: Change Topic Retention (Set SLA)
kafka-topics.bat --zookeeper localhost:2181 --alter --topic mytopic --config retention.ms=28800000
:: This sets retention of 8 hours on messages coming to the topic `mytopic`. After 8 hours, messages will be deleted.

:: Delete a Topic
kafka-run-class.bat kafka.admin.DeleteTopicCommand --zookeeper localhost:2181 --topic test

:: Create Topics
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic file_acquire_complete
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic job_result
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic trigger_match
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic event_result

:: List Existing Topics
kafka-topics.bat --list --zookeeper localhost:2181

:: Produce Messages with Console Producer
kafka-console-producer.bat --broker-list localhost:9092 --topic test

:: Consume Messages with Console Consumer
kafka-console-consumer.bat --zookeeper localhost:2181 --topic test --from-beginning

:: Purge a Topic
:: Set retention to 1 second
kafka-topics.bat --zookeeper localhost:2181 --alter --topic mytopic --config retention.ms=1000

:: Wait a minute...

:: Remove the retention config
kafka-topics.bat --zookeeper localhost:2181 --alter --topic mytopic --delete-config retention.ms

:: Delete a Topic
kafka-topics.bat --zookeeper localhost:2181 --delete --topic mytopic

:: Get the Earliest Offset Still in a Topic
kafka-run-class.bat kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic mytopic --time -2

:: Get the Latest Offset in a Topic
kafka-run-class.bat kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic mytopic --time -1

:: Consume Messages from the Beginning
kafka-console-consumer.bat --new-consumer --bootstrap-server localhost:9092 --topic mytopic --from-beginning

:: Get the Consumer Offsets for a Topic
kafka-consumer-offset-checker.bat --zookeeper=localhost:2181 --topic=mytopic --group=my_consumer_group

:: Read from __consumer_offsets
:: Add the following property to config/consumer.properties: exclude.internal.topics=false
kafka-console-consumer.bat --consumer.config config/consumer.properties --from-beginning --topic __consumer_offsets --zookeeper localhost:2181 --formatter "kafka.coordinator.GroupMetadataManager$OffsetsMessageFormatter"

:: Kafka Consumer Groups
:: List Consumer Groups (Old API)
kafka-consumer-groups.bat --zookeeper localhost:2181 --list

:: List Consumer Groups (New API)
kafka-consumer-groups.bat --new-consumer --bootstrap-server localhost:9092 --list

:: View Consumer Group Details
kafka-consumer-groups.bat --zookeeper localhost:2181 --describe --group <group name>

:: Zookeeper - Starting the Zookeeper Shell
zookeeper-shell.bat localhost:2181
