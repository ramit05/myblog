---
layout: post
title: "The Need of SRE"
date: 2017-09-07
author: Ramit Surana
tags: cloud sre aws gcp
category: sre cloud
excerpt: "Building SRE Practices for Future"
---

Minasan, kon'nichiwa (Japense)(In English - Hello Friends). Hope you folks are having an awesome day at work ! Recently I was watching a movie called as Deepwater Horizon.BTW Awesome Movie.The movie stars mark Walburg as a          on a oil cruise field called as DeepWater Horizon.Till the half-interval the movie shows the routine job they had everyday.But there one mistake costs them th damage they could not imagine.

## The True Disaster

Imagine a situation where you have started your day with coffee (Of Course !) and something fails.You begin checking for the mistakes one can experience while monitoring using your special set of tools.After a while you find the bug sitting somewhere inside some piece of code or something that blocks or brokes the continous pipleline that you and your team had laid out.You start fixing that,it's simple right ? 

Imagine another situation when you are in the mid interval session of your day job and your provider's entire infrastructure goes off.At first you are surprised and check if you or anyone on your team folks have made any mistakes.You first ran through every one of obvious tests and check every logical assumptions you have in mind.But the problems don't get any better.Meanwhile you start getting calls from around the office to ask why are the services they use not responding ? You feel frustrated but try to keep fixing the problem.Suddenly someone comes along gives you the information that the region you selected from your cloud provider has been down globally.

"Oh my ..", I believe these are your first words.You start planning your descisions accordingly,like transferring some of your services to other regions or starting to use some of the replacement based services that you have used earlier sometime before.Now I believe, if you have watched star wars, you might be thinking yourself as Captain Kirk and what would he had done,in your situation.This is typical stuff no engineer has been ever been trained for.



## How to prevent such incidents ?

Infrastructure disasters are real.Unfortunately,no government based special forces are going to come to your way to help you out.You are on your own.This is your battle.Now the disaster which I described above can happen due to a lot of reasons,DDoS(Denial of Service Attack),cloud provider's fault etc.But the real question still lies how to prevent such huge scale disasters from ever happening ? I think the answer to this sort of problem which enterprises have started to adopt is forming a special team of hanfull of engineers who are trained to handle such type of stuff.These engineers are your rescue guys to help you tackle such situations.Let's call them SRE(Site Reliability Engineers).


## The SRE Guys 

In the past few years there are quite few yet such elite teams who are there on your side to help you get the best out of these worse case conditions.According to my opinion SRE teams at Google and Netflix,are amongst the best I know of.They hire the people and train them to become the best of best.Every company has its own history of failures for several reasons.The company after resolving such type of huge issues often tend to keep a tabloid of information of the occurance of such type of incidents.Due the company's public image reasons.These are not made public until and unless the upper management approves of it.Over the year's these practices have been made public.Below down I am going to discuss some of these famous practices and how to test wheather your current infrastrcture is googd enough against them or not.

## The Chaos Monkey

It is one of the practice started by Netflix.It is made public as an open source project by 

## Conclusion

From the way of it,I believe in the upcoming days we can start by calling our companies as Infrastructure Goverments.(It will become like star trek with cruise ship and stuff).
Who knows we might end up calling software engineers as warriors some day.
