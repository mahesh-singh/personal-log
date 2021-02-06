+++
author = "Mahesh Singh"
title = "Chapter01 - Reliable Scalable and Maintainable Applications"
date = "2021-01-22"
description = "Reliable Scalable and Maintainable Applications"
tags = [
    "ddia", "notes",
]
+++

Whenever I read a book, I try to underline things and later try to convert into digital notes. It almost become two phase revision of the book. 
These notes are from Martin Kleppmann's book: Designing Data-Intensive Applications. 

Disclaimers:

* These notes are not a substitute of the book. 
* These notes are from my understanding of the topic and typed manually (Yes I prefer physical book over digital one) . There might be the case when I missed something.
  

## Table of content
* Chapter 01 - Reliable Scalable and Maintainable Applications
* (Chapter 02 - Data Model and Query Language)[./data-model-and-query-language/]
* Chapter 03 - Storage and Retrieval (coming soon...)


# Chapter 01 - Reliable, scalable, and maintainable applications

Following are the building blocks which use by typical data intensive applications

* Store data in database
* Caching data for remembering pre-compute result
* Indexes for faster search
* Messaging for async processing by other applications 
* Batch processing for crunching large amount of data in background

In general database, messaging system, caching all store data but they all serves different purpose. There are new systems which again blurring the boundaries like redis which store data on the same time its also provide queuing while Apache Kafka is a messaging queue which provide database like durability guaranty.

Important concerns for most of the software system

**Reliability** - System should work as per expected even when it face some kind failures like hardware/software fault, human error etc.

**Scalability** - As system grows there should be reasonable way to handle the growth.

**Maintainability** -  As system getting old lots of people contribute to the system. As system getting older, adding changes should be productive.  

## Reliability

> Continue to work correctly, even when things go wrong.

**Fault** - When things can't work as expected.

**Fault tolerance (or resilience)** - When system handle expected faults called fault tolerance.  

**Failure** - Failure are different from fault, when an entire system stop working that's called failure where fault is a system misbehave in certain condition.

To test system's fault tolerance, deliberately introduce faults in system like randomly killing individual process and check impact on other systems.

**Hardware fault** 
- Reduce by adding redundancy in hardware. 
- Use software fault-tolerance techniques or use in addition to hardware redundancy.
**Software errors**: Software errors normally have cascading effect and error in one system cause error in another system.
- Measure, monitor and analyze system behavior in production.

**Human error**: Configuration error by operators is leading cause of outrage.
- Design system in a way that minimize opportunity for error.
- Test, test and test from unit test to integration test to manual testing.
- Quick and easy recovery from human error like rollback of configuration changes to roll out new code gradually.
- Set up detail monitoring (performance, error rate). This is also called telemetry.

## Scalability
> System's ability to cope with increased load.

Scalability means considering questions like 
- "If the system grows in a particular way, what are the options for coping with growth"
- "How can we add computing resources to handle the additional load?"

### Describing load

**Load parameter** Load can be describe with few numbers. 

* Request per second for web server
* Response time
* Ratio of read to write in DB
* Number of simultaneous active uses in chat room
* Hit rate on a cache  

Twitter example: 

* Post tweet: 4.6k request/second on average, over 12k request/sec at peak 
* Home timeline: 300k request/sec

### Describing performance

- When you increase a load parameter and the keep the system resources unchanged, how the performance of your system affected?
- When you increase the load parameter, how much resources you need to increase if you want to keep performance unchanged?

- Note: Batch processing systems like hadoop you care about *throughput* (number of record process per second). For web system you care about response time. 


Note: ***Latency and response time:*** Response time is what client see. Latency is the duration that a request is waiting to handle.

Even making same request again and again, you'll get a slightly different response time every time. That's why response time is not a single number but distributed values that you can measure.

Average or mean response time is not a good metrics as it doesn't tell how many users actually experience this delay.

**Percentiles:**  If 95th percentile response time is 1.5 sec than means 95 out 100 request take less then 1.5 second and 5 out of 100 request take more than 1.5 seconds.

Ex. Amazon describe requirement of response time of internal services in term of 99.9th percentile though it only affects 1 in 1000 request but customers with the slowest request have are often those customers who have most data and they're the most valuable customers.

Requests could be fast individually but one slow request could slow down all the other requests. It takes just one slow call to make the entire end-user request slow. Tail latency amplification.

Percentiles are often used in SLOs (Service Level Objectives) and SLAs (Service Level Agreements)

### Approach to coping with Load

- Scaling up (vertical scaling)
- Scaling out (horizontal scaling)

*elastic* means they can automatically add computing resource when they detect a load increased. They are useful if load is highly unpredictable.

Manual scaled system are simpler and may have fewer operational surprises.

There is no such thing as a generic, one size fit for all scalable architecture. The problem may be the volume of reads, the volumes of write, the volume of data to store, the complexity of the data, the response time requirements, the access pattern, or usually some mixture of all od these plus many more issues.

Ex. a system design to handle 100,000 request/sec each in 1kb in size, looks very different form a system that is design for 3 request/min each 2 GB in size, even though the two system have the same data throughput.



## Maintainability 
> Majority of the cost of the system is not in its initial development but ongoing maintenance.

Design principle for software systems

* Operability: Make it easy for operation team.
  * Visibility of runtime behavior
  * Documentation
* Simplicity: Make it simple for new engineer in team.
* Evolvability: Make it easy to make changes in system for future requirement.


