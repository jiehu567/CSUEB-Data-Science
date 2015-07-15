---
layout: post
title: Install Linux (Ubuntu 14.04)
---

## What is Linux and Why Do I Need It?

Linux is technically the kernel for the GNU/Linux operating system, but it has become the generic term for any operating system with it as its kernel. So Linux is an operating system, but unlike Windows from Microsoft or OS X from Apple, Linux is an open source software. This means that the source code for the operating system is open for anyone to make it their own with any modifications they need. This makes Linux an operating system driven by the desires and needs of the community, not so much by profit making features and aesthetics. 

So why should you install Linux if you already have Windows or Apple? 

Linux is an operating system that has been around for a very long time, and has matured with flow of the community. It has now become the operating system in the back-end for many companies and servers around the world. But it has also become a very user friendly operating system. Most importantly, as I said before, Linux is an open source Operating System and this is the fundamental reason I believe everyone needs, to at least explore, Linux. The fact that it is driven by communities allows it to be at the bleeding edge of technology. Many technologies are built around Linux and the open source framework such as Hadoop, R, Hive and many others. The knowledge and ability to work in a Linux environment will go a long way, and I strongly believe that in the near future it will be adopted almost entirely in some fields, including statistics.   

## Get and Install Linux 

## 1. Getting a Linux Distribution 

A Linux Distribution is an incarnation of Linux, they differ from each other not so much by performance but by functionality and its interaction with the user. You can check out all the distros at [Distro Watch](http://distrowatch.com/). For this tutorial we will using the Ubuntu Distribution for its ease of use and easy installation, for me this is the most important part. Once we have it installed it, since its open source, we can change it anyway we want. If you would like to learn more about Ubuntu visit the [Ubuntu Website](http://ubuntu.com).

Let us now grab this operating system, our goal is to create a bootable USB from which we can boot and install Linux, it's that easy. We can download the ISO of the operating system here [http://www.ubuntu.com/download/desktop](http://www.ubuntu.com/download/desktop). 

For the most part if your computer was made in the last 4 to 5 years then you can grab the 64-bit version of Ubuntu, otherwise if you have an old computer then 32-bit is the choice for you. 

## 2. Creating a Bootable USB Drive

Once the ISO download is complete we want to create a bootable USB from which we can install Ubuntu. 

1. The first thing we need to get is an application to create the USB for us. For this tutorial we will use [Universal USB Installer](http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/#button). Download the from the link.

2. Make sure you have a USB pen drive that is at least 8GB in size, and whose contents you are willing to erase, and insert into a USB port. 

3. Now just open the USB installer.

4. Select Ubuntu from the dropdown Distro List ![Select Distro List](/CSUEB-Data-Science/assets/usb-universal-ss2.PNG)

5. Click browse and select the ISO image downloaded from Ubuntu ![Select Image](/CSUEB-Data-Science/assets/usb-universal-ss3.PNG)

6. Select the appropriate drive containing your USB. Make sure you select Format for the drive. ![Select USB](/CSUEB-Data-Science/assets/usb-universal-ss4.PNG)

7. Simply click create and just finish until the process is complete.


## 3. Installing Linux

Once the process of creating the bootable USB drive is complete simply plug in the USB into the computer in which you would like to install Linux and restart it, then follow these steps:

1. Your computer should boot into the USB, if your screen turns purple or Ubuntu logo appear you are set. Skip to step 4, otherwise move onto step 2.

2. The main reason a computer does not boot into the USB drive is due to configuration of BIOS. Once again restart your computer, in the process of restarting press the key that boots the computer into the BIOS set up. In most cases this is either F1, F10, F11 or F12. A simple search on Google will provide you with exactly this key. 

3. Now find the menu that provides you with Boot priority. You want to make sure USB/USB Hard Drive is at the top. Now save changes and exit/restart.

4. Once you are booted into Ubuntu you have the option to Boot into Live Mode or Install. **Select Install** ![install option](/CSUEB-Data-Science/assets/installdesktop1.jpg)

5. You will be presented with a wireless set up, simply select the appropriate network and password. 

6. Next you are presented with a checklist window. Make sure all of the requirements are met. I also recommend you select **Download Updates** and **Install Third-Party software**.    
![install option](/CSUEB-Data-Science/assets/installdesktop-2.jpg) 

7. Select **Installation Type**. Here if you wish to run only Ubuntu then select the **replace** option. Otherwise select the **install alongside** option.
![install option](/CSUEB-Data-Science/assets/image-installdesktop-4.jpg)

8. If you selected to replace your operating system simply follow on screen instructions to complete the installation.

9. If you want to install alongside, you must select how much space to allocate for Ubuntu
![install option](/CSUEB-Data-Science/assets/image-installdesktop-5.jpg)
Generally at least 20GB is enough.

10. Once you have entered your location and information you will be asked to restart. 
![install option](/CSUEB-Data-Science/assets/image-installdesktop-10.jpg)
