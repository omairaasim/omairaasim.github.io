---
layout: post
title: Install Hadoop in Standalone mode on MAC OS
description: Hadoop Installation - Step by step tutorial on how to install Hadoop in stand alone mode.
categories: [installation]
tags: [installation, hadoop installation]
fullview: false
comments: true

---

In this tutorial, I am going to show you how to setup Hadoop in stand alone mode. Before you follow this - you have to install Ubuntu on VirtualBox. If you have not done that yet - then please follow this tutorial first - [Installing Ubuntu on VirtualBox on MAC OS](/installation/2016/06/11/install-ubuntu-virtualbox-mac.html)

**Step 1: Install Java**

The first thing we need to install is java. Run the following command to install Java.

```
sudo apt-get install openjdk-7-jdk
```

!["Install Java"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step1_Install_Java.png)

Java is successfully installed.
!["Install Java"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step1_done_java.png)

**Step 2: Test Java installation**

Check if Java was successfully installed by typing the following command. You should see the output as shown in the screenshot below.

```
java -version
```

!["Test Java Installation"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step2_Test_java.png)

**Step 3: Finding IP address of Virtual Machine**

This is a matter of personal preference. Using the VirtualBox is not very user friendly. We cannot copy/paste commands etc (Atleast I've not figured out an easy way) - so I prefer to use the MAC Terminal to connect to Virtualbox and use that.

So in order to connect - we need to know the IP address of the Virtual Machine. To find that out - enter the following command

```
ifconfig
```

You will see an inet addr: 192.168.1.10 - This is the IP address we will use to connect via Terminal.

!["ifconfig"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step3_ifconfig.png)

**Step 4: Using Terminal**

  Open the Terminal window in your MAC and type the ollowing ssh command. Replace the IP address with what you found in Step 3. Also "hadoop" is the username we created while installing Ubuntu - so if you used a different username - you need to replace that as well.

```
ssh hadoop@192.168.1.10
```

This will give you some authenticity error message - just type "yes" so it will add this IP to your known hosts.

It will then prompt you for a password. Again this is the same password we created earlier. Enter your chosen password.

!["using MAC terminal"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step4_Using_terminal.png)

**Step 5: Download Hadoop**

I am using Hadoop 1.2.1 version here. We can download that from the apache website using the wget command as shown below.

```
wget https://archive.apache.org/dist/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
```

!["download hadoop file"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step5_Download_Hadoop.png)

**Step 6: Untar Hadoop file**

Next we need to untar this hadoop file that we downloaded. The command for that is

```
tar -xvzf hadoop-1.2.1.tar.gz
```

!["Untar hadoop file"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step6_untar_hadoop.png)

**Step 7: Copy the hadoop folder**

Once you untar the hadoop tar file - you will see a folder created in the same directory with the name *hadoop-1.2.1*.

Next we need to copy this into /usr/local/hadoop

Use this command to copy

```
sudo cp -r hadoop-1.2.1 /usr/local/hadoop
```

!["Copy hadoop folder"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step7_copy_usr.png)

**Step 8: Setup Environment**

Next we need to inform the system where Hadoop framework is located. We have to edit the .bashrc file.

```
sudo vi /home/hadoop/.bashrc
```

Move to end of the .bashrc file
- Press "i" to enable "insert" mode.


Then copy paste the 2 lines below at the end of the .bashrc file

```
export HADOOP_PREFIX=/usr/local/hadoop
export PATH=$PATH:$HADOOP_PREFIX/bin
```

  - Press "esc" to exit insert mode
  - Then press "wq" to write and quit

!["Edit bashrc file"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step8_add_export_bash.png)

**Step 9: Update the bash Settings**

To update the bash settings - we simply need to execute it.

Run the command

```
exec bash
```

!["Exec bashrc file"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step12_exec_bash.png)

**Step 10: Setup Java location in Hadoop**

Next we need to inform Hadoop where java is located in the system. We have to specify this in the *hadoop-env.sh* file.

Enter the command

```
sudo vi /usr/local/hadoop/conf/hadoop-env.sh
```
- Press "i" to insert mode.
- Uncomment the JAVA_HOME and replace with

```
export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-i386
```

- Press "esc" to exit insert mode
- Press "wq" to write and quit

!["Specify JAVA_HOME"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step10_uncomment_java.png)

**Step 11: Test Hadoop**

In order to check if Hadoop is successfully installed - run the following command

```
hadoop
```

You should see a similar output

!["Test Hadoop"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step11_test_hadoop.png)

At this point, Hadoop is successfully installed in Standalone mode.

Things to note about Stand alone mode
- All Hadoop services run within a single JVM
- HDFS is not present
- Hadoop uses local file system

So I typically do not like working in this mode. It does not give you a feel of working in a Hadoop cluster.

In the next tutorial, we will install Hadoop in a Pseudo Distributed mode.
