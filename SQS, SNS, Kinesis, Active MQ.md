 synchronous and asynchronous comm patterns
 - decouple app when synchronous becomes problematic from spikes, using:
	 - SQS - queue model
	 - SNS - pub/sub model
	 - Kinesis - real time streaming model
# SQS - Standard queues
- What's a queue?
	-  producer or multiple prod. send messages into queue - consumers poll messages from queue
	- SQS - standard queue, over 10 yrs old
	- fully managed, used to decouple applications
	- **anytime you see decouple in exam think SQS
	- SDK sendMessage API
	- message persisted until a consumer deletes\
• Oldest offering (over 10 years old)

• Fully managed service, used to decouple applications

• Attributes:  
• Unlimited throughput, unlimited number of messages in queue

• Default retention of messages: 4 days, maximum of 14 days • Low latency (<10 ms on publish and receive)  
• Limitation of 256KB per message sent

• Can have duplicate messages (at least once delivery, occasionally) • Can have out of order messages (best effort ordering)

SQS – Producing Messages

• Produced to SQS using the SDK (SendMessage API)  
• The message is persisted in SQS until a consumer deletes it • Message retention: default 4 days, up to 14 days

• Example: send an order to be processed • Order id

• Customer id  
• Any attributes you want

• SQS standard: unlimited throughput

SQS – Consuming Messages

• Consumers (running on EC2 instances, servers, or AWS Lambda)...  
• Poll SQS for messages (receive up to 10 messages at a time)  
• Process the messages (example: insert the message into an RDS database) • Delete the messages using the DeleteMessage API

SQS – Multiple EC2 Instances Consumers

SQS Queue

- Consumers receive and process messages in parallel
    
- At least once delivery
    
- Best-effort message ordering
    
- Consumers delete messages after processing them
    
- We can scale consumers horizontally to improve throughput of processing
Amazon SQS - Security

• Encryption:  
• In-flight encryption using HTTPS API

• At-rest encryption using KMS keys  
• Client-side encryption if the client wants to perform encryption/decryption itself

• Access Controls: IAM policies to regulate access to the SQS API

• SQS Access Policies (similar to S3 bucket policies)

- Useful for cross-account access to SQS queues
    
- Useful for allowing other services (SNS, S3...) to write to an SQS queue
### SQS – Message Visibility Timeout

• After a message is polled by a consumer, it becomes invisible to other consumers

• By default, the “message visibility timeout” is 30 seconds

• That means the message has 30 seconds to be processed

• After the message visibility timeout is over, the message is “visible” in SQS
• If a message is not processed within the visibility timeout, it will be processed twice • A consumer could call the ChangeMessageVisibility API to get more time  
• If visibility timeout is high (hours), and consumer crashes, re-processing will take time • If visibility timeout is too low (seconds), we may get duplicates
### Amazon SQS - Long Polling
- When a consumer requests messages from the queue, it can optionally “wait” for messages to arrive if there are none in the queue
    
- This is called Long Polling
    
- LongPolling decreases the number of API calls made to SQS while increasing the efficiency and reducing latency of your application
    
- The wait time can be between 1 sec to 20 sec (20 sec preferable)
- Long Polling is preferable to Short Polling
    
- Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds
### Amazon SQS – FIFO Queue
- • FIFO = First In First Out (ordering of messages in the queue)
- • Limited throughput: 300 msg/s without batching, 3000 msg/s with 
- Exactly-once send capability (by removing duplicates)  
• Messages are processed in order by the consumer

# Amazon SNS

• The “event producer” only sends message to one SNS topic  
• As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications • Each subscriber to the topic will get all the messages (note: new feature to filter messages)  
• Up to 12,500,000 subscriptions per topic  
• 100,000 topics limit

Amazon SNS – How to publish

• Topic Publish (using the SDK) • Create a topic

• Create a subscription (or many) • Publish to the topic

• Direct Publish (for mobile apps SDK)

- Create a platform application
    
- Create a platform endpoint
    
- Publish to the platform endpoint
    
- Works with Google GCM, Apple APNS, Amazon ADM...

