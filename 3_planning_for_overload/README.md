# 3 Building Open Source Applications


## Review Questions (Theory)

1. Name the common sources of overload in Erlang Systems
- Error logger exploding, locking and blocking operations, and unexpected messages. The error logger process could take a long while to log things to disk over the network(much more slowly than errors can be generated)
2. What are the two main classes of strategies to handle overload?
- Restricting input input and discarding data
- We can discard data by randomly dropping messages
3. How can long-running operations be made safer?
- Execute long-running tasks in seperate processes so that they can be allowed to crash/run without blocking other operations. We can also attach timeouts to these operations
4. When going synchronous, how should timeouts be chosen?
- We should select timeouts such that the timers at the edges of the system havea longer wait time then those within. The rationale is that the first timer will be at edge of the system but all the critical operations will be happening deep within it. This means that the timer at the edge of the system will need to have a longer wait time than those within
5. What is an alternative to having timeouts?
- We can use buffers to drop messages when a system is overloaded
6. When would you pick a queue buffer before a stack buffer?
- Queue buffers are a great alternative when you don't have stringent requirement of low latency 
- Both the stack buffer and the queue buffer provide control over the messages you get rid of.
- One would also pick a queue buffer when one needs the messages to be processed in the order that was submitted(e.g. there are lots of sequences of dependent tasks). A stack buffer doesn't guarantee that the messages are going to be processed in the order that they were submitted.

## Open-ended Questions(Practical)
1. What is a true *bottleneck*? How can you find it?
- A true bottleneck is a point where everything has been optimized and the system as a whole can't be optimized further. To find this point, we optimize everything to the best of our ability
- For instance, the book states an example where there are too many logs sent around and there's a bottleneck on databases that need the consistency or there's not enough knowledge or manpower in your organization to improve things there.
2. In an application that calls a third party API, response times vary by a lot depending on how healthy the other servers are. How could one design the system to prevent occasionally slow requests from blocking other concurrent calls to the same service?
- One can make asynchronous calls
3. What's likely to happen to new requests to an overloaded latency-sensitive service where data has backed up in a stack buffer? What about old requests?
- When the stack goes beyond a certain size or an element is kept waiting then all elements beyond a certain point in the stack will be dropped.
4. Explain how you could turn a load-shedding overload mechanism into one that can also provide back-pressure.
- Quick Recap: Load shedding refers to dropping messages when there is overload and backpressure refers to slowing down producers on overloaded
- So we could have a producer which produces messages in proportion to the amount of messages received and at periodic intervals it could send an update to the producer so that it could regulate message proudction accordingly. In short, this could be a circuit-breaker mechanism
5. Explain how you could turn a back-pressure mechanism into a load-shedding mechanism.
- So if we use a queue and then place a probe(See Fred Herbert on Handling Overload) on the queue to determine how much time it takes from entering the queue and leaving the queue and drop messages based on that message
6. What are the risks, for a user, when dropping or blocking a request? How can we prevent duplicate messages or missed ones?
- The request could be critical for downstream requests so by dropping it other requests can't function properly. We can implement exactly once processing semantics. The implementation is rather complicated though. 

7. What can you expect to happen to your API design if you forget to deal with overload, and suddenly need to add back-pressure or load-shedding to it?
- It will have to change significantly. Not sure what the more specific answer should be.



### Nice articles
1. [Fred Herbert on Overload](https://ferd.ca/queues-don-t-fix-overload.html)
2. [Fred Herbert on Handling Overload](https://ferd.ca/handling-overload.html)
3. [How Kafka Implements Exactly Once Processing](https://www.confluent.io/blog/exactly-once-semantics-are-possible-heres-how-apache-kafka-does-it/)
4. [Long running processes](https://stackoverflow.com/questions/8879177/erlang-gen-server-with-long-running-tasks)