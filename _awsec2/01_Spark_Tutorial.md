---
published: true
title: "Apache Spark Tutorial (Step 4 on 4) "
collection: awsec2
layout: single
author_profile: true
---

This topic will help you install Apache-Spark on you cluster AWS EC2.  
I'll show you how to made a standard configuration which allow your elected Master to spread its jobs on Worker nodes.  

The "election" of the primary master is handled by Zookeeper. 

This tutorial will be Split in 5 sections. 
1. The installation of Apache-Spark on your intances
2. The configuration of your Master nodes
3. The configuration of your Slave nodes
4. Add dependences to connect Spark and Cassandra
5. Launch your Master and your Slave nodes

The goal of this final tutorial is to configure Apache-Spark on your instances and make them communicate with your Apache-Cassandra Cluster with a full resilience.


# 1. Apache-Spark installation 

## a. A few words on Spark :

Spark can be configured with multiple cluster managers like YARN, Mesos etc. Along with that it can be configured in standalone mode. 
For this tutorial, I choose to deploy Spark in Standalone Mode.

> ** Standalone Deploy Mode ** :
This is the simplest way to deploy Spark on a private cluster. Both driver and worker nodes runs on the same machine.
Standalone mode is good to go for a developing applications in spark. Spark processes runs in JVM. 

**Java should be pre-installed on the machines on which we have to run Spark job.** 

## b. Conexion SSH on every nodes except the node named Zookeeper :

