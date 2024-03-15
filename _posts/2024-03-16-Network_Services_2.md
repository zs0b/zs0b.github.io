---
layout: post
title: Network Services 2
date: 2024-03-16 19:00 +0700
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






















































































