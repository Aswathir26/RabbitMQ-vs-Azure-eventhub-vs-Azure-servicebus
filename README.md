RabbitMQ vs Azure event hub vs Azure service bus

Overview
RabbitMQ, Azure event hub and Azure service are the services which almost have same usage. RabbitMQ is a message broker. It accepts and forwards messages from a sender/ publisher to consumer/ receiver through queues. We can use RabbitMQ to Scale image (Image scaling task is added to RabbitMQ and image scaling service get the task from queue and scale it and make its correct size image available), Scan file, Create DB backup, ...

Azure event hub focuses more on event streaming. It receives and processes millions of events per second. Data sent to an event hub can be transformed and stored by using any storage adapters or real-time analytics provider. Event is a notification that data has been processed and some objects’ state has changed. We can use event hub for Telemetry and distributed data streaming.

Azure Service Bus is a messaging service on cloud used to connect any applications, devices, and services running in the cloud to any other applications or services. We can use service bus for Order processing and financial transactions.

Messaging Concepts / Features
There are lots of features in RabbitMQ. We can Send and receive a message from a named queue, distribute time-consuming tasks among multiple workers through Work Queue, send messages to many consumers at once – Publish/Subscribe, make Routing - receiving messages selectively, receive messages based on a pattern, Build a Remote Procedure Call system: it is an inter process communication technique that is used for client-server based applications- A client has a request message that the RPC translates and sends to the server, Use publisher confirms to make sure published messages have safely reached the broker.

Azure service bus also have some features: we can schedule a job to become available for processing at a certain time, it supports a dead-letter queue  to hold messages that cannot be delivered to any receiver, message ordering or message sessions (first in first out) it enable joint and ordered handling of unbounded sequences of related messages, duplicate detection of messages, auto delete idle for queue, Topics and subscription allowing subscribers to select particular messages from a published message stream, filter messages, The auto-forwarding feature enables to remove message from a queue or subscription and placed to another queue or topic that is part of the same namespace, Message deferral means we can hold messages in queue if we are busy. Client-side batching enables a queue or topic client to delay sending a message for a certain period of time

Event Hubs lacks all these features because it serves the sole purpose of allowing the event platform to handle the large volume of messages.

TTL/ Retention
RabbitMQ and azure service allows to set Time to live for both messages and queues. Message TTL can be applied to a single queue, a group of queues or applied on the message-by-message basis.

But There is no specific Time to live for an event but there is a retention policy on the hub. The default value and shortest possible
retention period is 1 day (24 hours). For Event Hubs Standard,
the maximum retention period is 7 days. For Event Hubs Premium and Dedicated, the maximum retention period is 90 days. If you change the retention period, it applies to all messages including messages that are already in the event hub.

Message Reply
RabbitMQ and Azure service bus do not have the capability of message reply.

But event hub has. Since the messages are not removed from the stream by the receiver The messages in the stream can be processed by simply adjusting the index that the receiver points to.

In RabbitMQ However, a client can acknowledgement a message when it fails to handle the message The message will simply be added back to the queue.

Poison Message
There is a chance for the generation of messages that cannot be processed. It is called poison message.

In RabbitMQ Poison message can be handle by using the Quorum Queues. So that it can be marked for deletion by RabbitMQ.

In Event Hubs we can’t remove a corrupted message from the stream. It must be processed like every other message in the stream.

Like that in Azure Service Bus, these messages can either be sent to dead lettered queue or abandoned.

Scaling
By using sharding plugin and clustering we can scale up RabbitMQ.

In case of event hub we will need to increase the partitions and assign a single processor for each partition.

In case of service bus, we can scale up the receivers of a message by using Competing consumer pattern.

Integration
RabbitMQ can integrate with Springboot, Azure VM.

Event Hubs can integrate with Azure SQL DB, Azure Storage, Stream analytics, Power BI, Machine learning.

Service Bus can integrate with Azure Functions, Azure Logic Apps, Azure Event Grid, Stream Analytics, Dynamic 365

Size limit of Message/ Event hub publication
Theoretical message size limit in RabbitMQ is 2GB up to 3.7. 0 version (recommended size-128mb).

In service bus Message size limit is 256 KB. For premium it is 1MB.

Eventhub publication size for basic is 256 KB. For premium it is 1MB.

Maximum Number of Messages/ Consumer groups

In RabbitMQ maximum number of messages that can held in a queue is 50000.

But there is no that kind of limit in service bus.

Number of consumer groups per event hub: 1 for premium: 100

Cost
RabbitMQ is a free source. So there is no charging. But azure eventhub and service bus are not free. Pricing tiers: basic, standard and premium.