Make sure your well connected by SSH.  
If you don't remenber how to do that, you can check the last section of my first tutorial :   
<span style="color:blue">[AWS EC2 Instances Tutorial (Step 1 on 4)](https://anthonyhoudaille.github.io//awsec2/04_Aws_EC2_Tutorial/)</span> 

## c. Go on Apache-Spark' Website :

**I prefer to install the stable version 2.3.2.**  
If you want to choose the version 2.4.0, you need to be careful! Some software (like Apache-Zeppelin) don't match with the version. 

You need to copy the link to donwload Apache-Zookeeper.  
<span style="color:blue">[Spark' Website for version 2.3.2](https://www.apache.org/dyn/closer.lua/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz)</span> 

![image](https://AnthonyHoudaille.github.io/images/Spark_Website.png)

## d. Download the .tar.gz file :  **On each nodes**

Ones your in the good directory, execute the following command :  
``` wget http://apache.mediamirrors.org/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz```

You will see something like this :  
![image](https://AnthonyHoudaille.github.io/images/Spark_Wget.png)

## e. Extract the software : **On each nodes**

You need to extract the software and remove the .tar.gz file by executing the command bellow :  
```bash
$ tar -xv spark-2.3.2-bin-hadoop2.7.tgz 
$ rm spark-2.3.2-bin-hadoop2.7.tgz
```

Your terminal of your Master nodes looks like :  
![image](https://AnthonyHoudaille.github.io/images/Spark_Extract1.png) 


Your terminal of your Slave nodes looks like :  
![image](https://AnthonyHoudaille.github.io/images/Spark_Extract2.png) 


## f.  ```~/.bashrc``` file

You need to define the ``` $SPARK_HOME``` path on **each nodes**. 

Execute  ```$ vi ~/.bashrc ``` and define some exports :  

![image](https://AnthonyHoudaille.github.io/images/Spark_bashrc.png)

To quit and save changes : 
'ESC' then write ':wq' then 'ENTER'   
When your on the terminal, execute  ``` source ~/.bashrc```


# 2. The configuration of your Master nodes 

We have 2 files to modify :
* spark-env.sh
* spark-default.conf

Those two files are in the "conf" directory from "spark-2.3.2-bin-hadoop2.7".
Go inside this directory and follow the following modification.  

## a. Save the original files :

First of all, you need to save the original files.  

```bash 
$ cp spark-env.sh.template spark-env.sh 
$ cp spark-defaults.conf.template spark-defaults.conf 
```
![image](https://AnthonyHoudaille.github.io/images/Spark_copy.png)

Let's begin the modification of Spark configuration files. 

## b. 'spark-default.conf' file :

Now, we will add some lines in the 'spark-default.conf' file : 
```bash
spark.master                        spark://PRIVATE_DNS_MASTER1:7077,PRIVATE_DNS_MASTER2:7077
spark.jars.packages                 datastax:spark-cassandra-connector:2.0.0-s_2.11
spark.cassandra.connection.host     <PRIVATE_DNS_Slaves> (separated by ',')
```

Here an example of my cluster : 
```bash
spark.master                        spark://ip-172-31-35-3.ec2.internal:7077,ip-172-31-43-237.ec2.internal:7077
spark.jars.packages                 datastax:spark-cassandra-connector:2.0.0-s_2.11
spark.cassandra.connection.host     ip-172-31-33-255.ec2.internal,ip-172-31-40-97.ec2.internal,ip-172-31-43-212.ec2.internal,ip-172-31-45-7.ec2.internal,ip-172-31-32-5.ec2.internal
```


Example :  
![image](https://AnthonyHoudaille.github.io/images/Spark_default.png)

Save and quit.

## c. 'spark-env.sh' file :

Now, we will add some lines in the 'spark-env.sh' file : 
```bash
export SPARK_LOCAL_IP=<PRIVATE_DNS_this_NODE>
export SPARK_MASTER_HOST=<PRIVATE_DNS_this_NODE>
export SPARK_MASTER_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=<PRIVATE_DNS_Node_Zk1>:2181,<PRIVATE_DNS_Node_Zk2>:2182,<PRIVATE_DNS_Node_Zk3>:2183"
```

Here an example of my Master2 node : 
```bash
export SPARK_LOCAL_IP=ip-172-31-43-237.ec2.internal
export SPARK_MASTER_HOST=ip-172-31-43-237.ec2.internal
export SPARK_MASTER_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=ip-172-31-33-255.ec2.internal:2181,ip-172-31-40-97.ec2.internal:2182,ip-172-31-39-129.ec2.internal:2183"
```

Example :  
![image](https://AnthonyHoudaille.github.io/images/Spark_env.png)


# 3. The configuration of your Slave nodes 

We have 2 files to modify :
* spark-env.sh
* spark-default.conf

Those two files are in the "conf" directory from "spark-2.3.2-bin-hadoop2.7".
Go inside this directory and follow the following modification.  

## a. Save the original files :

First of all, you need to save the original files.  

```bash 
$ cp spark-env.sh.template spark-env.sh 
$ cp spark-defaults.conf.template spark-defaults.conf 
```
![image](https://AnthonyHoudaille.github.io/images/Spark_copy.png)

Let's begin the modification of Spark configuration files. 

## b. 'spark-default.conf' file :

Now open with "vi" and add some lines in the 'spark-default.conf' file : 
```bash
spark.master                        spark://PRIVATE_DNS_MASTER1:7077,PRIVATE_DNS_MASTER2:7077
spark.jars.packages                 datastax:spark-cassandra-connector:2.0.0-s_2.11
spark.cassandra.connection.host     <PRIVATE_DNS_Slaves> (separated by ',')
```

Here an example of my cluster : 
```bash
spark.master                        spark://ip-172-31-35-3.ec2.internal:7077,ip-172-31-43-237.ec2.internal:7077
spark.jars.packages                 datastax:spark-cassandra-connector:2.0.0-s_2.11
spark.cassandra.connection.host     ip-172-31-33-255.ec2.internal,ip-172-31-40-97.ec2.internal,ip-172-31-43-212.ec2.internal,ip-172-31-45-7.ec2.internal,ip-172-31-32-5.ec2.internal
```

Example :  
![image](https://AnthonyHoudaille.github.io/images/Spark_default.png)

Save and quit.

## c. 'spark-env.sh' file :

Open the file and add those lines in the 'spark-env.sh' file : 
```bash
export SPARK_LOCAL_IP=<PRIVATE_DNS_this_NODE>
export SPARK_MASTER_HOST=<PRIVATE_DNS_MASTER1,PRIVATE_DNS_MASTER1>
```

Here an example of my Slave1 node : 
```bash
export SPARK_LOCAL_IP=ip-172-31-33-255.ec2.internal
export SPARK_MASTER_HOST=ip-172-31-35-3.ec2.internal,ip-172-31-43-237.ec2.internal
```

Example :  
![image](https://AnthonyHoudaille.github.io/images/Spark_env2.png)


# 4. Add dependencies to connect Spark and Cassandra

The configuration of Spark for both Slave and Master nodes is finish.   
Therefore, if you want to use Spark to launch Cassandra jobs, you need to add some dependencies in the "jars" directory from Spark.  


This part is quit simple. You just need to download some '.jar' files.  

First, go in the 'jars' directory.  
Then you juste have to execute this : 
```bash
sudo wget http://central.maven.org/maven2/com/twitter/jsr166e/1.1.0/jsr166e-1.1.0.jar;
sudo wget http://central.maven.org/maven2/com/datastax/spark/spark-cassandra-connector_2.11/2.4.0/spark-cassandra-connector_2.11-2.4.0.jar
```
Do it in each nodes.

Those two files are very important to link Spark and Cassandra. 

When you want to add some dependencies, you need to be sure of the version.  
My experience show me that some times we are alone and you need to find solutions to bypass problems.  
If you got problems, you can refer to HortonWorks' Website where there are some troubleshooting pages.   
One response I've got when I do not find any information on the net : 
![image](https://AnthonyHoudaille.github.io/images/hortonworks.png)


# 5. Launch your Master and your Slave nodes

For this final section, I'll show you how to launch Spark on all your nodes.  
We will also check the Master Resilience handled by Zookeeper.  

To be sure that your Saprk cluster  will be launched correctly, you should follow the 3 points below in order :   
* Launch Zookeeper on all nodes where the software is installed. 
* Launch the two Spark Master 
* Launch all Spark Worker nodes 

## a. Launch Zookeeper :


To execute Zookeeper, go on the "Zookeeper_X/Bin/" directory and execute this command on each Zk nodes  : ```./zkServer.sh Start ```

## b. Launch Spark on your Master nodes : 

Go back to the Spark directory.  
Then, to launch Spark, you should go insite the "sbin" directory and start your Master : 
```bash 
$ cd /home/ubuntu/spark-2.3.2-bin-hadoop2.7/sbin
$ ./start-master.sh
```
![image](https://AnthonyHoudaille.github.io/images/Spark_master_launch.png)

Once the master is running, navigate to port 8080 on the Node’s Public IP and you get a snapshot of the cluster.
On your browser, go on <IPV4_IP_Public_Master>:8080.  
You will see the following page :  
![image](https://AnthonyHoudaille.github.io/images/Spark_master_web.png)

## c. Launch Spark on your Slave nodes : 

 
The command line to launch Spark on all slave nodes is quit different. You have to specify the address of the Master 1 and 2.  
```bash 
$ cd /home/ubuntu/spark-2.3.2-bin-hadoop2.7/sbin
$ ./start-slave.sh spark://<PRIVATE_DNS_MASTER1>:7077,<PRIVATE_DNS_MASTER2>:7077
```
Here an example :  
![image](https://AnthonyHoudaille.github.io/images/Spark_slave_launch.png)

Once all slave nodes are running,  reload your master browser page. All Worker nodes will be attached to the Master 1 :   
![image](https://AnthonyHoudaille.github.io/images/Spark_master_web2.png)

## d. Master Resilience : 

What if I shutdonw my master 1?   
At this moment, Zookeeper will handle the selection of a new Master Spark.

When I'll stop my master 1, the Master 2 will be elected new Master and all Worker nodes will be attached to the new elected master. 

To check it : 
* Shutdonw the master 1
* Check the navigate port 8080 of the master 2

![image](https://AnthonyHoudaille.github.io/images/Spark_master1_down.png)

Check the transfer of Master : 

Execute like I do :
![image](https://AnthonyHoudaille.github.io/images/Spark_master2_cat.png)

You will be able to read that Zookeeper elected the master2 as the primary master :  
![image](https://AnthonyHoudaille.github.io/images/Spark_master2_election.png)


And to finish, if you navigate to the Master 2 port 8080, you will see all Slave nodes attached :  
![image](https://AnthonyHoudaille.github.io/images/Spark_master2_web.png)


> **Conclusion** : We covered the basics of setting up Apache Spark on an AWS EC2 instance. We ran both the Master and Slave daemons on the same node. Finally we demonstrated the resilience of our Masters thanks to Zookeeper. 
This set of tutorials is now ended. Thanks you for reading. 
