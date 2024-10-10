---
layout: post
title: Apache Kafka Tips
short_description: I decided to centralize the Apache Kafka commands and tool tips to help me access them quickly...
date: 2024-01-03
updated_at: 2024-04-22
---

# Apache Kafka Tips

![Apache Kafka](https://raw.githubusercontent.com/rca0/rca0.github.io/master/_posts/assets/kafka.png){:width="70%" style="display: block; margin-left: auto; margin-right: auto;"}

Apache Kafka is an open-source distributed event streaming platform used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications.

I decided to centralize the Apache Kafka commands and tool tips to help me access them quickly. Therefore, I will cover the main Kafka commands here.

## Concepts

### LEADER

A Leader is the broker/replica accepting produce messages

### FOLLOWER

A Follower is a broker/replica that can join an ISR list and participate of the high watermark (used by the leader when acknowledging messages back to the producer).

### OBSERVER

An Observer is a broker/replica that also has a copy of data for a given topic-partition, and consumers are allowed to read from them even though it is not the leader (know as “Follower Fetching”).

The data is copied asynchronous from the leader such that a producer does not wait on observers to get back an acknowledgment.

By the default Observers do not participate in the ISR list and cannot automatically become the leader if the current leader fails, but if a user manually changes leader assignment then they can participate in the ISR list.

### ISR List

An ISR list (in-sync replicas) includes brokers that have a given topic-partition. The data is copied from the leader to every member of the ISR before the producer gets an acknowledgment. The followers in an ISR can become the leader if the current leader fails.

## Download 

- https://kafka.apache.org/downloads

## Kafka Auth Settings

When you are using authentication, which is often required for Confluent Cloud clusters, you must create a `cluster-with-auth.properties` file with content similar to:

- Set up the credentials

`~/kafka_2.12-3.3.2/config/cluster-with-auth.properties`
```bash
security.protocol=SASL_SSL
sasl.mechanism=SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="<USERNAME-HERE>" password="<PASSWORD-HERE>";
```

- Pass the credential file using the Kafka command line.

```bash
./<command-here> <paramenters-here> \
    --command-config ~/kafka_2.13-3.4.1/config/cluster-with-auth.properties
```

```bash
...<kafka-command-line> --command-config ../config/cluster-with-auth.properties
```

--- 

# Apache Kafka - Command lines

## Performance tests

```bash
time kafka-producer-perf-test --producer-props bootstrap.servers=$kf_brokers \
    --topic $topic \
    --num-records $messages \
    --record-size $size \
    --throughput $throughput \
    --print-metrics \
    enable.idempotence=true \
    compression.type=snappy \
    linger.ms=20 
```

## Kafka-configs

* To add partitions

```bash
./kafka-configs.sh --bootstrap-server $kf_brokers \
    --alter \
    --entity-name $topic_name \
    --entity-type topics \
    --add-config \
    --partitions 6
```

* To alter Topic Replica Placement

`topic-reg.cfg`
```json
{
    "version": 1,
    "replicas": [
        "count": 3,
        "constraints": {"rack": "us-east-1"}
    ],
    "observers": [
        "count": 3,
        "constraints": {"rack": "central-us-1"}
    ]
}
```

```bash
./kafka-configs.sh --bootstrap-server $kf_brokers \
    --alter \
    --entity-name $topic_name \
    --entity-type topics \
    --replica-placement topic-reg.cfg
```

* To alter config

```bash
./kafka-configs.sh --bootstrap-server $kf_brokers \
    --alter \
    --entity-name $topic_name \
    --entity-type topics \
    --add-config \
    x=y
```

* To remove a config

```bash
./kafka-configs.sh --bootstrap-server $kf_brokers \
    --alter \
    --entity-name $topic_name \
    --entity-type topics \
    --delete-config \
    x=y
```

## Topics

* Consume a topic offset

```bash
./kafka-console-consumer.sh --bootstrap-server $kf_brokers --topic $topic_name
```

* Consumer a topic offset using key and value

```bash
./kafka-console-consumer --bootstrap-server $kf_brokers --topic $topic_name --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true
```

* Consumer a topic from the beginning

```bash
./kafka-console-consumer.sh --bootstrap-server $kf_brokers --topic $topic_name --from-beginning
```

* Create new topic

```bash
kafka-topics --bootstrap-server $kf_brokers --create --topic $topic_name --partitions $parition_number --replication-factor $replica_factor --config retention.ms=$retention_time --config segment.ms=10000

# tips
# --if-not-exists use this parameter if not exists
```

* Create new topic with constraints

```bash
kafka-topics --bootstrap-server $kf_brokers --create --topic $topic_name --partitions $parition_number --replication-factor $replica_factor --replica-placement /etc/kafka/topic-reg.cfg
```

* List topics

```bash
./kafka-topics.sh --bootstrap-server $kf_brokers --list
```

* Describe topic

```bash
./kafka-topics --bootstrap-server $kf_brokers --topic $topic_name --describe 
```

* Show partitions offsets of a Topic

```bash
./kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list $kf_brokers --topic $topic_name
```

* Show specific topic configuration overrides

```bash
./kafka-topics.sh --bootstrap-server $kf_brokers --describe --entity-type topics --entity-name $topic_name
```

* Delete a Topic

```bash
./kafka-topics.sh --bootstrap-server $kf_brokers --delete --topic $topic_name
```

* Remove topic from Consumer Group
```bash
./kafka-consumer-groups.sh --bootstrap-server $kf_brokers --group $consumer_group_name --topic $topic_name --delete
```

* Change Topic partition number

```bash
./kafka-topics.sh --bootstrap-server $kf_brokers --alter --topic $topic_name --partitions $partition_number
```

* Find all the partitions where are not in-sync with the Leader

```bash
./kafka-topics.sh --bootstrap-server $kf_brokers --describe --under-replicated-partitions
```

* Reassignment Topic Configs

- Change the partitions numbers
- Add Replicas
- Add Observers

```bash
# Check topic configurations
./kafka-topics.sh --bootstrap-server $kf_brokers --topic $topic --describe
```

topic-reassign.json
```json
{
  "version":1,
  "partitions":[
    {
      "topic":"topic-one",
      "partition":0,
      "replicas":[1,2,6],
      "observers":[2,6]
    },
    {
      "topic":"topic-one",
      "partition":1,
      "replicas":[1,2,6],
      "observers":[2,6],
    },
    {
      "topic":"topic-one",
      "partition":2,
      "replicas":[1,2,6],
      "observers":[2,6]
    }
  ]
}
```

execute kafka reassign partitions command line

```bash
./kafka-reassign-partitions.sh --bootstrap-server $kf_broker --reassignment-json-file topic-reassign.json --execute
```

increse topic partitions number to 3

```bash
./kafka-topics.sh --bootstrap-server $kf_brokers --alter --topic $topic --partitions 3
```

_For OLD Kafka version, use `--zookeeper` instead of `--bootstrap-server` argument_

* Get topic size per GB

```bash
./kafka-log-dirs.sh --bootstrap-server $kf_brokers --describe  | grep -E "^{"  | jq '[.brokers[].logDirs[].partitions[]] |  sort_by(.size) | map({ (.partition): (.size / (1024*1024*1024) | round | tostring + " GB") })'  | grep GB
```

* Change topic log retention settings

| **ms**      | **days** |
| ---         | ---      |
| 259200000   | 3        |
| 604800000   | 7        |
| 1209600000  | 14       |
| 2592000000  | 30       |

```bash
# log.retention.ms=-1 
# -1 means it is infinte...

./kafka-configs.sh --bootstrap-server $kf_brokers --alter --entity-name $topic_name --entity-type topics --add-config retention.ms=$retention_time

# then validate the topic settings
./kafka-topics.sh --describe --ootstrap-server $kf_brokers --topic $topic_name
```

* Purge Topics offsets by retention time ms

As a workaround, change the retention to one minute. This allows you to purge the offsets quickly, and then you can simply return the retention time to its original setting.

```bash
./kafka-configs.sh --bootstrap-server $kf_brokers --alter --entity-type topics --entity-name $topic_name --add-config retention.ms=1000
```

* Remove Offsets from a Partition of a Topic

Generally, when the data retention of a topic is very long or infinite, it can fill the disk, causing major unforeseen events for those using the cluster.

Generate a JSON file:

```json
{"partitions": [
    {"topic": "topic_name", "partition": 0, "offset":  200000000},
    {"topic": "topic_name", "partition": 1, "offset":  200000000},
    {"topic": "topic_name", "partition": 2, "offset":  200000000},
    {"topic": "topic_name", "partition": 3, "offset":  200000000},
    {"topic": "topic_name", "partition": 4, "offset":  200000000}
    ],
 "version":1 }
```

When executing the command below, messages will be removed up to data prior to the informed offset, 200000000;

```bash
/kafka-delete-records.sh --bootstrap-server $kf_brokers --offset-json-file ./file.json
```

* Leader election for partitions

Election types
- `preferred`; the election is only performed if the current leader is not the preferred leader for the topic partition
- `unclean`; the election is only performed if there are no leader for the topic partition

```bash
./kafka-leader-election --bootstrap-server $kf_brokers --all-topic-partitions --election-type preferred
```

Or using JSON FILE

```json
{"partitions":
   [
      {"topic": "topic-name", "partition": 0}
   ]
}
```

```
# ./kafka-leader-election --bootstrap-server $kf_brokers --election-type preferred --path-to-json-file topic.json
```

* Get Topic Count

```
./kafka-run-class.sh kafka.tools.GetOffsetShell --bootstrap-server $kf_brokers --topic $topic --time -1
./kafka-run-class.sh kafka.tools.GetOffsetShell --bootstrap-server $kf_brokers --topic $topic --time -2
...
# Pass --command-config <FILE-WITH-SSL-CONFIGs> for kafka auth's
```

## Consumer-groups

* List Consumer Groups

```bash
,/kafka-consumer-groups.sh --bootstrap-server $kf_brokers --list
```

* List and Describe all Consumer Groups

```bash
,/kafka-consumer-groups.sh --bootstrap-server $kf_brokers --all-groups --describe
```

* Describe Consumer Groups

```bash
./kafka-consumer-groups.sh --bootstrap-server $kf_brokers --describe --group $consumer_group_name --verbose
```

* Reset offset pointer to latest

```bash
# dry-run
./kafka-consumer-groups.sh --bootstrap-server $kf_brokers --topic $topic_name:$partition_number --group $group_name --reset-offsets --to-latest --dry-run

# execute
./kafka-consumer-groups.sh --bootstrap-server $kf_brokers --topic $topic_name:$partition_number --group $group_name --reset-offsets --to-latest --execute
```

* Reset offset pointer to beginning 

```bash
# dry-run
./kafka-consumer-groups.sh --bootstrap-server $kf_brokers --topic $topic_name:$partition_number --group $group_name --reset-offsets --to-earliest --dry-run

# execute
./kafka-consumer-groups.sh --bootstrap-server $kf_brokers --topic $topic_name:$partition_number --group $group_name --reset-offsets --to-earliest --execute
```

## Producer

* Produce message to a topic

```bash
./kafka-console-producer.sh --broker-list $kf_brokers --topic $topic_name

# from a file
./kafka-console-producer.sh --broker-list $kf_brokers --topic $topic_name < input-messages.txt
```

* Produce message to a topic using key and value

```bash
kafka-console-producer.sh --broker-list $kf_brokers --topic $topic_name --property parse.key=true 
--property key.separator=:

# e.g.
# >key:value
# >foo:bar
# >anotherKey:another value
```

## Zookeeper Commands

- Connect to Zookeeper command line

```bash
./zookeeper-shell.sh $zookeeper_broker:2181
```

- List brokers in Kafka cluster

```bash
./zookeeper-shell $zookeeper_broker:2181 ls /brokers/ids
```

- Get Kafka cluster ID

```bash
./zookeeper-shell $zookeeper_broker:2181 get /broker/ids/$broker_id
```

- Get details of specific topic

```bash
./zookeeper-shell $zookeeper_broker:2181 get /brokers/topics/$topic_name
```

- Check if zookeeper is healthy or not

```bash
echo stat | nc $zk_broker 2181
echo ruok | nc $zk_broker 2181
echo mntr | nc $zk_broker 2181
echo isro | nc $zk_broker 2181
```

## Tools - KR

- https://github.com/fgeller/kt

### Install

```bash
brew tap fgeller/tap && brew install kt
```

### How to use

```bash
$ kt -help
kt is a tool for Kafka.

Usage:

        kt command [arguments]

The commands are:

        consume        consume messages.
        produce        produce messages.
        topic          topic information.
        group          consumer group information and modification.
        admin          basic cluster administration.

Use "kt [command] -help" for for information about the command.

Authentication:

Authentication with Kafka can be configured via a JSON file.
You can set the file name via an "-auth" flag to each command or
set it via the environment variable KT_AUTH.
```

* Consume topic

```bash
KT_BROKERS=b-1.domain.com,b-2.domain.com,b-3.domain.com kt consume -topic topic.name_001
```

## Tools - KAF

- https://github.com/birdayz/kaf

### Install

* Via install script

```bash
curl https://raw.githubusercontent.com/infinimesh/kaf/master/godownloader.sh | BINDIR=$HOME/bin bash
kaf config add-cluster local -b localhost:9092
kaf config select-cluster
```

* Via Go

```bash
go install github.com/birdayz/kaf/cmd/kaf@latest
```

* Via Brew

```bash
brew tap birdayz/kaf
brew install kaf
```

### Start Zookeeper and Kafka (if running locally)

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties
```

### Usage

```bash
$ kaf --help                  
Kafka Command Line utility for cluster management

Usage:
  kaf [command]

Available Commands:
  completion  Generate completion script for bash, zsh, fish or powershell
  config      Handle kaf configuration
  consume     Consume messages
  group       Display information about consumer groups.
  groups      List groups
  help        Help about any command
  node        Describe and List nodes
  nodes       List nodes in a cluster
  produce     Produce record. Reads data from stdin.
  query       Query topic by key
  topic       Create and describe topics.
  topics      List topics

Flags:
  -b, --brokers strings          Comma separated list of broker ip:port pairs
  -c, --cluster string           set a temporary current cluster
      --config string            config file (default is $HOME/.kaf/config)
  -h, --help                     help for kaf
      --schema-registry string   URL to a Confluent schema registry. Used for attempting to decode Avro-encoded messages
  -v, --verbose                  Whether to turn on sarama logging
      --version                  version for kaf

Use "kaf [command] --help" for more information about a command.
```

* List nodes

```bash
kaf node ls
```

* List topics

```bash
kaf topics
```

* Describe topics

```bash
kaf topics describe $topic_name
```

* List Consumer-groups

```bash
kaf groups
```

* Describe Consumer Group

```bash
kaf group describe $consumer_group_name
```

* Consume topic message from oldest

```bash
 kaf consume $topic_name --raw --offset oldest | grep "ID-TO-FILTER"
```

* Consume topic message from earliest

```bash
 ./kaf consume $topic_name --raw --offset oldest | grep "ID-TO-FILTER"
```

* Offset Reset to latest for all partitions

```bash
kaf group commit $consumer_group_name -t $topic_name --offset latest --all-partitions
```

* Set offset to oldest

```bash
kaf group commit $consumer_group_name -t $topic_name --offset oldest --all-partitions
```

* Set offset to 1001 for partition 0

```bash
kaf group commit $consumer_group_name -t $topic_name --offset 1001 --partition 0
```



