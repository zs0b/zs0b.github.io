---
layout: post
title: Network Services 2
date: 2024-03-16 22:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Understanding NFS

### What is NFS?

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

### How does NFS work?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/748e550b-0d07-4b8e-9dae-00d6a88c7d63)

We don't need to understand the technical exchange in too much detail to be able to exploit NFS effectively- however if this is something that interests you, I would recommend this resource: [https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html](https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html)

First, the client will request to mount a directory from a remote host on a local directory just the same way it can mount a physical device. The mount service will then act to connect to the relevant mount daemon using RPC.

The server checks if the user has permission to mount whatever directory has been requested. It will then return a file handle which uniquely identifies each file and directory that is on the server.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

- The file handle

- The name of the file to be accessed

- The user's, user ID

- The user's group ID

These are used in determining access rights to the specified file. This is what controls user permissions, I.E read and write of files.

### What runs NFS?

Using the NFS protocol, you can transfer files between computers running Windows and other non-Windows operating systems, such as Linux, MacOS or UNIX.

A computer running Windows Server can act as an NFS file server for other non-Windows client computers. Likewise, NFS allows a Windows-based computer running Windows Server to access files stored on a non-Windows NFS server.

### More Information:

Here are some resources that explain the technical implementation, and working of, NFS in more detail than I have covered here.

