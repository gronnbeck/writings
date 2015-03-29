# Evolution of distributed data processing

This post is the second of my series of understanding the concept of distributed systems. 
The first post of the series was a short introduction to distrbuted systems. It is written in
Norwegian so you might want to use Google Translate if you do not know the language. 

Write intro to what data processing is in a distributed context.

What is the difference between processing data on a single machine vs multiple? 
What are the different techniques used for distributed processing?
What are the most common issues related to distributed processing? 
Why use a framework? Why not build your own? Why is a simple model important?

Notes: 
* Data size
* Processing gurantees: none, at-least once, at-most once 

## Batch based processing
What is batch based processing? What is it used for? Where did it come from? What advantages does it how?
What clear disadvantages does it have? 

Notes: 
* Hadoop, 
* MapReduce, 
* Complete data set, 
* Offline

## Stream based processing
Where did needs come from? What is it used for? What was the different approaches? 

Notes: 
* MapReduce Streaming Scientific Community, 
* Storm by Backtype (Nathan Marz), 
* Online/Real-time, 
* Incomplete data set,
* At-least once 

## Micro batch based processing
Where did it come from? What does it solve that is not solved by batch or stream based processing? 

Notes:
* Spark,
* Storm Trident,
* At-most once 


## Bringing it all together
What should you use? How can they be combined? Any readings to recommned?

Notes: 
* Lambda Architecture
