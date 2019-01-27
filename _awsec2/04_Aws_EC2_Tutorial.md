---
published: true
title: "AWS EC2 Instances Tutorial (Step 1 on 4) "
collection: awsec2
layout: single
author_profile: true
---

This tutorial will help you get jump started with AWS EC2. 


# 1. Go to Amazon Web Services’ Website

The step one is simple, you need to go to AWS' website.

Here i the link :  <span style="color:blue">[AWS' Website](https://aws.amazon.com/fr/console/)</span>
Click on "Sign In to the Console" (top right)

Then, you just to sign in if you have account, if not make one.

# 2. Launch EC2 instances 

Now, click on "EC2" and then, you will have this page : 

Click on "Launch Instance".
![image](https://AnthonyHoudaille.github.io/images/EC2_launch_instances.png)


# 3. Choose your AMI

At this point, you will select the boot OS.  

For this tutorial, I'll choose "Ubuntu Server 18.04 LTS (HVM), SSD Volume Type"
![image](https://AnthonyHoudaille.github.io/images/EC2_launch_Ubuntu.png)

# 4. Instances options :

## a. Select the type 

Select the t2.micro.  
Those instances are perfect to made a test cluster. 
![image](https://AnthonyHoudaille.github.io/images/EC2_launch_t2micro.png)

## b. Configure your instance 

Recall: We need 8 instances to deployed the cluster. So you need to specified it :
![image](https://AnthonyHoudaille.github.io/images/EC2_number.png)

## c. Add Storage

In function of your needs, you can increase the storage capacity.  
For exemple, in my <span style="color:blue">[GDELT progect](https://aws.amazon.com/fr/console/)</span> , we needed 650GB on each instances.   

Be carefull, you paid each GB of EBS storage.

Here, take the standard storage : 8GB. 

## d. Add Tags

You can add tags if you want. I just pass to the next step.

## e. Configure Security Group

This step is realy important. The security group allow your instances to communicate with others instances under the same security group.

For example, if you create 2 instances. Then, 2 days later if you want to add some instances to your cluster, you must put your new intances under the same security group as the old ones.

The communication between Slaves and Masters is essential to transfer Data.

![image](https://AnthonyHoudaille.github.io/images/EC2_security_group.png)

For this tutorial, my "Cluster_test" is open to the world.
You can change this configuration to protect your job. 


## f. Review and launch

This is the last part of the configuration. Check options you put and launch your instances if it's ok for you.


# 5. Create a Key Pair 

The key pair is an important file. It allow us to be connected with the instances in ssh.

Create a key pair and save it !
![image](https://AnthonyHoudaille.github.io/images/EC2_key_pair.png)

Be carreful ! The file loaded is a '.txt'.  
You need yo change the extention. The real name must be : "Cluster_test_Key_Pair.pem".  

# 6. Change the Name of your Intances 

All your instances are now in initialisation.  
I recommand you to change the name of your instances to be clear later. 
![image](https://AnthonyHoudaille.github.io/images/EC2_change_name.png)

# 7. Connexion to our Instances in SSH

First, you need to open a terminal to execute bash commands.

## a. Protect your KeyPair against accidental overwriting

In command line, go into the directory where your "Cluster_test_Key_Pair.pem" is stocked.

Then, execute this commande :   
``` bash chmod 400 Cluster_test_Key_Pair.pem```

## b. Try a connexion to one of your instances 

Copy the public DNS on AWS' website : 
![image](https://AnthonyHoudaille.github.io/images/EC2_copy_DNS.png)

Then, go back to your terminal and execute the commande bellow :  
``` ssh -i "<path to your keyPair directory>/Cluster_test_Key_Pair.pem" ubuntu@<copy the public DNS>```   
Example :   
``` ssh -i "/Users/anthonyhoudaille/Desktop/Tuto_Cluster_EC2_AWS/Cluster_test_Key_Pair.pem" ubuntu@ec2-54-211-51-246.compute-1.amazonaws.com```



> **Conclusion** : "Bravo" You're connected to your instances in SSH.  
You can pass to my new tutorial : <span style="color:blue">[How to Install Apache-Cassandra on AWS EC2 Instances](https://anthonyhoudaille.github.io//awsec2/03_Cassandra_Tutorial/)</span>

Thank you for reading.  
If you have any questions, please, don't hesitate to send me an email.

