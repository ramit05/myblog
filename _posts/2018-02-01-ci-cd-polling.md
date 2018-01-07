---
layout: post
title: "Using Polling with CI/CD in Work"
date: 2018-02-01
author: Ramit Surana
tags: Jenkins Continous Delivery Continous Integration Polling Syntax
excerpt: "Some important lessons learnt in CI/CD Journey"
---

Hi Everyone,As Martin Fowler correctly explains :

> Continuous Delivery is a software development discipline where you build software in such a way that the software can be released to production at any time.
The primary goal of th process is to be production ready anytime and anywhere. During this journey, I had some reletive experiences and good practices that I developed. In this post I will be illustrating them in depth from a CI/CD admin perspective.

So hope you guys enjoy it !

## What is Polling ?

The term polling can be defined as the method 

## Polling with Jenkins

## What happens when you do Multiple Checkouts ?

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

* Avoid using Polling at all Costs

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

Python is a super amazing and fun language to work with. But one of the cool reasons to use it because of the awesome libraries it has support to like dictionary, json, csv etc. 

## Conclusion

Hope you guys enjoyed the post and please share your side of the views in the comments too.Have a happy day !
