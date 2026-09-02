# Chapter 1 - Reliable, Scalable and Maintanable applications.

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


