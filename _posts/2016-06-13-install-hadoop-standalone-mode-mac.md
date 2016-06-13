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

  Open the Terminal window in your MAC and type the following ssh command. Replace the IP address with what you found in Step 3. Also "hadoop" is the username we created while installing Ubuntu - so if you used a different username - you need to replace that as well.

```
ssh hadoop@192.168.1.10
```

  This will give you some authenticity error message - just type "yes" so it will add this IP to your known hosts.

  It will then prompt you for a password. Again this is the same password we created earlier. Enter your chosen password.
  !["ifconfig"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step4_Using_terminal.png)

  **Step 5: Download Hadoop**

  I am using Hadoop 1.2.1 version here. We can download that from the apache website using the wget command as shown below.

```
wget https://archive.apache.org/dist/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
```

  !["ifconfig"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_2_install_hadoop_standalone_mode/Step5_Download_Hadoop.png)

```python
s = "Python syntax highlighting123"
print s
```

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```
