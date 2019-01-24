---
layout: archive
title: "AWS EC2"
permalink: /awsec2/
author_profile: true
---

# How to deploy a cluster AWS EC2 with Spark and Cassandra

## Context : 

This tutorial presents a step-by-step guide to configure a cluster AWS EC2 with Apache Spark and Apache Cassandra. 

### Architecture we will create : 

First of all, I would like to present you the Architecture I want to deployed. 

![image](https://AnthonyHoudaille.github.io/images/Architecture_Cluster.png)

On this architecture, we've got 8 instances AWS EC2 : 

* 2 Masters nodes with apache-Spark-2.3.2
* 5 Slaves nodes with apache-Spark-2.3.2 and apache-cassandra-3.11.2, including zookeeper installed on 2 of these nodes.
* The last one is a node created for the resilience of the Master. We Installed zookeeper in it.

In terms of resilience : 

* The Slaves resilience is automatically handled by the master Spark. 
* The Masters resilience is handled by Zookeper.
* Zookeeper is resilient thanks to the corrum of Three nodes which contains the software.


If you want to realised this architecture, I invite you to follow (in order) the 4 tutorials bellow.

1. The first one explain how to create instances on AWS EC2.
2. The second explain you how to install apache Cassandra and how to configure it
3. The third explain you how to install apache Spark and how to configure it
4. The last one show you how to have a Master resilience thanks to Zookeeper

Of course, you can rise the number of Master nodes and Slaves nodes. The process will be the same as I explain. 

{% include base_path %}

{% for post in site.awsec2 reversed %}
  {% include archive-single.html %}
{% endfor %}


