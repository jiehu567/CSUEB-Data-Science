---
layout: page
mathjax: true
title: Getting Started with Linux
---
<hr>
*In this article we look at the basic commands to getting around Linux. The goal of this tutorial is to introduce the reader to basic navigation and file manipulation in the Linux operating system. We seek for the user to begin to use the terminal a lot more by the end of the this article.*
<hr>
<a name='top'></a>
## Table of Content

- [1. Terminal Emulator](#Terminal)
- [2. Navigation](#navigation)
- [3. File and Folder Manipulation](#manipulation)
  - [3.1 Creating and Deleting Directories](#creatingAndDeletingDirs)
  - [3.2 Renaming and Moving Directories](#renamingAndMovingDirs)
  - [3.3 Creating and Deleting Files](#creatingAndDeletingFiles)
- [4. Installing Applications](#installingApplications)  
  - [4.1 Install R](#installR)


<a name='Terminal'></a>
## 1. Terminal Emulator 
[Top](#top)

If you come a Windows background, chances are you have never interacted with a Terminal. Those of you with OS X experience may have once of twice, but much like Windows users, it was never used to its full potential. In Linux the terminal is the most efficient way one can interact with the operating system. There are applications that allows us to visually use Linux, but none come close the efficiency that is experienced using the terminal.

Opening the Terminal:

To open the terminal simply use the shortcut `Ctrl-ALt-t`. This will bring up a window similar to this:
<center>![The terminal](/CSUEB-Big-Data/assets/the-terminal.png)</center>
Where the entry on the window idicates that the user:`ergz` is logged in at the computer: `ergz-X140e` in this session.

<a name='navigation'></a>
## 2. Navigation
[Top](#top)

In the Linux operating system everything is represented my a folder (directory), from DVD's to USB drives they are all folders, this makes navigation very simple and intuitive. On top of that everything is a set up as a tree stemming from the `/` root directory. Let us explore some commands by navigating the OS.

**Listing Directories**

Open the terminal and type `ls`. The `ls` command will list all directories in the current directory. What directory am I in? Simply enter the command `pwd` to print the current working directory.

Here is some sample output of the two commands above
<center>
![terminal ls and pwd](/CSUEB-Big-Data/assets/terminal-ls-pwd.png)
</center>

**Changing Directories**

Once we know what directories we have, we can navigate to them with the command `cd` for change directory. We now also introduce some directory shortcuts that we will explore with the `cd` command

* `.` stands for current directory

* `..` stands for parent directory

* `~` stands for home directory

The output will show navigation into the `Documents` folder followed by navigation to the Downloads folder and back home, each time a new directory is entered we print `pwd`.
<center>
![terminal navigation](/CSUEB-Big-Data/assets/terminal-navigation.png)
</center>

play around with the commands until you have a good feel for it.

*When using the `ls` command it is possible to get more details, specifically to show folders in list view with the `-l` option. So in the terminal one would type `ls -l`*
<cente>![ls -l](/CSUEB-Big-Data/assets/terminal-ls-l.png)</center>

Here we see more detail for everything that is in our directory. We will explore what each means later on.

<a name='manipulation'></a>
## 3. File and Folder Manipulation
[Top](#top)  

Now that we can navigate the system, we will begin to create files and folders. We begin with folders

<a name='creatingAndDeletingDirs'></a>
### 3.1 Creating and Deleting Directories

**Create A Directory**

Lets create a directory called `Data` to do so we use the command `mkdir`, the terminal the command will look like:

```bash
mkdir Data
```

Doing so in the terminal will return nothing, but using `ls` aftewards will confirm the creation of this new folder.

**Deleting a Directory**

To delete this newly created directory we will use the command `rmdir`, in the terminal this will look like:

```bash
rmdir Data
```

an `ls` will confirm this deletion.

<a name="renamingAndMovingDirs"></a>
### 3.2 Renaming and Moving Directories


Renaming and moving directories both use the same command in Linux, `mv`. When using it to rename what we are doing is moving it to the current directory with a different name. Lets us now try some of these commands out.

Lets first create the folder `Data` we had just deleted. We first want to rename it to `Datasets`.
<center>
![REname folder](/CSUEB-Big-Data/assets/terminal-rename-folder.png)
</center>

Next lets move `Datasets` into the `Documents` directory
<center>![move folder](/CSUEB-Big-Data/assets/terminal-move-folder.png)</center>

<a name="creatingAndDeletingFiles"></a>
### 3.3 Creating and Deleting Files


**Create A File**

To create a generic file we use the `touch` command. With the `touch` command we can specify a file extension or not. In the terminal using it would look like this:

```bash
touch myFile.txt
```

an `ls` will confirm the creation of this file.

**Deleting a File**

Much like deleting a directory we use the `rm` command to delete a file. In the terminal this would look like:

```bash
rm myFile.txt
```

once again an `ls` confirms the deletion of the file. 

**Editing the File**

One can begin to edit the file after it has been created or upon creation of the file. Suppose we want to create yet another file, this file will be called `Hello.txt`. We can create the file and begin editing the file with the command:

```bash 
nano Hello.txt
```

doing so will bring to the terminal based text editor called nano:
![nano example](/CSUEB-Big-Data/assets/nano-example.png)

within nano one can begin to edit the newly created file. 


<a name="installingApplications"></a>
## 4. Installing Applications 
[Top](#top)

If you are familiar with either the Apple app store or the Android Market place then you are pretty familiar with the basic elements of the **Aptitude repository** used by Ubuntu and other Debian based Linux distributions. The aptitude repository is essentially an app store for different applications, plug-ins, and other resources that are available for Ubuntu. Since they are in the repository one can feel 100% safe and confident of the functionality of all these applications. Although there are some Graphical Interfaces that can be used to interact with the repository, once again the best way to do this is through the command line. We follow up on our previous task of creating a file by installing a different text editor we will use to edit this file. 

First lets take a look at the command that will install emacs on our Ubuntu machine, then we will look at all the parts that we typed.  

```bash 
sudo apt-get install emacs 
```

1. `sudo` stands for super user do. This command tells Linux that we have permissions to install an application on the computer. This command will also prompt for the user password, in order to complete the install.

2. `apt-get` is the command that interacts with the aptitude repository. On its own it does nothing.

3. `install` here we give an instruction to the `apt-get` command. We are now ready to install an application from the aptitude repository.

4. `emacs` is the application we wish to install. *Note: presssing tab will give you a HUGE list of all the auto completions possible, in this case everything in the repository.*

After you enter the password for your user, the installtion will complete and emacs is ready to be used. Now try this command:

```bash 
emacs Hello.txt
```

you can now edit your file with emacs!

<a name = "installR"></a>
### 4.1 Installing R

Installing \\(\texttt{R}\\) form apt repos, will install a good stable version of \\(\texttt{R}\\), but not the latest version lets correct this.

First things first lets open up the file `/etc/apt/sources/list`. This file is a list of servers that contain all of the applications we need. We can do this with the following command:

```bash 
sudo nano /etc/apt/sources.list
```

*note: the sudo is required for the opening of the file since we accesing a file that is in the root directory, one should always be cautious when editing files in root directories.*

Once we have this file open we need to add a new source where we can get the latest \\(\texttt{R}\\) and \\(\texttt{R}\\) packages. So from rcran we get this: 
`deb http://cran.cnr.Berkeley.edu/bin/linux/ubuntu trusty/` we add this at the end of the file. 

It is good to get used to adding comments whenever we modify a file, I would add it like this:

```bash   
# for latest R and R packages hosted an UC Berkeley, CA 
deb http://cran.cnr.Berkeley.edu/bin/linux/ubuntu trusty/
```

the `#` indicates a comment.

the last thing we need to do is add a secure APT key so that the new source can be trusted. To do this we simply run the following via the command line. 

```bash 
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
``` 

Now we run the following to update all of our sources at apt.

```bash 
sudo apt-get update
```

and to install \\(\texttt{R}\\):

```bash 
sudo apt-get install r-base
```

[Top](#top)
