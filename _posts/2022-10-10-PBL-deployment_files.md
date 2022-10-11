---
toc: true
comments: false
layout: post
title:  Deployment Files
description: Shared machine will require some standards
categories: []
type: pbl
week: 8
---

## Communication onf Machines
> It would be nice if there were some standards published.  Here are some ideas.  It takes about 15 minutes to plan or hours to fix.  

## P1: nighthawkcodingsociety.tk

|Period|Table|Port|Project|image_nm|nginx|subdomain|
|1|1|8011|T1_|||
|1|2|8012|T2_|||
|1|3|8013|T3_|||
|1|4|8014|T4_|||
|1|5|8015|T5_|||
|1|6|8016|T6_|||
|1|7|8017|T7_fantasy-fb|fantasy-fb_v1|

## P2: nighthawkcodingsociety.ml

|Period|Table|Port|Project|image_nm|nginx|subdomain|
|2|1|8091|t1_juiceVRLD|juiceVRLD_v1||
|2|2|8092|T2_|||
|2|3|8093|T3_|||
|2|4|8094|t4_csateam|csateam_v1||
|2|5|8095|T5_|||
|2|6|8096|t6-breadbops|||

## Notes from Rohan J
> For Period 2, to avoid port conflicts please use port 809#, where # is your table number, for your docker containers. (Table 10 use 8100).
For NGINX files please start the filename with T# (as Mr. M said).

> I've seen some issues when working with docker containers with duplicate container names in the docker-compose files in different projects. Therefore, please replace "web" in docker-compose with "web_t#" to ensure each container has a unique name (period 2) & replace "javaspring_v1" w/ "javaspring_v1_t#" in docker-compose.yml