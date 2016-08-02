---
layout: post
title: "Achieving CI/CD with Jenkins & Kubernetes"
date: 2016-09-07
author: Ramit Surana
tags: kubernetes jenkins docker fabric8 Continous Integration/Delivery
category: containers jenkins docker
excerpt: "Building CI/CD pipeline with jenkins & kubernetes"
---

Hola amigos !!(In English - Hello Friends !!) Hope you are having a jolly good day ! In this rather interesting piece of article we are going to discuss and explore two amazing and rather interesting pieces technology.One i.e. Jenkins,a popular Continous Integration/Deployment tool
CI/CD is an amazing concept.But as many of you might know,it is a lot harder that said done.In terms of Jeff Humble,it can be defined as
""
With the advancement of time and evolution of [kubernetes][11] we want 

So let's get started 
**WARNING** Your machine may hang several times while performing the below steps.

## Methodology

There are many methodology using which we can achieve the above on our machine,some of these are

* [Kubernetes-Jenkins Plugin](#kubernetes-jenkins-plugin)
* [Fabric8](#fabric8)
* [Using Helm with Kubernetes](https://medium.com/@thepauleh/scalable-jenkins-with-helm-and-kubernetes-207df883ac9a)

and many more ...

## Overview of Architecture

Before Starting the work,first let's analyze the workflow required to start  using [docker][6] images with [docker][6].


Before starting to use [jenkins][7] and [kubernetes][11] you need to follow these steps:

![k8s-jenkins-docker](https://cloud.githubusercontent.com/assets/8342133/16848715/ba1473ea-4a14-11e6-99c2-cf8899c351a9.png)


Similarly while using [rkt][] containers a.k.a rktnetes.Here's the architecture:

![k8s-jenkins-rkt](https://cloud.githubusercontent.com/assets/8342133/16848756/ef5afe52-4a14-11e6-8723-a251b0b5c675.png)

*Currently there is no plugin supported rkt containers.But I assume that the workflow will remailn the same after its due implementation. * 

## Kubernetes-Jenkins Plugin

**Setting up Kubernetes on Host Machine**

Setting up [kubernetes][11] on your host machine is an easy task.In case you wish to try out on your local machine I would recommend you to try out [minikube][8].In case you wishe to the best of [kubernetes][11],please follow the steps below:

<script src="https://gist.github.com/ramitsurana/21a137d1007980cadb98253b43a35ead.js"></script>

An amazing work in this direction has been done by [carlossg]().He has built a [kubernetes][11] plugin for [jenkins][7]. Using this plugin you can easily start using [jenkins][7] with [kubernetes][11] directly.Also to provide users with more easy options to configure.He has built a [jenkins][7] image which by default contains [kubernetes][11] plugin.This image is available at docker hub.In the next steps we are going to fetch this image from docker hub and create a volume /var/jenkins_home for storing all your [jenkins][7] data.

### One Problem

Although we are doing everything as we want to do.We still run into a problem,you will notice that whenever you are about to restart your [jenkins][7] container after closing it down.All your data is lost.Whatever you did like creating jobs,installing plugins etc. will be lost.This is one of the common problems with containers.Let's discuss it in a bit depth:

### A word about Data Containers

Data is a tricky concept when it comes to containers.When you first try to install and run the [jenkins][7] container,you may find that the plugins you installed are not visible .There are many ways to deal with such a problem.One is to use docker volumes.But I found it not that useful due to some reasons.One of the ways I found to deal with persistent storage is to crete another container and use it as a source of dynamic data instead of depending on one image.

![data-containers](https://cloud.githubusercontent.com/assets/8342133/16850837/6f36ca6c-4a1e-11e6-89ec-366e314fff3e.png)

One of the problems while running a [jenkins][7] container is that whatever you do it will not get saved after you stop the container.
This is known as stateless container.But that is somewhat the curse of containers.So the question is ,What's the solution ?
The solution is quite simple.We create another container from the same image.We change the name of the container to 'jenkins-data'.
From next time we will run the the second command

````
//Created a container for containing jenkins data with the image jenkins
$ docker create --name jenkins-k8s csanchez/jenkins-kubernetes
````

````
//Running jenkins using another container containing data
$ docker run --volumes-from jenkins-k8s -p 8080:8080 -p 50000:50000 -v /var/jenkins_home csanchez/jenkins-kubernetes
````

The above command will save data in a container called jenkins-data in the form of an image.THe image contents will be same as /var/jenkins_home

Open http://localhost:8080 in your browser,you should see the below screen:

![dash](https://cloud.githubusercontent.com/assets/8342133/17297438/5a53522e-5823-11e6-8501-af049b8c2b5f.png)


### Configuring settings for Kubernetes over Jenkins

Now the jenkins is pre-configured with [kubernetes][11] plugin.So let's jump to the next step.Using the jenkins GUI go to
**Manage Jenkins** -> **Configure System** -> **Cloud** -> **Add new *** --> **Kubernetes**
The screen looks like below after you have followed the above steps:


![k8s-config](https://cloud.githubusercontent.com/assets/8342133/17297596/02cd4bc6-5824-11e6-8ecc-2c4193f91186.png)

Now fill up your configuration settings according to the the pic below:


 

## Fabric8

It is an open source microservices platform based on [Docker][], [Kubernetes][11] and [Jenkins][7].
In order to getting started 
Fabric8 installs the following 
* Jenkins​
* [Gogs​](https://gogs.io/)
* Fabric8 registry​
* [Nexus​](https://wiki.jenkins-ci.org/display/JENKINS/Nexus+Artifact+Uploader)
* [SonarQube](http://sonarqube.org)

Here's a pic of the architecture of [Fabric8][10]

![screen shot 2014-11-06 at 10 07 10](https://cloud.githubusercontent.com/assets/8342133/17319339/a37ae71e-58aa-11e6-9956-2562973a2bc4.png)

In order to get started,first you need to install the command line tool for [fabric8][10] i.e. gofabric8.You can do that using 

````
Download gofabric8 from https://github.com/fabric8io/gofabric8/releases
Untar it and cp /usr/local/bin/
````
Then you need to fix the path of its existence and deploy it simultaneously.

````
$ gofabric8 deploy -y
````
Your terminal screen should look like this

![fabric8](https://cloud.githubusercontent.com/assets/8342133/17319676/e272ed34-58ac-11e6-9a9d-fdd80e4971af.png)

Generating Secrets

````
$ gofabric8 secrets -y
````
Your terminal screen should look like this

![fabric8-2](https://cloud.githubusercontent.com/assets/8342133/17319713/2eab1ab4-58ad-11e6-8fb2-aad173854063.png)

Check for the status of pods using kubectl

````
$ kubectl get pods
````
![jenkins-pods](https://cloud.githubusercontent.com/assets/8342133/17319224/d4440dd6-58a9-11e6-871e-f5db9d52ca14.png)

It will take a while to get all the container images to pull down and getting started.So you can go out and drink coffee :)
You can use kubectl describe pods to check if something fails.

![fabric8-4](https://cloud.githubusercontent.com/assets/8342133/17319386/def51db4-58aa-11e6-9358-659510c082d4.png)

You can checkout the status of your pods via a opening the [kubernetes][11] dashboard in a browser:

http://192.168.99.100:30000

![fabric8-5](https://cloud.githubusercontent.com/assets/8342133/17319479/882ba7f4-58ab-11e6-822c-d8c5da934ae2.png)
 
Similarly you can open the [fabric8][10] hawtio browser interface

![fabric8-console](https://cloud.githubusercontent.com/assets/8342133/17319603/62bcd1a4-58ac-11e6-90b4-4955f54d3274.png)

From my analysis here's what happened when you run the following commands

![fabric8-layout](https://cloud.githubusercontent.com/assets/8342133/17319646/ac7e9f2a-58ac-11e6-9b0c-31bb993c950c.png)

## Achieving CI/CD 

Again easier said than done,building [jenkins][7] from source and integrating [kubernetes][11] is one part of the story.But achieving Continous Delivery with your setup is another very different part of the story.Here are some of my tips on achieving Continous Delivery with [jenkins][7] when you are dealing with containers:

* Pipeline Plugin

This is a core plugin built by the [jenkins][7] community.This plugin ensures that you can easily integrate any orchestration engine with your enviorment with very less complexity.Currently this was started as different communities had started building different plugins for various orchestration engine.This created a mess of various plugins.Using this plugin users now can implement a project’s entire build/test/deploy pipeline in a Jenkinsfile and store that alongside their code, treating their pipeline as another piece of code checked into source control

* Github Plugin

These days most companies are using github as SCM tool.In this case I would recommend you to use this plugin.This plugin helps you to push the code from github and analyze,test it over [Jenkins][7].For authentication purposes I would recommend you to look
[Github Oauth Plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+OAuth+Plugin)

* Docker Plugin

For Docker guys 

* [AWS Pipeline](https://wiki.jenkins-ci.org/display/JENKINS/AWS+CodePipeline+Plugin)

Also checkout [AWS CodeCommit](https://wiki.jenkins-ci.org/display/JENKINS/CodeCommit+URL+Helper).

* [OpenStack](https://wiki.jenkins-ci.org/display/JENKINS/Openstack+Cloud+Plugin)

* [Google Cloud Platform](https://wiki.jenkins-ci.org/display/JENKINS/Google+Deployment+Manager+Plugin)

This is a very new plugin.But I think it is worth a try if you wish to automate and work things out with Google Cloud Platform.


Please let us know your valuable comments below.The slides for the above article can be found below

<iframe src="//www.slideshare.net/slideshow/embed_code/key/D66umPt8TAvYdD" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/ramitsurana/achieving-cicd-with-kubernetes" title="Achieving CI/CD with Kubernetes" target="_blank">Achieving CI/CD with Kubernetes</a> </strong> from <strong><a href="//www.slideshare.net/ramitsurana" target="_blank">Ramit Surana</a></strong> </div>

## The Remote Lab DevOps Offerings:

<iframe src="//www.slideshare.net/slideshow/embed_code/key/h9h9GNjX5Gncpi" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/bhalothia/the-remote-lab-devops-offerings" title="The Remote Lab DevOps Offerings" target="_blank">The Remote Lab DevOps Offerings</a> </strong> from <strong><a href="//www.slideshare.net/bhalothia" target="_blank">Virendra Bhalothia</a></strong> </div>


**Hope you Enjoyed! Keep exploring.Keep learning.**

**Need DevOps help? - Get in touch with** [The Remote Lab][1]
[LinkedIn][2] [Facebook][3] [Github][4] [Twitter][5]


  [1]: http://theremotelab.com
  [2]: https://www.linkedin.com/company/the-remote-lab
  [3]: https://www.facebook.com/TheRemoteLab
  [4]: https://github.com/TheRemoteLab
  [5]: https://twitter.com/TheRemoteLab
  [6]: http://docker.com
  [7]: https://jenkins.io
  [8]: http://github.com/kubernetes/minikube
  [9]: http://coreos.com/rkt
  [10]: http://fabric8.io
  [11]: http://kubernetes.io		
