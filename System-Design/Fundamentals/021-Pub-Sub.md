## What is the publish subscribe pattern?
This is a popular messaging model that consists of publishers and subscribers.

## What is a publisher?
A publisher publish messages to special topics aka channels without caring about or even knowing who will read those messages.

## What is a subscriber?
A subscriber is a component of the system that subscribes to specific topics and reads the messages coming through those topics.

## What is a message?
A message in this context is not a literal message like in a chat app. This could be a process, a string, a JSON - anything really. It just depends on the context of the system and what the publisher is sending out and what the subscriber is expecting to receive from the topic.

## What is a Topic?
A topic is a component of the pubsub system also known as a channel. In a nutshell, it receives messages from the publisher and stores those messages to a persistent storage. Subscribers subscribe to receive messages from a topic.

## What are the characteristics of a pub sub system?

- Persistent Storage at Topic Level
- At Least Once delivery:
Each topic is aware of the number of subscribers it has connected to it. When the messages has been sent to the subscriber, the subscriber will send an acknowledgement response to the topic and the topic will register that this message has been received by that subscriber. This means that as far as the topic is concerned, unless it receives an acknowledgement from the subscriber it will assume the subscriber has not gotten the message and will resend it.
- Compulsorily Idempotent Because of Not exactly once delivery:
Since the Topic will keep resending the message to a subscriber until it gets an acknowledgement response there is every tendency that a message will be sent more than once to the subscriber. This is a problem if the process contained within the message is not an Idempotent one. 

An operation that has the same outcome regardless of how many times it is performed is an Idempotent operation. If an operation can be performed multiple times without changing its overall effect, its idempotent. Operations performed through a pub/Sub messaging system have to be idempotent, since pub/Sub systems tend to allow the same messages to be consumed (send or published) multiple times.

- Queue Based Ordered Messages: The topic acts like a queue data system where the first item in is also the first item to go out.
- Message Replay?
Message replay allows brokers to resend messages to new or existing clients that request them hours (or even days) after those messages were first received by the event broker.

## What is an Event Broker:
 Publishers just need to know what topic to send an event to, and the event broker takes care of delivery to systems that need it.

## Why Do We Have Multiple topics?
We have multiple topic to manage different situation. This could be because the different topics manage different process types.

Example of pubsub tools: Google Cloud PubSub, Apache Kafka

There are additional features that come with third party pub sub tools:
- Subscribers can Have Content Based Filtering
- E2E Encryption Can also be added based on the third party provider.

Remember To create pubSub code Sample

BEGIN TRANSACTION


SELECT * from Jobs Where Status = "Queued" Order By CreatedAt ASC Limit 1 as X

If X is null
    Rollback;

Update Jobs Set Status = "Running" where x.Id == id

Commit