[https://www.datto.com/library/what-is-nfs-file-share](https://www.datto.com/library/what-is-nfs-file-share)

[http://nfs.sourceforge.net/](https://nfs.sourceforge.net/)

[https://wiki.archlinux.org/index.php/NFS](https://wiki.archlinux.org/title/NFS)

### What does NFS stand for?

`Network File System`

### What process allows an NFS client to interact with a remote directory as though it was a physical device?

`Mounting`

### What does NFS use to represent files and directories on the server?

`file handle`

#### What protocol does NFS use to communicate between the server and client?

`RPC`

### What two pieces of user data does the NFS server take as parameters for controlling user permissions? Format: parameter 1 / parameter 2

`user id / group id`

### Can a Windows NFS server share files with a Linux client? (Y/N)

`Y`

### Can a Linux NFS server share files with a MacOS client? (Y/N)

`Y`

### What is the latest version of NFS? [released in 2016, but is still up to date as of 2020] This will require external research.

`4.2`

## Enumerating NFS

### Let's Get Started

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware - this can take up to five minutes so be patient!

### What is Enumeration?

Enumeration is defined as "a process which establishes an active connection to the target hosts to discover potential attack vectors in the system, and the same can be used for further exploitation of the system." - [Infosec Institute](https://resources.infosecinstitute.com/what-is-enumeration/). It is a critical phase when considering how to enumerate and exploit a remote machine - as the information you will use to inform your attacks will come from this stage

### Requirements

In order to do a more advanced enumeration of the NFS server, and shares- we're going to need a few tools. The first of which is key to interacting with any NFS share from your local machine: **nfs-common**.

### NFS-Common

It is important to have this package installed on any machine that uses NFS, either as client or server. It includes programs such as: **lockd, statd, showmount, nfsstat, gssd, idmapd** and **mount.nfs**. Primarily, we are concerned with "showmount" and "mount.nfs" as these are going to be most useful to us when it comes to extracting information from the NFS share. If you'd like more information about this package, feel free to read: [https://packages.ubuntu.com/jammy/nfs-common](https://packages.ubuntu.com/jammy/nfs-common).

You can install **nfs-common** using " *sudo apt install nfs-common* ", it is part of the default repositories for most Linux distributions such as the Kali Remote Machine or AttackBox that is provided to TryHackMe.

### Port Scanning

Port scanning has been covered many times before, so I'll only cover the basics that you need for this room here. If you'd like to learn more about **nmap** in more detail please have a look at the nmap room.

The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, open ports and operating system of the target machine. You can go as in-depth as you like on this, however, I suggest using **nmap** with the **-A** and **-p-** tags.

### Mounting NFS shares

Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed. You can create
this folder anywhere on your system. Once you've created this mount point, you can use the "mount" command to connect the NFS share to the mount point on your machine like so:

`sudo mount -t nfs IP:share /tmp/mount/ -nolock`

Let's break this down

**Tag**	**Function**
sudo	Run as root
mount	Execute the mount command
-t nfs	Type of device to mount, then specifying that it's NFS
IP:share	The IP Address of the NFS server, and the name of the share we wish to mount
-nolock	Specifies not to use NLM locking


Now we understand our tools, let's get started!

### Conduct a thorough port scan scan of your choosing, how many ports are open?

`nmap -T4 -p- -A 10.10.42.219`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fb543499-8ee9-470c-a16d-8a225f686cab)

`7`

### Which port contains the service we're looking to enumerate?

`2049`

### Now, use /usr/sbin/showmount -e [IP] to list the NFS shares, what is the name of the visible share?

`showmount -e 10.10.42.219`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1c944db0-6f9c-4c68-bd2d-f65f9eccbc51)

`/home`

Time to mount the share to our local machine!

First, use "mkdir /tmp/mount" to create a directory on your machine to mount the share to. This is in the /tmp directory- so be aware that it will be removed on restart.

### Then, use the mount command we broke down earlier to mount the NFS share to your local machine. Change directory to where you mounted the share- what is the name of the folder inside?

```
mkdir /tmp/mount
sudo mount -t nfs 10.10.42.219:/home /tmp/mount 
cd /tmp/mount
ls
```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b1334c54-5a8f-4c19-b2ed-d58cc034da5d)

`cappucino`

### Interesting! Let's do a bit of research now, have a look through the folders. Which of these folders could contain keys that would give us remote access to the server?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ab3f6ac6-98eb-4cdf-981a-8fb97808744e)

`.ssh`

### Which of these keys is most useful to us?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6e0030f8-0017-4ecf-ba89-23643951336b)

`id_rsa`

Copy this file to a different location your local machine, and change the permissions to "600" using "chmod 600 [file]".

Assuming we were right about what type of directory this is, we can pretty easily work out the name of the user this key corresponds to.

### Can we log into the machine using ssh -i <key-file> <username>@<ip> ? (Y/N)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fdd5e2a8-28de-4cc8-a807-1e5f7ef3d3b0)

`Y`

## Exploiting NFS

### We're done, right?

Not quite, if you have a low privilege shell on any machine and you found that a machine has an NFS share you might be able to use that to escalate privileges, depending on how it is configured.

### What is root_squash?

By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.

### SUID
So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

### Method

This sounds complicated, but really- provided you're familiar with how SUID files work, it's fairly easy to understand. We're able to upload files to the NFS share, and control the permissions of these files. We can set the permissions of whatever we upload, in this case a bash shell executable. We can then log in through SSH, as we did in the previous task- and execute this executable to gain a root shell!

### The Executable

Due to compatibility reasons, we'll use a standard Ubuntu Server 18.04 bash executable, the same as the server's- as we know from our nmap scan. You can download it [here](https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash). If you want to download it via the command line, be careful not to download the github page instead of the raw script. You can use

`wget https://github.com/polo-sec/writing/raw/master/Security%20Challenge%20Walkthroughs/Networks%202/bash`

Mapped Out Pathway:

If this is still hard to follow, here's a step by step of the actions we're taking, and how they all tie together to allow us to gain a root shell:
```
    NFS Access ->
        Gain Low Privilege Shell ->
            Upload Bash Executable to the NFS share ->
                Set SUID Permissions Through NFS Due To Misconfigured Root Squash ->
                    Login through SSH ->
                        Execute SUID Bit Bash Executable ->
                            ROOT ACCESS
```
Lets do this!

### Now, we're going to add the SUID bit permission to the bash executable we just copied to the share using "sudo chmod +[permission] bash". What letter do we use to set the SUID bit set using chmod?

`s`

### Let's do a sanity check, let's check the permissions of the "bash" executable using "ls -la bash". What does the permission set look like? Make sure that it ends with -sr-x.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5fe2b4e1-e94e-4dcb-a458-9450921840cb)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9a592dde-5588-4021-9b7d-e653aa5f3ae4)

`-rwSr-Sr-x`

### Great! If all's gone well you should have a shell as root! What's the root flag?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8ebfc697-20d1-4b6a-936c-6b86d55a6f33)

`THM{nfs_got_pwned}`



































































