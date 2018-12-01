# Kinesis Consumer in Python

A kinesis consumer is purely written in python. This is a lightweight wrapper 
on top of AWS python library [boto3](https://github.com/boto/boto3). You also can 
consume records from Kinesis Data Stream (KDS) via: 

* Lambda function: I have a demo [kinesis-lambda-sqs-demo](https://github.com/HengfengLi/kinesis-lambda-sqs-demo)
showing how to consume records in a serverless and real-time way. 
* [Kinesis Firehose](https://aws.amazon.com/kinesis/firehose/): This is a AWS managed service and easily save records
into different sinks, like S3, ElasticSearch, Redshift. 

## Installation

Install the package via `pip`: 
```bash
pip install kcpy
```

## Getting started

```python
from kcpy import StreamConsumer
consumer = StreamConsumer('my_stream_name')
for record in consumer:
    print(record)
```

The output would look like:

```bash
{
    'ApproximateArrivalTimestamp': datetime.datetime(2018, 11, 13, 11, 57, 55, 117807), 
    'Data': b'Jessica Walter', 
    'PartitionKey': 'Jessica Walter', 
    'SequenceNumber': '1'
}
```

Or, you can consume stream data with checkpointing: 

```python
from kcpy import StreamConsumer
consumer = StreamConsumer('my_stream_name', consumer_name='my_consumer', checkpoint=True)
for record in consumer:
    print(record)
```

## Checkpointing

Below shows the schema of checkpointing: 

```
+---------------+-------------+----------+--------+
| consumer_name | stream_name | shard_id | seq_no |
+---------------+-------------+----------+--------+
| consumer_1    | stream_1    | shard_1  |   5    |
| consumer_1    | stream_1    | shard_2  |   15   |
| consumer_1    | stream_1    | ...      |   15   |
| consumer_1    | stream_1    | shard_N  |   XX   |
| consumer_2    | stream_1    | shard_1  |   6    |
+---------------+-------------+----------+--------+
```

## Features

* Read records from a stream with multiple shards
* Save checkpoint for each shard consumer for a stream

## Todo

* Support other storage solutions (mysql, dynamodb, redis, etc.) for checkpointing  
* Rebalance when the number of shards changes
* Allow kcpy to run on multiple machines

## Changelog

### 0.1.5

* Add consumer checkpointing with a simple sqlite storage solution. 

### 0.1.4

* Pass aws configurations into boto3 client directly. 

### 0.1.3

* Update the README. 

### 0.1.2

* Add markdown support for long description. 

### 0.1.1

* Add a long description.

### 0.1.0

* First version of kcpy.  

## License

Copyright (c) 2018 Hengfeng Li. It is free software, and may
be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: /LICENSE
