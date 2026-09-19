# Chapter 1 - Reliable, Scalable and Maintanable applications.

[Reliability](##Reliability)

[Scalability](##Scalability)

[Maintanability](##Maintanability)



## Intro

A data intensive application is build from smaller blocks, these blocks provide commonly needed functionality. The challenge is to know which tools are accessible and know which ones are the ones we need for our applications. There are multiple database systems but one can be more beneficial than the other.

Some of the new tools that have been emerging on the recent years no longer fit under one category some may be queues that ofer durability similar to a database such as `Apache Kafka` or data stores that can behave as message queues as `Redis`.

Applications now need a wide range of tools to achieve their goal and it's the responsibility of the application code to hold all the pieces together i.e invalidating caches, making sure to store values into database, processing messages or events from a queue.

Once we create these systems to provide an API (Application Programming Interface) we are essentially creating a system and hidding the implementation from the consumers, however now we need to make sure our system provides certain guarantees. For example invalidating caches when a new entry is modified, providing good performance, scaling the application based on traffic, etc.

The 3 main concerns we care about are?

- Reliability -  can continue to work even if some adversities happen

- Scalability - should be able to deal with growth in traffic/users

- Maintainability - people should be able to work on the system productively

## Reliability

"Continue to work correctly even if things go wrong"

- Performs expected functions

- Tolerates user mistakes (inputs etc)

- Acceptable performance

- Prevents unathorized access or abuse

**Faults** are things that can go wrong, one component of the system deviating from its spec

**Failure** system as a whole stops providing the required service to the user

> Since it's imposible to reduce the probability of faults to 0% our goal is to design systems that are 'fault-tolerant' that prevent these faults from causing failures 

### Hardware faults

#### Examples

- RAM failing

- Disk dying on a data center due to it's mean time to failure

- Someone disconnecting electricity

#### Responses

- Hot-Swappable CPU

- Diesel generator

- Batteries

- Dual power supplies

- RAID systems

Redundancy of components used to be sufficient, however now that services require larger number of machines such as cloud instances there is a move towards designing systems that can tolerate the loss of entire machines. This brings benefits such as allowing do release code on batches, updating  application nodes one by one to prevent down time etc.

### Software faults

#### Examples

- Software bug leap year on 2012

- Process consumming lots of memory affecting the system

- Service we depend on starts to slow down

- Cascading failures, one small fault causes issues on other services

#### Responses

It's hard to prepare for this type of fauls since they may remain dormant for long time what we can do is careful thinking on assumptions i.e coding expecting we always get a reponse from system A, we can also allowing processes to crash/restart to retry, monitoring the system and even doing some healthchecks on the systems to identify if certain guarantees are working e.g if I publish a message to this queue it should have exactly one message.

### Human faults

Systems are designed by humans, and humans are unrealiable they make mistakes infact more than actual hardware errors 

#### Responses

- Create abstractions so we can minimize opportunities for errors

- Decouple places in which errors can happen i.e create a UAT env

- Test all levels, unit tests, integration tests and manual

- Allow quick en easy recovery from errors, e.g rollback config changes, release code gradually to prevent reverts be able to rollback computations

- Setup monitoring `telemetry`

- Implement safe practices for SDLC and training 

> [!IMPORTANT]
> 
> Reliability is important since outages can reprecent financial loss or damage to reputation



---



## Scalability

The ability of a system to cope with load, this means that the system can handle increases in load. 

### Describing load

Systems can describe load in multiple ways it could be writes per second, read per second, ratio of reads to writes, simultaneous users in a chat. 

Numbers used to describe load are called `load parameters`

> [!IMPORTANT]
> 
> Sometimes the scaling challenges are not hard due to the volume they need to handle but more to the `fan-out` 
> 
> Similar to Twitter fan-out problem for tweets and timelines and users with lots of followers

#### Twitter homepage

##### Option 1

Perform a query to retrieve all the posts from the people you follow, i.e select * from tweets of people where the follower is me

> This option is the one that Twitter had on its 1st version, however when the load increased the system had issues to keep up with the amount of queries done to create people feeds

##### Option 2

This separate option was to keep a cache for each user feeds and when a user posts the system looks at all the people following that user and inserts the post into its feed.

> This means that the posts are more writte heavy than before, however it was discovered to be better since reads are multiple orders of magnitude larger than writtes.

However as someone will imagine no design is perfect and there could be superstar users with millions of followers if, if we followed the approach described in `option 2` this means we will write into million of cache feeds, for this Twitter created an hybrid system and for these type of users ir fetches the posts as done in `option 1`

### Describing Performance

- When you increase the load, and keep the same system resources how is the performance of your system?

- When you increase the load how much you need to increase the resources to keep the performance unchanged

`Latency` and `Response time` are not the same, the response time is what the client sees (including network time, processing etc), and the latency is the time it's request took to be serviced.

Mean == Average

Median = p50 

> If we say we have a median response time of 200ms that means that half of the requests are less than 200 and the other half over.

In an analysys of 100 requests a 95th percentile of 1.5 sec would mean that 95 requests of 100 are on less than 1.5 secs and the other 5 over.

Percentiles are often used to describe SLO (Service level objectives) and SLA (Service level agreements)

> And SLA may consider a service is up and running if it has median response time of 200ms and a 95th percentile of under 1s

It only takes a small number of slow requests to hold up the processing of subsequent requests => `head of line blocking`

*Tail latency amplification* the more requests that are needed to fulfil a user need the larger the chance one request can be slow and delay the whole thing.

### Approaches to cope with load

- An application that is appropiate for one level of load is unlikely to do well if we increase that load 10x

- Good architectures often involve a pragmatic approach of doing a mix between large instance and several smaller.

- Some systems are *elastic* meaning that they can add resources as the load increases

> A system that handles 100,000 requests per second of 1KB each one looks very different than a system that handles 3 request per minute of 2GB in size, even that both work with the same data through-put

## Maintanability

Most cost of software is spent on maintenance this involes fixing bugs, maintaning the system in operation adding new functionality, fixing technical debt.

There are 3 principes that we can use to help on this area:

- Operability
  
  - Make it easy for operations to keep the system running
  
  - Create tools that can monitor the health of the system, review how multiple systems interact 
  
  - Tools for deployment
  
  - Maintaning systems security patches

- Simplicity
  
  - Avoid un-neded complexity, make it easy for others to work on the code and prevent unwanted completixy, create good abstractions so people code to them rather to try to work around them

- Evolvability
  
  - Use well defined tools to extend the capabilities of the system as Agile
