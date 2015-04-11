# Evolution of distributed data processing

This post is the second of my series of understanding the concept of distributed systems. The first post of the series was a short introduction to distrbuted systems. It is written in Norwegian so you might want to use Google Translate if you do not know the language. 

## An intro to what data processing is in a distributed context.

### What is the difference between processing data on a single machine vs multiple? 
### What are the different techniques used for distributed processing?
### What are the most common issues related to distributed processing? 
### Why use a framework? Why not build your own? Why is a simple model important?

Notes: 
* Data size
* Processing gurantees: none, at-least once, at-most once 

## Batch based processing
### What is batch based processing? 
Batch processing of data is the most basic processing technique. The model assumes that all data to be processed is available at the time of start of the batch processing job. That means data to arrive after the a processing job has started cannot be included in the currently running job. 

### What is it used for? 
Because of MapReduce the batch processing scheme is both historically and generally used for many distributed data processing problems. Batch processing is very useful for solving problems where a complete data set is curcial or the result needs to be accurate. 

### Where did it come from? 
Batch processing is the classic way of processing data. It comes from when data was small enough to be able to process on a single machine. Batch based distributed processing was first introduced by Google with the introduction of MapReduce in 2004 [MapReduce]. Later Yahoo popularized the MapReduce model when they open-sourced Hadoop [HadoopWiki]. Hadoop is widely used and many frameworks are built for and upon Hadoop such as Apache, Apache Zookeeper, Apache Pig, and lastly Apache Spark which we will discuss later. 

### What advantages does it how?
A great advantage with batch based processing that it is very easy to reason about. One reason is that being not having to consider incoming data is great. Another is with models/frameworks such as MapReduce the developer does not have to know about the distributed infrastructure. 

### What clear disadvantages does it have? 
If you are dependent upon newly arrived data. Then the batch processing paradigm is clearly not the approach for you. Then a you might consider stream based or micro-batch processing instead, usually sacrificing accuracy for speed. 

### MapReduce Model

#### Introduction
#### Concepts
#### Infrastructure


Notes: 
* Hadoop, 
* MapReduce, 
* Complete data set, 
* Offline

## Stream based processing
### Where did needs come from? 
### What is it used for? 
### What was the different approaches?

### Storm

#### Introduction

#### Concepts
* Spout
* Bolt
* Topology

### Infrastructure 


Notes: 
* MapReduce Streaming Scientific Community, 
* Storm by Backtype (Nathan Marz), 
* Online/Real-time, 
* Incomplete data set,
* At-least once 

## Micro batch based processing
### Where did it come from? 
### What does it solve that is not solved by batch or stream based processing? 

### Apache Storm Trident
### Apache Spark with Microbatching

Notes:
* Spark,
* Storm Trident,
* At-most once 


## Bringing it all together
What should you use? How can they be combined? Any readings to recommned?

Notes: 
* Lambda Architecture