Amazon SNS – Security

• Encryption:  
• In-flight encryption using HTTPS API

• At-rest encryption using KMS keys  
• Client-side encryption if the client wants to perform encryption/decryption itself

• Access Controls: IAM policies to regulate access to the SNS API

• SNS Access Policies (similar to S3 bucket policies)

- Useful for cross-account access to SNS topics
    
- Useful for allowing other services ( S3...) to write to an SNS topic
• Push once in SNS, receive in all SQS queues that are subscribers  
• Fully decoupled, no data loss  
• SQS allows for: data persistence, delayed processing and retries of work • Ability to add more SQS subscribers over time  
• Make sure your SQS queue access policy allows for SNS to write  
• Cross-Region Delivery: works with SQS Queues in other regions

# Kinesis Overview
- Makes it easy to collect, process, and analyze streaming data in real-time
    
- Ingest real-time data such as: Application logs, Metrics, Website clickstreams, IoT telemetry data...
    

- Kinesis Data Streams: capture, process, and store data streams
    
- Kinesis Data Firehose: load data streams into AWS data stores
    
- Kinesis Data Analytics: analyze data streams with SQL or Apache Flink
    
- Kinesis Video Streams: capture, process, and store video streams

• Retention between 1 day to 365 days  
• Ability to reprocess (replay) data  
• Once data is inserted in Kinesis, it can’t be deleted (immutability)  
• Data that shares the same partition goes to the same shard (ordering) • Producers: AWS SDK, Kinesis Producer Library (KPL), Kinesis Agent

• Consumers:  
• Write your own: Kinesis Client Library (KCL), AWS SDK  
• Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics,

• Provisioned mode:  
• You choose the number of shards provisioned, scale manually or using API

• Each shard gets 1MB/s in (or 1000 records per second)  
• Each shard gets 2MB/s out (classic or enhanced fan-out consumer) • You pay per shard provisioned per hour

• On-demand mode:

- No need to provision or manage the capacity
    
- Default capacity provisioned (4 MB/s in or 4000 records per second)
    
- Scales automatically based on observed throughput peak during the last 30 days
    
- Pay per stream per hour & data in/out per GB

- Control access / authorization using IAM policies
    
- Encryption in flight using HTTPS endpoints
    
- Encryption at rest using KMS
    
- You can implement encryption/decryption of data on client side (harder)
    
- VPC Endpoints available for Kinesis to access within VPC
    
- Monitor API calls using CloudTrail
# SQS vs SNS vs Kinesis

SQS:

- Consumer“pulldata”
    
- Dataisdeletedafterbeing
    
    consumed
    
- Canhaveasmanyworkers (consumers) as we want
    
- Noneedtoprovision throughput
    
- Orderingguaranteesonlyon FIFO queues
    
- Individualmessagedelay capability
    

SNS:

- Pushdatatomany subscribers
    
- Upto12,500,000subscribers
    
- Dataisnotpersisted(lostif
    
- Pub/Sub
    
- Upto100,000topics
    
- Noneedtoprovision throughput
    
- IntegrateswithSQSforfan- out architecture pattern
    
- FIFOcapabilityforSQSFIFO
    

not delivered)

Kinesis:

• Standard:pulldata • 2 MB per shard

• Enhanced-fanout:pushdata • 2 MB per shard per consumer

- Possibilitytoreplaydata
    
- Meantforreal-timebigdata,
    
    analytics and ETL
    
- Orderingattheshardlevel
    
- DataexpiresafterXdays
    
- Provisionedmodeoron- demand capacity mode
    
# Amazon MQ

- SQS, SNS are “cloud-native” services: proprietary protocols from AWS
    
- Traditional applications running from on-premises may use open protocols such as:MQTT,AMQP,STOMP,Openwire,WSS
    
- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ
    
- Amazon MQ is a managed message broker service for
    

- Amazon MQ doesn’t “scale” as much as SQS / SNS
    
- Amazon MQ runs on servers, can run in Multi-AZ with failover
    
- Amazon MQ has both queue feature (~SQS) and topic features (~SNS)