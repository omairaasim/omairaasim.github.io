---
layout: post
title: Install Hadoop in Pesudo Distributed mode on MAC OS
description: Hadoop Installation - Step by step tutorial on how to install Hadoop in Pseudo Distributed mode.
categories: [hadoop-tutorials,hadoop-installation]
tags: [installation, hadoop installation]
fullview: false
comments: true

---

In this tutorial, we are going to setup Hadoop in Pseudo Distributed mode.

Before you follow this - please follow the steps in these 2 tutorials.

  - [Installing Ubuntu on VirtualBox on MAC OS](/hadoop-tutorials/hadoop-installation/2016/06/01/install-ubuntu-virtualbox-mac.html)

  - [Install Hadoop in Standalone mode](/hadoop-tutorials/hadoop-installation/2016/06/03/install-hadoop-standalone-mode-mac.html)

&nbsp;

**So what is Pesudo Distributed Mode?**

To give you a little background - in Hadoop there are 5 services namely

- NameNode
- DataNode
- JobTracker
- TaskTracker
- Secondary NameNode

So in Standalone mode - all these 5 services run on a single JVM and no HDFS is created.

In Pseudo Distributed mode - each Hadoop service runs on a separate JVM and HDFS is created.

So in order to setup Hadoop in Pesudo Distributed mode - we need to configure all 5 services and HDFS.

Lets get started !!!

&nbsp;

**Step 1: Find IP address**

Run the command below to find the IP address of your virtual machine.

```
ifconfig
```

Note the value of inet addr. In my case this value is *192.168.1.19*

!["Get IP Address"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step1_Get_IP_new.png)

&nbsp;  
**Step 2: Connect via Terminal**

As mentioned in the earlier tutorials - its much easier to work with Terminal. So fire up your terminal and connect to your Virtual Machine using your username/password and IP address from STEP 1.

```
ssh hadoop@192.168.1.19
```

&nbsp;

!["Connect via Terminal"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step2_connect_via_terminal_new.png)


**Step 3: Setup NameNode service**

In core-site.xml, we need to specify the value for these 2 properties

- *fs.default.name*

  - Location to run Namenode service.
  - Specify the IP address of your machine
  -  Standard practice is to give port number 10001.

- *hadoop.tmp.dir*

  - Temporary directory for HDFS

&nbsp;

Open core-site.xml in vi editor using the following command

```
sudo vi /usr/local/hadoop/conf/core-site.xml
```

- Press i to enable insert mode
- Paste the following between the *configuration* tags.

```xml
<property>
	<name>fs.default.name</name>
	<value>hdfs://192.168.1.19:10001</value>
</property>

<property>
	<name>hadoop.tmp.dir</name>
	<value>/usr/local/hadoop/hdfs</value>
</property>
```

- Press "esc" to exit insert mode
- Enter :wq to write and quit

!["configure Namenode"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step3_configure_namenode_service_new.png)

**Step 4: Setup JobTracker service**

In mapred-site.xml, we need to specify the value for this property

- *mapred.job.tracker*

  - Location to run JobTracker service.
  - Specify the IP address of your machine
  - Standard practice is to give port number 10002

&nbsp;

Open mapred-site.xml in vi editor using the following command

```
sudo vi /usr/local/hadoop/conf/mapred-site.xml
```

- Press i to enable insert mode
- Paste the following between the *configuration* tags.

```xml
<property>
	<name>mapred.job.tracker</name>
	<value>hdfs://192.168.1.19:10002</value>
</property>
```

- Press "esc" to exit insert mode
- Enter :wq to write and quit

!["configure JobTracker"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step4_configure_jobtracker_service_new.png)


**Step 5: Setup DataNode and TaskTracker service**

Open "slaves" file in vi editor using the following command

```
sudo vi /usr/local/hadoop/conf/slaves
```

- Press i to enable insert mode
- Enter the IP address of your machine
- Press "esc" to exit insert mode
- Enter :wq to write and quit

!["configure DataNode and TaskTracker"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step5_configure_datanode_tasktracker_new.png)


**Step 6: Setup Secondary NameNode service**

Open "masters" file in vi editor using the following command

```
sudo vi /usr/local/hadoop/conf/masters
```

- Press i to enable insert mode
- Enter the IP address of your machine
- Press "esc" to exit insert mode
- Enter :wq to write and quit

!["configure DataNode and TaskTracker"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step6_configure_secondary_namenode_new.png)


**Step 7: Create HDFS folder**

Create a folder called hdfs, which will act as HDFS file system for your Hadoop setup

```
sudo mkdir /usr/local/hadoop/hdfs
```

  &nbsp;

**Step 8: Assign permissions to Hadoop folders**

Assign the permissions of all hadoop folders to the user "hadoop". Incase you created a different username - then change the command accordingly.

```
sudo chown hadoop /usr/local/hadoop
sudo chown hadoop /usr/local/hadoop/bin
sudo chown hadoop /usr/local/hadoop/lib
sudo chown hadoop /usr/local/hadoop/conf
sudo chown hadoop /usr/local/hadoop/hdfs
```

  &nbsp;

**Step 9: Format NameNode**

```
hadoop namenode -format
```

!["format NameNode"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step9_format_namenode_new.png)

**Step 10: Create password less setup for calling Hadoop services**

Run this command

```
ssh-keygen
```

  It will ask you 3 questions - simply Press "ENTER" for each

- Enter file in which to save the key
- Enter passphrase
- Enter same passphrase again

!["ssh keygen"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step10_ssh_keygen_new.png)

Then enter the following command

```
cat /home/hadoop/.ssh/id_rsa.pub > /home/hadoop/.ssh/authorized_keys
```

  &nbsp;

**Step 11: Start Hadoop services**

We are all ready to start the hadoop services. Run the command below

```
start-all.sh
```

  &nbsp;

It may prompt you that the authenticity of your IP cant be established. Do you want to continue - simply type "yes"

  &nbsp;

!["start hadoop services"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step11_start_hadoop_services_new.png)

  &nbsp;

**Step 12: Check if all services have started**

This is the moment of truth. We need to check if all 5 Hadoop services have started.

To check that - run the command below

```
jps
```

You should see the 5 services NameNode, DataNode, JobTracker, TaskTracker and SecondaryNameNode running as shown below.

!["hadoop services running"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_3_install_hadoop_pseudo_distributed_mode/Step12_hadoop_services_running_new.png)


### Congratulations !!! You have successfully setup Hadoop in Pseudo Distributed mode.

Some important notes

To stop hadoop services

```
stop-all.sh
```

&nbsp;

If you dont see NameNode service running (Note: formatting namenode will delete all data)

```
hadoop namenode -format
start-all.sh
```

&nbsp;

Web URL's for accessing the services (Replace with your IP)

- NameNode: http://192.168.1.19:50070/
- DataNode: Can be accessed by going to NameNode and clicking on Browse File system
- JobTracker: http://192.168.1.19:50030/
- TaskTracker: http://192.168.1.19:50060/
- SecondaryNameNode: http://192.168.1.19:50090/
