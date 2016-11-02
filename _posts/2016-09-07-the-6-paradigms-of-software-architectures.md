---
layout: post
title: "The 6 paradigms of software architectures"
date: 2016-09-07
author: Ramit Surana
tags: software architecture serverless microservices
category: software architectures
excerpt: "Exploring different types of enterprise class software architectures"
---

The software world is revolutionizing like anything.The need of people from delivering solutions have changed.Priorities of software companies have changed.The need of the Hour relies on scalability,resillency and delivery.

![different-arch](https://cloud.githubusercontent.com/assets/8342133/19617754/61667bb8-9856-11e6-82f0-0d548a452e30.png)


## Java EE Architecture

The beauty of any software architecture is dependent on the ubiquity of the language used for designing architectures.I think that the very first mature step in the direction of building enterprise class architectures was done by [Java]().The concept of such type of architectures is 

A sample figure of Java EE based Architecture

![overview-full](https://cloud.githubusercontent.com/assets/8342133/19943059/e599a97e-a15b-11e6-97a0-aabe1cf3b2a6.png)

## Monolith Architecture

When the need of new 

A sample figure of Monolith based Architecture

![richardson-microservices-part1-1_monolithic-architecture](https://cloud.githubusercontent.com/assets/8342133/19943135/28e6e372-a15c-11e6-83c5-0ca2bc6b2fdc.png)

## Microservices Architecture

The needs of customers have pursued us as to tackle complexities and transform into simplicities 

A sample figure of Microservices Architecture

![richardson-microservices-part3-monolith-vs-microservices](https://cloud.githubusercontent.com/assets/8342133/19943184/4f9c0f92-a15c-11e6-9b7b-0bee57b0cd62.png)

### Serverless Architecture

This type of architecture has been in the market for very less no. of years.It was first introduced by [Amazon Web Services]
(https://aws.amazon.com/) in 2014.Currently a lot of the development has been observed in this type of architecture.There has been a lot of different frameworks and organizations being developed in the recent couple of years.One such service is the [Lambda Service](https://aws.amazon.com/lambda/) by AWS.There are different types of services by different types of cloud service providers:

**Lambda Service - AWS**
**Google Functions - Google Cloud Platforms(https://cloud.google.com/functions/docs/)**
**Azure Functions - Azure(https://azure.microsoft.com/en-in/services/functions/)**
**OpenWhisk - IBM Bluemix(https://developer.ibm.com/openwhisk/)**

The serverless architecture is usually seen as implemented in [nodejs]().But there are several different languages such as Python,Go etc which can be implemented by using various different frameworks.Some of these frameworks are 

<img width="688" alt="serverless-app" src="https://cloud.githubusercontent.com/assets/8342133/19942695/a5c9156a-a15a-11e6-8885-23d918c71b57.png">


### Reactive Architecture

Micoservices are highly revolutionised on a concept of [Domain Driven Design](http://domainlanguage.com) by [Eric Evans](https://twitter.com/ericevans0?lang=en).One of the core concepts of Domain Driven Design is Bounded Contexts.When we look at it from mathematics point of view,it reveals us that some form of  can be .A lot of great work has been done in this direction by [Debainsh Ghosh](https://twitter.com/debasishg).In his series of [blog posts](http://debasishg.blogspot.in/), he clearly explains how we can simulate these conditions and make it better.One of the nice things that I really like about this architecture is that it eases up the process of transforming huge enterprises to a more combustile and functional type.

One of the languages that this architecture has been implemented is [Scala](http://www.scala-lang.org/).The scala language is implemented over [Java]().The beauty of this language is the clarity and much less code than java to implement some task in hand.It has been popular in various big data based solutions.

![sapr_0201](https://cloud.githubusercontent.com/assets/8342133/19943645/51acc964-a15e-11e6-9a47-435f00f1ce69.jpg)


### Lambda Architecture

This awesome [architecture](http://lambda-architecture.net/) has been built by [Nathan Marz](https://twitter.com/nathanmarz) and [Michael Hausenblas](http://mhausenblas.info/).In the famous article on [How to beat CAP Theorem ?](http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html) by [Nathan Marz](https://twitter.com/nathanmarz).He explains a methodology using which huge databases architectures can be able to .The main idea of the Lambda Architecture is to build Big Data systems as a series of layers.Each layer satisfies a subset of the properties and builds upon the functionality provided by the layers beneath it.( Riak and Cassandra )

![enterprise-lambda-architecture](https://cloud.githubusercontent.com/assets/8342133/19943306/d32a3032-a15c-11e6-8cec-61118add9d2d.jpg)

## Conclusion

At the end,I feel that every type of software architecture has its own benefits and losses.There is no stop to evolution when it comes to the architecture world.	
