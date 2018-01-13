---
layout: post
title: "Automating your CI/CD with Chef"
date: 2018-02-01
author: Ramit Surana
tags: Jenkins Continous Delivery Continous Integration Syntax Checking
excerpt: "Some important lessons learnt in CI/CD Journey"
---

Hi Everyone,As Martin Fowler correctly explains :

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.

The primary goal of the process is to be production ready anytime and anywhere. During this journey, I had some reletive experiences and good practices that I developed. In this post I will be illustrating them in depth from a CI/CD admin perspective.

So hope you guys enjoy it !


## Habitat

Habitat is yet another amazing tool by Chef. The tool has been introduced recently in 2016. The tool is still in development phase. The project is written in rust and reactive by nature. Now let's do some installation:

First, visit https://github.com/habitat-sh/habitat#install :

````
$ curl https://raw.githubusercontent.com/habitat-sh/habitat/master/components/hab/install.sh | sudo bash
````

After the installation, try running it on the command line using the below command:

````
$ hab
hab 0.51.0/20171219021329

Authors: The Habitat Maintainers <humans@habitat.sh>
"A Habitat is the natural environment for your services" - Alan Turing

USAGE:
    hab [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    bldr      Commands relating to Habitat Builder
    cli       Commands relating to Habitat runtime config
    config    Commands relating to Habitat runtime config
    file      Commands relating to Habitat files
    help      Prints this message or the help of the given subcommand(s)
    origin    Commands relating to Habitat origin keys
    pkg       Commands relating to Habitat packages
    plan      Commands relating to plans and other app-specific configuration.
    ring      Commands relating to Habitat rings
    studio    Commands relating to Habitat Studios
    sup       Commands relating to the Habitat Supervisor
    svc       Commands relating to Habitat services
    user      Commands relating to Habitat users


ALIASES:
    apply      Alias for: 'config apply'
    install    Alias for: 'pkg install'
    run        Alias for: 'sup run'
    setup      Alias for: 'cli setup'
    start      Alias for: 'svc start'
    stop       Alias for: 'svc stop'
    term       Alias for: 'sup term'

````

If you receive the above output, then you have successfully installed habitat.

## Docker Vs Habitat

![docker-vs-habitat](https://user-images.githubusercontent.com/8342133/34904574-7af45216-f86e-11e7-87a0-1f2abf6aea3b.png)

## Habitat Builder

The Habitat Builder is a place similar to Docker Hub/Quay.io. It is a place where you can automatically check in you code with habitat and build a variety of different docker images. It also enables you to publish your docker images on docker hub by connecting your docker hub account.To get started sign up at [Habitat Builder](https://bldr.habitat.sh). As one can observe, the UI seems pretty slick, so kudos to Habitat Team :)

![habitat1](https://user-images.githubusercontent.com/8342133/34906971-4004eb8c-f89d-11e7-8241-4761a59d8563.png)

The term origin here can be defined as a namespace which is created by the user/organization to build one's own packages. It is similar to defining your name in the dockerhub account.

![habitat2](https://user-images.githubusercontent.com/8342133/34907004-c936141c-f89d-11e7-88fe-85277ac813a0.png)

As you can observe from above Habitat asks you to connect your GitHub account and specify the path to which your plan.sh file is placed. It has a by default path under the habitat folder in which it searches your plan.sh file. You can specify your path and use the dockerhub integration if you wish to publish your images to dockerhub.

Similar to DockerHub, you can also connect your ECR Registry on your AWS account by visiting the Integrations section.

![habitat3](https://user-images.githubusercontent.com/8342133/34907040-5400acba-f89e-11e7-9269-0ad62b71a6b5.png)

You can use some sample files as shown below:

<script src="https://gist.github.com/ramitsurana/96e38aab74ea5529f89d02bbd8822493.js"></script>

After creating a package you can observe the dependencies by scrolling down the page:

![habitat4](https://user-images.githubusercontent.com/8342133/34907085-34168d74-f89f-11e7-8c75-e5f7e7cc22be.png)

Here you can observe that it consists of 2 sections, labelled as Transitive dependencies and Dependencies.In simple terms, the transitive dependencies can be labelled as a basic set of packages that are required by every application that you wish to build using docker. These are provisioned and managed by the Habitat Team. You can also treat it similar to the **FROM** Section when writing a Dockerfile. 

On the other hand, Dependencies label is used to signify the extra packages you are using/mentioned in your **plan.sh** file being used by your application.

## Habitat Architecture

![chef-habitat](https://user-images.githubusercontent.com/8342133/34907583-145ce07a-f8a7-11e7-9c73-8f020a1cb739.png)

## Habitat Studio

Habitat Studio is a another important feature of Habitat that allows you to test and run you application in simulation to like a real enviornment before you publish it. If you are familiar with python, you can think it as similar as [virtualenv](https://virtualenv.pypa.io/en/stable/). So let's try out hab studio.


## Chef Automate

Chef automate is a CI/CD Based solution provided by Chef to complete your end to end delivery. Instead of using Jenkins or any other tool for delivery pipelines, it seems to be a perfect solution for organizations using Chef.

![download](https://user-images.githubusercontent.com/8342133/34776439-ee4fd3a4-f63c-11e7-96ed-caa29be81d25.png)

Download the package from https://downloads.chef.io/automate .

Try using:

````
ramit@ramit-Inspiron-3542:~$ automate-ctl
I don't know that command.
omnibus-ctl: command (subcommand)
create-enterprise
  Create a new enterprise
create-user
  Create a new user
create-users
  Create new users from a tsv file
delete-enterprise
  Deletes an existing enterprise
delete-project
  Deletes an existing project
delete-runner
.....
````

Check if everything is good or not:
````
ramit@ramit-Inspiron-3542:~$ sudo automate-ctl preflight-check
[sudo] password for ramit: 

Running Preflight Checks:
  Checking for required resources...
    ✔ [passed]  CPU at least 4 cores
    ✖ [failed]  memory at least 16GB
  Checking for required directories...
    ✔ [passed]  /var
    ✔ [passed]  /var has at least 80GB free
    ✔ [passed]  /etc
  Checking for required umask...
    ✔ [passed]  0022
....
````

## Chef Automate Architecture

![chef-automate-habitat](https://user-images.githubusercontent.com/8342133/34904580-9db6c5ae-f86e-11e7-974e-87ead19b8dac.png)


## Conclusion

The Chef Automate appears to do things in a creative and more better way than the regular ways than we usually do. But still it has a long way to go. The Dashboard seems nice and useful, giving you a seamless integration of new technological advancements that old tools used for CI/CD do not provide. For Chef regular enterprise users, this seems like a really good choice but when it comes to other non-chef users, it seems to not offer much out-of-the-box solutions. Hope you like this post, tell me your experiences with chef in the below comments section.

Here are some tips on using CI/CD in a better way:

 * Using Syntax Checker

One of the most primary starting point in CI/CD is to write a file using which we describe how our jobs are handled in the pipeline. For various tools, there are many syntax validators. I found them really useful. Some of there tools are:
  
  1. [Jenkins](https://job-dsl.herokuapp.com/)
  2. [Gitlab](https://gitlab.com/ci/lint)
  3. [Travis](https://lint.travis-ci.org/)

* Use CI Web Pages for better output in Web Development Related Projects

You can use this script in Gitlab (.gitlab-ci.yml) to obtain the output at 
*http://<-USERNAME-OF-GITLAB->.gitlab.io/<-PROJECT-NAME->/*

````
pages:
  stage: deploy
  script:
  - mkdir .public
  - cp -r * .public
  - mv .public public
  artifacts:
    paths:
    - public
  only:
  - master
````

For GitHub use the below script in (_config.yml) to obtain the output at 
*http://<-USERNAME-OF-GITHUB->.github.io/<-PROJECT-NAME->/*

````
theme: jekyll-theme-cayman
````

* Avoid using Polling

As correctly said by [Koshuke](http://kohsuke.org/2011/12/01/polling-must-die-triggering-jenkins-builds-from-a-git-hook/), it is important that we adopt new methods to trigger the pipelines.

* Use proper checkout strategy

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

* Try Using Python for writing automation scripts

Python is a super amazing and fun language to work with. One of the cool reasons why I recommend it is because of the awesome libraries it has support to like dictionary, json, csv etc. 