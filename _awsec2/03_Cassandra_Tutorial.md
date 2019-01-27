---
published: true
title: "Apache Cassandra Tutorial (Step 2 on 4) "
collection: awsec2
layout: single
author_profile: true
---

This topic contains information for deploying Apache-Cassandra on your Slave nodes.


# 1. Conexion SSH on each Slave nodes

For this tutorial, we will only used Slave nodes.  
Be connected on each one by SSH.  
If you don't remenber how to do that, you can check the last section of my previous tutorial :   
<span style="color:blue">[AWS EC2 Instances Tutorial (Step 1 on 4)](https://anthonyhoudaille.github.io//awsec2/04_Aws_EC2_Tutorial/)</span> 


# 2. Install Java SDK 8

Ones your instances ready and your connection done :

Java 8 is needed to run Cassandra.  
Install it with the following command on each Slave nodes :   
``` sudo apt install openjdk-8-jre-headless```

When it's done, execute  ```vi ~/.bashrc ``` and define some exports :  

![image](https://AnthonyHoudaille.github.io/images/Cassandra_exports.png)

To quit and save changes : 
'ESC' then write ':wq' then 'ENTER'   
When your on the terminal, execute  ``` source ~/.bashrc```


# 3. Go on Apache-Cassandra' Website 

You need to copy the link to donwload Apache-Cassandra.  
<span style="color:blue">[Apache-Cassandra' Website for version 3.11.3](http://www.apache.org/dyn/closer.lua/cassandra/3.11.3/apache-cassandra-3.11.3-bin.tar.gz)</span> 

![image](https://AnthonyHoudaille.github.io/images/Cassandra_Website.png)


# 4. Install Apache-Cassandra on your instances

Make sure your on the /ubuntu/home/ directory.  

## a. Download the .tar.gz file :

Ones your in the good directory, execute the following command :  
``` wget http://wwwftp.ciril.fr/pub/apache/cassandra/3.11.3/apache-cassandra-3.11.3-bin.tar.gz```

You will see something like this :  
![image](https://AnthonyHoudaille.github.io/images/Cassandra_Wget.png)

## b. Extract the software :

You need to extract the software by executing the command bellow :  
``` tar -xv apache-cassandra-3.11.3-bin.tar.gz```

Just after, remove the .tar.gz file :  
``` rm apache-cassandra-3.11.3-bin.tar.gz```

Your terminal looks like :  
![image](https://AnthonyHoudaille.github.io/images/Cassandra_Extract.png)

## c. Do it on each Slave nodes 

Execute the same commands in order.


# 5. Configuration of your 5 nodes Cluster 

For our tutorial, we need to modify 2 files :  
* cassandra.yaml
* cassandra-rackdc.properties   

Those two files are in the "conf directory" :  
``` cd apache-cassandra-3.11.3/conf/```

## a. Cassandra.yaml file :

Open the file :  
``` vi cassandra.yaml```

We will change : 
* cluster_name: "give the name you want"
* listen_address: Private_ip_address (only of this node)
* rpc_address: Private_ip_address (only of this node)
* seed_provider: Private_ip_address (from each nodes of your cluster Cassandra)
* endpoint_snitch: Ec2Snitch

Save and quit.

Example :  
![image](https://AnthonyHoudaille.github.io/images/Cassandra_yaml.png)

Do the same thing on the 5 Slave nodes.

## b. cassandra-rackdc.properties file : 

For this tutorial, we just have to comment the two lines uncommented.  
We do not have to specify any rack name or the name of the DataCenter because we just have one.  

![image](https://AnthonyHoudaille.github.io/images/Cassandra_rack.png)


Right now, all the configuration is set for your Cassandra Cluster.   

# 6. Try the node connection

The last step is to try the connection between every Cassandra nodes.  

You need to go in the 'bin' directory.   
Now, execute this commande on each node : 
``` ./cassandra```    
Execute this on each node.  
You will see some line with "Handshaking" which mean that the nodes communucate with each others.  
To see the complete connection, execute this line :  
``` ./nodetool describecluster```  
![image](https://AnthonyHoudaille.github.io/images/Cassandra_Final.png)



Your Slave nodes with Apache-Cassandra are now configured.  

> **Conclusion** : The next step is to install and configure zookeeper on the dedicated node and on two of our five Workers nodes.  
You can follow my tutorial : 
<span style="color:blue">[How to install and configure Zookeeper](https://anthonyhoudaille.github.io//awsec2/02_Zookeeper_Tutorial/)</span> 
