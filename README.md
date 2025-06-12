## Kafka setup :

> https://www.geeksforgeeks.org/installation-guide/how-to-install-and-run-apache-kafka-on-windows/

## YouTube Channels used to learn Kafka :

> https://youtu.be/tU_37niRh4U?si=fKPbW1ahauKlCQWy

# Kafka Hands-On Tasks Guide

This guide provides essential Apache Kafka commands and tasks to practice on Windows. It covers creating topics, producing and consuming messages, managing consumer groups, and more.

---

## Prerequisites

- Kafka installed on Windows
- ZooKeeper and Kafka broker running in separate terminals
- Kafka binaries located in `C:\kafka\bin\windows` (adjust if needed)

---

## Part 1: Kafka General Tasks

### 1. Create a Topic
kafka-topics.bat --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

### 2. List All Topics
kafka-topics.bat --list --bootstrap-server localhost:9092

### 3. Describe a Topic
kafka-topics.bat --describe --topic test-topic --bootstrap-server localhost:9092

### 4. Produce Messages (Console Producer)
kafka-console-producer.bat --topic test-topic --bootstrap-server localhost:9092
(Type messages and press Enter to send.)

### 5. Consume Messages (Console Consumer)
kafka-console-consumer.bat --topic test-topic --from-beginning --bootstrap-server localhost:9092

### 6. Consume Only New Messages
kafka-console-consumer.bat --topic test-topic --bootstrap-server localhost:9092

### 7. Delete a Topic
kafka-topics.bat --delete --topic test-topic --bootstrap-server localhost:9092
(Enable topic deletion by setting `delete.topic.enable=true` in `server.properties` and restart Kafka broker.)

### 8. Create Topic with Multiple Partitions
kafka-topics.bat --create --topic multi-partition-topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1

### 9. Produce Messages with Keys (Assign Partitions)
kafka-console-producer.bat --topic multi-partition-topic --bootstrap-server localhost:9092 --property "parse.key=true" --property "key.separator=:"
(Type messages like: key1:Hello, key2:World)

### 10. Consume from a Specific Partition
kafka-console-consumer.bat --topic multi-partition-topic --partition 0 --bootstrap-server localhost:9092 --from-beginning

### 11. Check Kafka Broker Status (Basic)
netstat -an | findstr 9092

### 12. Set Message Retention for a Topic
kafka-configs.bat --alter --entity-type topics --entity-name test-topic --add-config retention.ms=60000 --bootstrap-server localhost:9092

### 13. Check Topic Configuration
kafka-configs.bat --describe --entity-type topics --entity-name test-topic --bootstrap-server localhost:9092

### 14. Create Topic with Custom Max Message Size
kafka-topics.bat --create --topic big-messages --bootstrap-server localhost:9092 --config max.message.bytes=1048576 --partitions 1 --replication-factor 1

### 15. Increase Number of Partitions (Can Only Increase)
kafka-topics.bat --alter --topic test-topic --partitions 3 --bootstrap-server localhost:9092

---

## Part 2: Kafka Consumer Group Tasks

### 1. Start a Consumer with a Group
kafka-console-consumer.bat --topic test-topic --bootstrap-server localhost:9092 --group my-group

### 2. Start Multiple Consumers in the Same Group
(Open multiple terminals and run the above command with the same group name (my-group). Kafka balances partitions across consumers.)

### 3. List All Consumer Groups
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --list

### 4. Describe a Consumer Group
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --describe --group my-group

### 5. Reset Offsets for a Consumer Group (To Beginning)
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --group my-group --topic test-topic --reset-offsets --to-earliest --execute

### 6. Consume from Beginning with Group (After Reset)
kafka-console-consumer.bat --topic test-topic --from-beginning --bootstrap-server localhost:9092 --group my-group

### 7. Check Consumer Lag
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --describe --group my-group

### 8. Delete a Consumer Group (Kafka 2.4+)
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --delete --group my-group
(Only possible if the group is inactive (no consumers connected).)

### 9. Assign Partitions Manually to a Consumer
kafka-console-consumer.bat --topic test-topic --partition 0 --bootstrap-server localhost:9092 --group my-group

### 10. Consume Without a Group (No Offset Save)
kafka-console-consumer.bat --topic test-topic --bootstrap-server localhost:9092 --from-beginning --group ""

---

## Advanced Challenges

- Create a topic with 3 partitions and run 3 consumers in the same group to observe partition assignment.
- Simulate a consumer crash (Ctrl+C) and watch another consumer take over.
- Use multiple consumer groups on the same topic to simulate different applications consuming independently.

---

## Notes

- Adjust Kafka binary paths if installed elsewhere.
- Make sure ZooKeeper is running before starting Kafka brokers (unless using KRaft mode).
- Use Ctrl+C to stop Kafka processes safely.

---
