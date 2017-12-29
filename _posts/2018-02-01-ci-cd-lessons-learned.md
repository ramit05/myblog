---
layout: post
title: "Lessons Learnt in the CI/CD Journey"
date: 2018-02-01
author: Ramit Surana
tags: Jenkins Continous Delivery Continous Integration Polling Syntax Blue Green Deployment
excerpt: "Some immportant lessons learnt in CI/CD Journey"
---

Hi Everyone,As martin Fowler correctly explains :
Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.
The primary goal of th process is to be production ready anytime and anywhere. During this journey, I had some reletive experiences and good practices that I developed. In this post I will be illustrating them in depth from a CI/CD admin perspective.
So hope you guys enjoy it !

1. Using Syntax Checker :

One of the most primary starting point in CI/CD is to write a file using which we describe how our jobs are handled in the pipeline. For various tools, there are many syntax validators. I found them really useful. Some of there tools are:

* [Jenkins](https://job-dsl.herokuapp.com/)
* [Gitlab]()

2. Avoid using Polling at all Costs

As correctly said by [Koshuke](http://kohsuke.org/2011/12/01/polling-must-die-triggering-jenkins-builds-from-a-git-hook/), it is important that we adopt new methods to trigger the pipelines.  During one my experiences I faced such a problem, with using the polling mechanism
We were running multiple pipelines using the polling mechanism for multiple repositories. 

3. Use proper checkout strategy

One of the mistakes that one can do while checking out multiple repositories in a pipeline is the fact that unintended commit on other repo might be triggering the pipeline. The best command to checkout the repo is this :

````
checkout(
poll: false,
scm: [
  $class: 'GitSCM', branches: [[name: '*/master']],
  userRemoteConfigs: [[
    url: MY_URL.git,
    credentialsId: CREDENTIALS_ID]],
  extensions: [
    [$class: 'DisableRemotePoll'],
    [$class: 'PathRestriction', excludedRegions: '', includedRegions: '*']]
])

````

4. Use Python for writing automation scripts

Although one might be puzzeled by this tip, since its one own preference to use any language one might prefer. But after working out for a few common languages such as gradle,bash etc. I feel confident saying that python helps you automate things in a much simpler and faster way than any language possible. 
Why ? Because python has simply better libraries than observed in any other language and its dynamic and OOPS feature makes it an incredible tool for parsing and automating stuff. One such common use case is while parsing json using pthon vs bash.
I find it vary easy to use python libraries for parsiing instead of using bash for any such use cases. Except the jq tool bash simply is unable to parse json.


## Conclusion

Hope you guys enjoyed the post and please share your side of the views in the comments too.Have a happy day !
