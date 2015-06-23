---
layout: post
title: Getting Started with Linux
---
<hr>
*In this article we look at the basic commands to getting around Linux. The goal of this tutorial is to introduce the reader to basic navigation and file manipulation in the Linux operating system. We seek for the user to begin to use the terminal a lot more by the end of the this article.*
<hr>

### 1. Terminal Emulator

If you come a Windows background, chances are you have never interacted with a Terminal. Those of you with OS X experience may have once of twice, but much like Windows users, it was never used to its full potential. In Linux the terminal is the most efficient way one can interact with the operating system. There are applications that allows us to visually use Linux, but none come close the efficiency that is experienced using the terminal.

Opening the Terminal:

To open the terminal simply use the shortcut `Ctrl-ALt-t`. This will bring up a window similar to this:
<center>![The terminal](/CSUEB-Big-Data/assets/the-terminal.png)</center>
Where the entry on the window idicates that the user:`ergz` is logged in at the computer: `ergz-X140e` in this session.

### 2. Navigation

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

### 3. File and Folder Manipulation

Now that we can navigate the system, we will begin to create files and folders. We begin with folders

#### 3.1 Creating and Deleting Directories

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

#### 3.2 Renaming and Moving Directories

Renaming and moving directories both use the same command in Linux, `mv`. When using it to rename what we are doing is moving it to the current directory with a different name. Lets us now try some of these commands out.

Lets first create the folder `Data` we had just deleted. We first want to rename it to `Datasets`.
<center>
![REname folder](/CSUEB-Big-Data/assets/terminal-rename-folder.png)
</center>

Next lets move `Datasets` into the `Documents` directory
<center>![move folder](/CSUEB-Big-Data/assets/terminal-move-folder.png)</center>

#### 3.3 Creating and Deleting Files

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