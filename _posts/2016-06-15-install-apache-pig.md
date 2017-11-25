---
layout: post
title: Install Apache PIG on MAC OS
description: PIG Installation - Step by step tutorial on how to install PIG on MAC.
categories: [hadoop-tutorials,apache-pig]
tags: [installation, pig]
fullview: false
comments: true

---

In this tutorial, we are going to install Apache PIG.

Before you follow this - please follow the steps in these 3 tutorials.

- [Installing Ubuntu on VirtualBox on MAC OS](/hadoop-tutorials/hadoop-installation/2016/06/01/install-ubuntu-virtualbox-mac.html)

- [Install Hadoop in Standalone mode](/hadoop-tutorials/hadoop-installation/2016/06/03/install-hadoop-standalone-mode-mac.html)

- [Install Hadoop in Pesudo Distributed Mode mode](/hadoop-tutorials/hadoop-installation/2016/06/05/install-hadoop-pseudo-distributed-mode-mac.html)

&nbsp;

  Lets get started !!!

&nbsp;

**Step 1: Download PIG**

Let us download the latest release of Apache PIG tar file from [Apache PIG website](http://www-eu.apache.org/dist/pig/pig-0.16.0/pig-0.16.0.tar.gz) .

```
wget http://www-eu.apache.org/dist/pig/pig-0.16.0/pig-0.16.0.tar.gz
```
&nbsp;  

**Step 2: Untar the PIG installation file**

Use the following command to extract the contents of the tar file

```
tar -xvzf pig-0.16.0.tar.gz
```

This will create a folder named pig-0.16.0 in your current folder.

&nbsp;  

**Step 3: Move the PIG folder into /usr/local/pig**

```
sudo cp -r pig-0.16.0 /usr/local/pig
```

&nbsp;

**Step 4: Inform system location of PIG**

We need to edit the bash file to specify the location of PIG. As suggested in the earlier tutorials, it is much easier to use MAC terminal to connect to this virtual machine to make these changes as it allows copy/paste.

```
sudo vi /home/hadoop/.bashrc
```
&nbsp;  

Then go to the end of the file and paste the following export statements.

- Press i to enable "insert mode"
- Right click and paste the export statements
- Press "esc" to exit insert mode
- Enter :wq to write and quit

```
export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-i386
export PIG_PREFIX=/usr/local/pig
export PATH=$PATH:$PIG_PREFIX/bin
```

!["location of pig"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/pig/tutorial_1_install_pig/Step4_edit_bashrc_export_statements.png)

**Step 5: Execute the bash**

Next we need to execute the bash for changes to take effect

```
exec bash
```
&nbsp;  

**Step 6: Test PIG Installation**

Type in pig to see if you enter the grunt mode

```
pig
```

Thats it !!! We have successfully installed Apache PIG.
