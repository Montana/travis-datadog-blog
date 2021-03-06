---
title: "DataDog and Travis CI"
created_at: Fri Jan 07 2022 15:00:00 EDT
author: Montana Mendy
layout: post
permalink: 2022-01-07-datadog-travis
category: news
excerpt_separator: <!-- more --> 
tags:
  - news
  - feature
  - infrastructure
  - community
---

![TCI-Graphics for AdsBlogs (4)](https://user-images.githubusercontent.com/20936398/148598812-59e25905-a6a4-4cb0-8147-d30e500fe8ce.png)

In this weekly edition of my blog posts, I'm going to show you how to effcitevely pull HCL files (Terraform, Vault, Vagrant) and more using Travis CI and DataDog. Take a seat, this is going to be a goodie. 

<!-- more --> 

## Getting started 

Let's get and _set_ your `environment variables` from DataDog: 

```bash
export DATADOG_API_KEY=foo
export DATADOG_APP_KEY=bar
```
<img width="310" alt="Screen Shot 2022-01-07 at 11 08 02 AM" src="https://user-images.githubusercontent.com/20936398/148594393-03d28334-ce73-4cdd-9d9b-0411cd592730.png">

Get your credentials within DataDog, then port them over to Travis CI. Since this is a Go project I've selected, this is how our `.travis.yml` is going to look like:

```yaml
language: go
go:
  - 1.13.x

install: true

script:
  - script/test
  - go build

notifications:
  email: false
  slack: false

branches:
  only:
  - master
  - v2.0
```
 
You can in theory remove `-v2.0` from the branches, make sure you have all the proper HCL programs specified. You can run:

```bash
datadog-fetch-hcl -id <dashboard id> -title <resource title>
```

## Metrics

Here are some metrics DataDog will grab: 

| travis_ci.build.duration (gauge)  	| Duration of builds Shown as second                               	|
|-----------------------------------	|------------------------------------------------------------------	|
| travis_ci.build.finished (count)  	| Count of finished builds Shown as build                          	|
| travis_ci.build.num_jobs (count)  	| Count of jobs Shown as job                                       	|
| travis_ci.job.duration (gauge)    	| Duration of jobs Shown as second                                 	|
| travis_ci.job.finished (count)    	| Count of finished jobs Shown as job                              	|
| travis_ci.job.queued_time (gauge) 	| Time jobs spent waiting in queue before starting Shown as second 	|

## What is DataDog exactly counting?

Basically the duration of the build, amongst other nuance detailes, and if there's any 3rd party factors causing a slowdown if there is one. Make sure you're sill logged into DataDog:

<img width="793" alt="Screen Shot 2022-01-07 at 11 13 42 AM" src="https://user-images.githubusercontent.com/20936398/148595131-be4ba1cc-cf7a-4766-a431-bce4850b0c1e.png">

Now let's trigger a build in Travis CI: 

<img width="1326" alt="Screen Shot 2022-01-07 at 11 47 00 AM" src="https://user-images.githubusercontent.com/20936398/148598966-bef44d7a-44f2-4bfc-9b50-0a0c2d0b9124.png">

As you can tell, we've had a successful build with DataDog and Travis CI!

## Conclusion 

There you go, DataDog see's no vulns, and you were able to fetch your HCL programs you wanted. In this case Terraform was important to me, but other applications will be more important to other people at different times. 

There you have it! You've just used DataDog with Travis CI, again I'm glad to be showing the flexibility and ease of use with Travis CI. 

As always if you have any questions, any questions at all, please email me at [montana@travis-ci.org](mailto:montana@travis-ci.org).

Happy building!
