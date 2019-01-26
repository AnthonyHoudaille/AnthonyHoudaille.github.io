---
published: true
title: "Zookeeper Tutorial (Step 4 on 4) "
collection: awsec2
layout: single
author_profile: true
---

This topic contains Zookeeper (ZK) Install Information. In our architecture, we used **3 Zookeeper instances** to secure the Spark master instances. 


Zookeeper may be installed on its own on a node, or together with Spark/Cassandra on a worker node. *See architecture*. It is important that each zookeeper node is aware of other zookeeper instances so that they form a quorum of 3.

My choise is to install zookeeper before Spark to a gain of time (in terms of configuration files).

# 1. Conexion SSH on the Slave nodes 1 & 2 and Zookeeper node
  
Be connected on each one by SSH.  
If you don't remenber how to do that, you can check the last section of my first tutorial :   
<span style="color:blue">[AWS EC2 Instances Tutorial (Step 1 on 4)](https://anthonyhoudaille.github.io//awsec2/04_Aws_EC2_Tutorial/)</span> 

# 2. Go on Apache-Zookeeper' Website 

You need to copy the link to donwload Apache-Zookeeper.  
<span style="color:blue">[Zookeeper' Website for version 3.4.13](https://www-eu.apache.org/dist/zookeeper/zookeeper-3.4.13/)</span> 

![image](https://AnthonyHoudaille.github.io/images/Zookeeper_DL.png)


# 3. Install Apache-Cassandra on your instances

Make sure your on the /ubuntu/home/ directory.  

## a. Download the .tar.gz file :

Ones your in the good directory, execute the following command :  
``` wget https://www-eu.apache.org/dist/zookeeper/zookeeper-3.4.13/zookeeper-3.4.13.tar.gz```

You will see something like this :  
![image](https://AnthonyHoudaille.github.io/images/Zookeeper_Wget.png)

## b. Extract the software :

You need to extract the software by executing the command bellow :  
``` tar -xv zookeeper-3.4.13.tar.gz```

Just after, remove the .tar.gz file :  
``` rm zookeeper-3.4.13.tar.gz```

Your terminal of your nodes 1 & 2 looks like :  
![image](https://AnthonyHoudaille.github.io/images/Zookeeper_Extract.png)

## c. Do it on each nodes 

Execute the same commands in order.


# 5. Configuration of your three nodes 

For our tutorial, we need to :  
* modify zoo-sample.cfg
* modify spark-default.sh (this is for the next tutorial)
* rename the directory "zookeeper-3.4.13"
* create the directory "logs" and "data"
* create a file 'myid' in the new "data" dir


## a. Rename directory :

Make sure your on the /ubuntu/home/ directory.   
Execute the following command : 
``` mv zookeeper-3.4.13/ zookeeper_1```   
Put the digit 1 if it's your Worker node 1  
			  2 if it's your Worker node 2   
			  3 if it's your Worker node named 'Zookeeper'

## b. Create the directory "logs" and "data" :

Go on this new directory. Now, you need to create those 2 new directories :  
![image](https://AnthonyHoudaille.github.io/images/Zookeeper_data-logs.png)

## c. myid file : 

Go on the new directory "data" and create a new file which contains only a digit between 1 and 3.  
 Put the digit 1 if it's your Worker node 1  
			   2 if it's your Worker node 2   
			   3 if it's your Worker node named 'Zookeeper'

Example :  
![image](https://AnthonyHoudaille.github.io/images/Zookeeper_myid.png)

## d. Modify zoo-sample.cfg : 

First, make sure your on the "conf" directory.   
Copy the file 'zoo-sample.cfg' as 'zoo.cfg'.   

Use this command :    
``` cp zoo-sample.cfg zoo.cfg```

Now, we will change which is inside 'zoo.cfg' file :  
``` vi zoo.cfg```

We will change : 
* clientPort= 218X (The X is a digit between 1 and 3 --> 3 because we have 3 nodes with Zookeeper)
	--> refer to section 5.c. to know which digit you need to put. 
* add : ``` server.1=<PRIVATE.DNS.1>:2891:3881   
			server.2=<PRIVATE.DNS.2>:2892:3882   
			server.3=<PRIVATE.DNS.3>:2893:3883 ```
* datadir= ```<Path to the data dir>


Example :  
![image](https://AnthonyHoudaille.github.io/images/Zookeeper_zoo.png)

Save and quit.

## e. Copy some files : 

Go on the home directory (zookeeper_X) and execute this :  
```  java -cp zookeeper-3.4.13.jar:lib/log4j-1.2.17.jar:lib/slf4j-log4j12-1.7.25.jar:lib/slf4j-api-1.7.25.jar:conf org.apache.zookeeper.server.quorum.QuorumPeerMain conf/zoo.cfg >> logs/zookeeper.log &```

Do the same thing on the three nodes.  
Do not forget, you need "OpenJDK-8" on each nodes.

We discussed how to set up an ensemble with 3 nodes. Now you can create an ensemble with as many nodes as you want by making a few changes.


# 6. Lunch Zookeeper on each nodes 

Right now, all the configuration is set for your Zookeeper Cluster. 

To execute Zookeeper, go on the "Bin" directory and execute this command on each nodes : ```./zkServer.sh Start ```

Your quorum of 3 node with Zookeeper is now ready. You can install Apache-Spark.   
You can follow my tutorial : 
<span style="color:blue">[How to install and configure Apache-Spark](https://anthonyhoudaille.github.io//awsec2/01_Spark_Tutorial/)</span> 
