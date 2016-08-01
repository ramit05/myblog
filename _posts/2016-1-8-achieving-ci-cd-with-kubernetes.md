---
layout: post
title: "Achieving CI/CD with Kubernetes"
date: 2016-09-07
author: Ramit Surana
tags: kubernetes jenkins docker
category: containers jenkins docker
excerpt: "Building CI/CD pipeline with jenkins & kubernetes"
---

CI/CD is an amazing concept.But as many of you might know,it is a lot harder that said done.In terms of Jeff Humble,it can be defined as
""
With the advancement of time and evolution of kubernetes we want 

**WARNING** Your machine may hang several times while performing the below steps.

## Methodology

There are many methodology using which we can achieve the above on our machine,some of these are

* [Kubernetes-Jenkins Plugin](#kubernetes-jenkins-plugin)
* [Fabric8](#fabric8)
* [Using Helm with Kubernetes](https://medium.com/@thepauleh/scalable-jenkins-with-helm-and-kubernetes-207df883ac9a)

and many more ...

## Overview of Architecture

Before Starting the work,first let's analyze the workflow required to start  using [docker][6] images with [docker][6].


Before starting to use jenkins and kubernetes you need to follow these steps:

![k8s-jenkins-docker](https://cloud.githubusercontent.com/assets/8342133/16848715/ba1473ea-4a14-11e6-99c2-cf8899c351a9.png)


Similarly while using [rkt][] containers a.k.a rktnetes.Here's the architecture:

![k8s-jenkins-rkt](https://cloud.githubusercontent.com/assets/8342133/16848756/ef5afe52-4a14-11e6-8723-a251b0b5c675.png)

*Currently there is no plugin supported rkt containers.But I assume that the workflow will remailn the same after its due implementation. * 

## Kubernetes-Jenkins Plugin

**Setting up Kubernetes on Host Machine**

Setting up kubernetes on your host machine is an easy task.In case you wish to try out on your local machine I would recommend you to try out [minikube][8].In case you wishe to the best of kubernetes,please follow the steps below:

<script src="https://gist.github.com/ramitsurana/21a137d1007980cadb98253b43a35ead.js"></script>

An amazing work in this direction has been done by [carlossg]().He has built a kubernetes plugin for jenkins.using this plugin you can easily start using jenkins with kubernetes directly.Also to provide users with more easy options to configure.He has built a jenkins image which by default contains kubernetes plugin.This image is available at [docker hub]().In the next steps we are going to fetch this image from docker hub and create a volume /var/jenkins_home for storing all your jenkins data.

## One Problem

Although we are doing everything as we want to do.We still run into a problem,you will notice that whenever you are about to restart your jenkins container after closing it down.All your data is lost.Whatever you did like creating jobs,installing plugins etc. will be lost.This is one of the common problems with containers.Let's discuss it in a bit depth:

### A word about Data Containers

Data is a tricky concept when it comes containers.When you first try to install and run the jenkins container,you may find that the plugins you installed are not visible .There are many ways to deal with such a problem.One is to use docker volumes.But I found it not that useful due to some reasons.One of the ways I found to deal with persistent storage is to crete another container and use it as a source of dynamic data instead of depending on one image.

![data-containers](https://cloud.githubusercontent.com/assets/8342133/16850837/6f36ca6c-4a1e-11e6-89ec-366e314fff3e.png)

One of the problems while running a jenkins container is that whatever you do it will not get saved after you stop the container.
This is known as stateless container.But that is somewhat the curse of containers.So the question is ,What's the solution ?
The solution is quite simple.We create another container from the same image.We change the name of the container to 'jenkins-data'.
From next time we will run the the second command

//Created a container for containing jenkins data with the image jenkins:2.0

docker create --name jenkins-k8s csanchez/jenkins-kubernetes

//Running jenkins using another container containing data

docker run --volumes-from jenkins-k8s -p 8080:8080 -p 50000:50000 -v /var/jenkins_home csanchez/jenkins-kubernetes

The above command will save data in a container called jenkins-data in the form of an image.THe image contents will be same as /var/jenkins_home

Open http://localhost:8080 in your browser,you should see the below screen:


### Configuring settings for Kubernetes over Jenkins

 

## Fabric8




## Achieving CI/CD 

Again easier said than done,building jenkins from source is one part of the story.But achieving Continous Delivery with your setup is another very different part of the story.Here are some of my tips on achieving Continous Delivery with jenkins when you are dealing with containers:

* Pipeline Plugin

This is a core plugin built by the jenkins community.This plugin ensures that you can easily integrate any orchestration engine with your enviorment.Currently this was started as different communities had started building different plugins for various orchestration engine.This created a mess of various plugins.Using this plugin users now can implement a project’s entire build/test/deploy pipeline in a Jenkinsfile and store that alongside their code, treating their pipeline as another piece of code checked into source control

* Github Plugin

These days most companies are using github as SCM tool.In this case I would recommend you to use this plugin.This plugin helps you to push the code from github and analyze,test it over Jenkins.For authentication purposes I would recommend you to look [Github Oauth Plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+OAuth+Plugin)

* Docker Plugin

For Docker guys 

* [AWS Pipeline](https://wiki.jenkins-ci.org/display/JENKINS/AWS+CodePipeline+Plugin)

Also checkout [AWS CodeCommit](https://wiki.jenkins-ci.org/display/JENKINS/CodeCommit+URL+Helper).

* [OpenStack](https://wiki.jenkins-ci.org/display/JENKINS/Openstack+Cloud+Plugin)

* [Google Cloud Platform](https://wiki.jenkins-ci.org/display/JENKINS/Google+Deployment+Manager+Plugin)

This is a very new plugin.But I think it is worth a try if you wish to automate and work things out with Google Cloud Platform.


Hope you enjoyed this article.Please let us know your valuable comments below.The slide for the above article can be found below

<iframe src="//www.slideshare.net/slideshow/embed_code/key/D66umPt8TAvYdD" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ramitsurana/achieving-cicd-with-kubernetes" title="Achieving CI/CD with Kubernetes" target="_blank">Achieving CI/CD with Kubernetes</a> </strong> de <strong><a href="//www.slideshare.net/ramitsurana" target="_blank">Ramit Surana</a></strong> </div>


  [1]: http://theremotelab.com
  [2]: https://www.linkedin.com/company/the-remote-lab
  [3]: https://www.facebook.com/TheRemoteLab
  [4]: https://github.com/TheRemoteLab
  [5]: https://twitter.com/TheRemoteLab
  [6]: http://docker.com
  [7]: https://jenkins.io
  [8]: http://github.com/kubernetes/minikube
  [9]: http://coreos.com/rkt
  [10]: https://github.com/	
