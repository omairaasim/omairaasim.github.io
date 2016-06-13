---
layout: post
title: Installing Ubuntu on VirtualBox on MAC OS
description: Hadoop Installation - Step by step tutorial on how to install Ubuntu on Virtualbox.
categories: [installation]
tags: [virtualbox, installation, ubuntu, hadoop installation]
fullview: false
comments: true

---

  There are many ways of getting started with Hadoop. If you don't want to get your hands dirty - the quickest way is to download a quickstart VM from Cloudera or Hortonworks and get going. But if you want to get into the nitty gritty details and want to have the satisfaction of installing Hadoop from scratch - this tutorial is for you.

  In order to Install Hadoop, we need to have a Linux operating system. So we cannot install Hadoop on Windows or MAC systems. The work around for that is we need to install a virtualization software and then install Linux on that. There are many virtualization software's available like VMWare Fusion but it only comes as a 30 day trial version. So I prefer using Oracle VirtualBox.

  In this tutorial, I will be sharing step by step instructions of installing Ubuntu on VirtualBox.  
  &nbsp;

**Step 1 - Download VirtualBox**

   You can download virtual box from here (https://www.virtualbox.org/wiki/Downloads). You can select the OS X version. Once you download it, you will see a file similar file in your download folder - *VirtualBox-5.0.20-106931-OSX.dmg*
!["Download VirtualBox"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step1_Download_Virtualbox.png)

**Step 2 - Begin installation of VirtualBox**

  Double click the dmg file which will open a similar window as shown below. Double click on VirtualBox.pkg icon.
!["Begin installation of VirtualBox"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step2_Install_Virtualbox.png)

**Step 3 - Install VirtualBox**

 Click on "continue".
!["Install VirtualBox"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step3_Install_1.png)

**Step 4 - Installation successful**

  In the next step it will show you the space it will take on your system and give you an option to change the install location. I've gone ahead with the default location. Click on "Install" to proceed. This will complete the installation of VirtualBox.
!["Installation successful"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step4_Install_2.png)
!["Install VirtualBox successful"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step4_Install_successful.png)

**Step 5 - Launch VirtualBox**

 Go to your applications and launch VirtualBox. The first screen will look like below.
!["Launch VirtualBox"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step5_Launch_Virtualbox.png)

**Step 6 - Specify name and Operating system**

  Click on "New" and specify any name for the Virtual machine. Since I am going to be using it for Hadoop purpose - I'm specifying "HadoopVM".

  Next we need to select an operating system. Select Type as "Linux" and version as "Ubuntu (64-bit)".

  Click on "Continue".
!["Specify name and Operating system"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step6_New.png)

**Step 7 - Specify Memory Size**

  It is recommended to allocate 1GB RAM. So I am going to specify 1024 MB. Click on "Continue".
!["Specify Memory Size"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step7_Memory_Size.png)

**Step 8 - Hard disk**

  We are going to create a new virtual hard disk. So select that option and click on "Create".
!["Hard disk"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step8_Harddisk.png)

**Step 9 - Hard disk file type**

  On the next screen, we need to select the file type. Select the default option of VDI (VirtualBox Disk Image) and click on "Continue".
!["Hard disk file type"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step9_Harddisk_type.png)

**Step 10 - Fixed or dynamically allocated hard disk**

  In this step, we need to specify if we want the hard disk to grow dynamically or if we want to create it with a fixed size. We are going to select the "Dynamically allocation" option here.
!["Fixed or dynamically allocated hard disk"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step10_dynamically_allocated.png)

**Step 11 - Size of hard disk**

  Here specify 20 GB as maximum hard disk allocation.
!["Size of hard disk"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step11_Size.png)

**Step 12 - Virtual machine created**

  *Tada !! Thats it - your virtual machine is ready. This was the easy part. Next we need to install the Ubuntu operation system on this virtual machine.*
!["Virtual machine created"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step12_VM_created.png)

**Step 13 - Network Settings**

  Before we install Ubuntu - we need to make the following network settings.

  * Click on "Settings"
  * Click on "Network"
  * Select "Adapter 1"
  * Make sure "Enable Network Adapter" is checked
  * For Attached to - Select "Bridged Adapter"
  * Click "OK".

  Maybe there is a different way - but the reason I do this is so I can connect to this virtual machine from my MAC's terminal. Its much easier to work on the Terminal as it allows copy/paste etc. You will see that as we move on.  
  &nbsp;  
!["Network Settings"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step13_Network_settings.png)

  &nbsp;

**Step 14 - Download and select Ubuntu**

  Next we need to install Ubuntu on this Virtual machine. Follow these steps.

  * First download Ubuntu operating system from [here](https://s3.amazonaws.com/omairaasim.github.io/ubuntu/ubuntu-12.04.3-server-i386.iso)
  * Then back on VirtualBox - click on Settings
  * Click on Storage
  * Click on Empty
  * Click on the small disk next to IDE Secondary Master and select "Choose Virtual Optical Disk File"

    &nbsp;
  !["Download and select Ubuntu"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step14_Select_Ubuntu.png)
  &nbsp;

  * Browse to the folder where you downloaded the Ubuntu operating system and select the iso file - *ubuntu-12.04.3-server-i386.iso*
  * Click on Open.
  !["Browse iso ubuntu file"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step14_select_ISO.png)
  * Select "OK".
  !["Select iso Ubuntu file"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step14_Ubuntu_selected.png)

**Step 15 - Install Ubuntu**

  Back on the main VirtualBox screen - its time to power the Virtual machine on. Click on the "Start" button. You will be asked to select "Language". To continue with "English" press "Enter". Here your mouse will not work - so use the keyboard arrows and enter to take actions.
!["Install Ubuntu"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step15_Click_Start_Language.png)

**Step 16 - Install Ubuntu Server**

  Here the default selected option is "Install Ubuntu Server". Press "Enter" to continue.
!["Install Ubuntu Server"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step16_Install_Ubuntu_server.png)

**Step 17 - Select Language**

  It will ask for language again. Select the language and press "Enter" to continue.
!["Select Language"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step17_Select_language.png)

**Step 18 - Select Country**

  Select the country and press "Enter" to continue.
!["Select Country"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step18_Country.png)

**Step 19 - Detect keyboard**

  For Detect keyboard layout - select "No" and press "Enter" to continue.
!["Detect keyboard"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step19_keyboard_layout.png)

**Step 20 - Keyboard language**

  Select the language and press "Enter" to continue.
!["Keyboard language"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step20_Language.png)

**Step 21 - Keyboard layout**

  Select the language and press "Enter" to continue. This will begin the installation.
!["Keyboard layout"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step21_keyboard_language.png)

**Step 22 - Enter Hostname**

  Here you can specify any Hostname you like. Since I am using this for Hadoop - I am going to enter Hostname as "hadoop". Press tab which will select "Continue" option - then press "Enter".
!["Enter Hostname"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step22_Hostname.png)

**Step 23 - Enter name for user**

  It will ask you to enter name for the new user. Again I am using "hadoop". Press tab and then hit "Enter".
!["Enter name for user"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step23_Username.png)

**Step 24 - Enter username**

  It will ask you to enter username for the new user. Again I am using "hadoop". Press tab and then hit "Enter".
!["Enter username"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step24_useraccount.png)

**Step 25 - Enter Password**

  On the next screen - it will ask you to enter a password for this user. Enter the password of your choice - hit tab and press enter to continue.
!["Enter Password"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step25_password.png)

**Step 26 - Re Enter password**

  The next screen will ask you to confirm the password - re enter the same password and continue.
!["Re Enter password"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step26_confirm_password.png)

**Step 27 - Encrypt Home Directory**

  The next step will ask if you want to encrypt your home directory - we dont need that here - so will select "No".
!["Encrypt Home Directory"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step27_Encrypt_Home.png)

**Step 28 - Select Time Zone**

  Based on your location, it will automatically get your time zone. If it is correct - just press "Enter" and continue.
!["Select Time Zone"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step28_Timezone.png)

**Step 29 - Partition Disks**

  Here we need to select a partitioning method. Select the first option "Guided - use entire disk" and press Enter
!["Partition Disks"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step29_Partition_Disk.png)

**Step 30 - Select disk to partition**

  It will show only one disk - just press Enter.
!["Select disk to partition"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step30_partition_disk_name.png)

**Step 31 - Write changes to disk**

  Here it will ask you to confirm if you want to write the changes to disk. Select "Yes" and press "Enter"
!["Write changes to disk"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step31_Write_to_Disk.png)

**Step 32 - Install base system**
!["Install base system"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step32_Installing_Base_system.png)

**Step 33 - HTTP proxy information**

  Leave this empty and continue.
!["HTTP proxy information"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step33_Proxy.png)

**Step 34 - Managing upgrades**

  Just select the default option "No Automatic Updates" and continue.
!["Managing upgrades"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step34_Updates.png)

**Step 35 - Software Selection**

  Here we need to select what software we want to install. For the time being just select OpenSSH server. To select press "Space bar" - then hit tab and press "Enter" to continue.
!["Software Selection"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step35_Install_Openssh.png)

**Step 36 - Install Grub boot loader**

  Select "Yes" to install Grub boot loader.
!["Install Grub boot loader"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step36_Grub_Install.png)

**Step 37 - Finish Installation**

  Finally - the Ubuntu installation is complete. Select Continue to proceed.
!["Finish Installation"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step37_Installation_Complete.png)

**Step 38 - Login Prompt**

  Once the installation process completes - the login prompt will come. Enter the username and password we created at the time of installation to login. I had created the user "hadoop" - so I am using that to login.
!["Login Prompt"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step38_login_VM.png)

**Step 39 - Successful Login Screen**

  Once you login - you will see this screen.
!["Successful Login Screen"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step39_successful_login.png)

**Step 40 - Update Ubuntu OS**

  Next step is to update the Ubuntu OS. You need to be connected to internet to do this update. Type in this command
  ```
  sudo apt-get update
  ```
!["Update Ubuntu OS"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step40_Update_Ubuntu.png)

**Step 41 - Ubuntu update complete**

  Once Ubuntu is updated - you will see a screen similar to the one below.
!["Install VirtualBox"](https://s3.amazonaws.com/omairaasim.github.io/images/tutorial/hadoop/tutorial_1_install_virtualbox_ubuntu/Step41_Done_Updating_OS.png)

Hola !  That brings us to the end of this tutorial. We have successfully installed Ubuntu on VirtualBox. This is the first step required for installing Hadoop.

In the next tutorial - we will start with installing Hadoop on this Virtual machine.
