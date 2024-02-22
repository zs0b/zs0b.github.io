---
layout: post
title: Windows Fundamentals Part 1
date: 2024-02-23 8:00 +0700
categories: [TryHackMe, Pre Security]
tags: [Windows Fundamentals, Blue Team]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

##  Windows Editions

The Windows operating system has a long history dating back to 1985, and currently, it is the dominant operating system in both home use and corporate networks. Because of this, Windows has always been targeted by hackers & malware writers.

Windows XP was a popular version of Windows and had a long-running. Microsoft announced Windows Vista, which was a complete overhaul of the Windows operating system. There were many issues with Windows Vista. It wasn't received well by Windows users, and it was quickly phased out.

When Microsoft announced the end-of-life date for Windows XP, many customers panicked. Corporations, hospitals, etc., scrambled and tested the next viable Windows version, which was Windows 7, against many other hardware and devices. Vendors had to work against the clock to ensure their products worked with Windows 7 for their customers. If they couldn't, their customers had to break their agreement and find another vendor that upgraded their products to work with Windows 7. It was a nightmare for many, and Microsoft took note of it.

Windows 7, as quickly as it was released soon after, was marked with an end of support date. Windows 8.x came and left and it was short-lived, like Vista.

Then arrived [Windows 10](https://www.microsoft.com/en-us/windows/tips-and-tricks?activetab=NewPopular) , which is the current Windows operating system version for desktop computers.

Windows 10 comes in 2 flavors, Home and Pro. You can read the difference between the Home and Pro [here](https://www.microsoft.com/en-us/windows/compare-windows-11-home-vs-pro-versions). 

Even though we didn't talk about servers, the current version of the Windows operating system for servers is [Windows Server 2019](https://www.microsoft.com/en-us/windows-server).

Many critics like to bash on Microsoft, but they have made long strides to improve the usability and security with each new version of Windows.

Note: The Windows edition for the attached VM is Windows Server 2019 Standard, as seen in System Information.

Update: As of June 2021, Microsoft announced the retirement dates for Windows 10 [here](https://learn.microsoft.com/en-us/lifecycle/products/windows-10-home-and-pro?ranMID=24542&ranEAID=kXQk6*ivFEQ&ranSiteID=kXQk6.ivFEQ-4cKUPfbv9lM_IR2EX7K_hw&epi=kXQk6.ivFEQ-4cKUPfbv9lM_IR2EX7K_hw&irgwc=1&OCID=AID2000142_aff_7593_1243925&tduid=(ir__feexvhocigkfqna9kk0sohznb32xutanagupypus00)(7593)(1243925)(kXQk6.ivFEQ-4cKUPfbv9lM_IR2EX7K_hw)()&irclickid=_feexvhocigkfqna9kk0sohznb32xutanagupypus00). 

"Microsoft will continue to support at least one Windows 10 Semi-Annual Channel until October 14, 2025".

As of October 5th, 2021 - Windows 11 now is the current Windows operating system for end-users. Read more about Windows 11 [here](https://www.microsoft.com/en-us/windows?wa=wsignin1.0).  

### What encryption can you enable on Pro that you can't enable in Home?

`BitLocker`

## The Desktop (GUI)

The Windows Desktop, aka the graphical user interface or GUI in short, is the screen that welcomes you once you log into a Windows 10 machine.

Traditionally, you need to pass the login screen first. The login screen is where you need to enter valid account credentials; usually, a username & password of a preexisting Windows account on that particular system or in the Active Directory environment (if it's a domain-joined machine). 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d11d81c9-573e-4409-921f-3a92a143bf52)

The above screenshot is an example of a typical Windows Desktop. Each component that makes up the GUI is explained briefly below.

- The Desktop

- Start Menu

- Search Box (Cortana)

- Task View

- Taskbar

- Toolbars

- Notification Area

### The Desktop

The desktop is where you will have shortcuts to programs, folders, files, etc. These icons will either be well organized in folders sorted alphabetically or scattered randomly with no specific organization on the desktop. In either case, these items are typically placed on the desktop for quick access.

The look and feel of the desktop can be changed to suit your liking. By right-clicking anywhere on the desktop, a context menu will appear. This menu will allow you to change the sizes of the desktop icons, specify how you want to arrange them, copy/paste items to the desktop, and create new items, such as a folder, shortcut, or text document.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e9b32f84-82d0-475c-aefe-c111dc0280a2)

Under `Display settings`, you can make changes to the screen's resolution and orientation. In case you have multiple computer screens, you can make configurations to the multi-screen setup here. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f0b187c4-b2d6-4809-a067-cdf981069fde)

`Note`: In a Remote Desktop session, some of the display settings will be disabled. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9f642ecc-c1eb-4b7b-ba22-c993e6c3324d)

You can also change the wallpaper by selecting `Personalize`.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/987ac737-9c17-4012-9124-a2058d7ef99d)

Under Personalize, you can change the background image to the Desktop, change fonts, themes, color scheme, etc. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6d8dc210-47af-4578-a2e2-82f15bddef1a)

### The Start Menu

In previous versions of Windows, the word `Start` was visible at the bottom left corner of the desktop GUI. In modern versions of Windows, such as Windows 10, the word 'Start' doesn't appear anymore, but rather a Windows Logo is shown instead. Even though the look of the Start Menu has changed, its overall purpose is the same. 

The Start Menu provides access to all the apps/programs, files, utility tools, etc., that are most useful. 

Clicking on the Windows logo, the Start Menu will open. The Start Menu is broken up into sections. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1a5f57af-d4f0-4e55-a097-e7ecb2aebdce)

- This section of the Start Menu provides quick shortcuts to actions that you can perform with your account or login session, such as making changes to your user account, lock your screen, or signing out of your account. Other shortcuts specific to your account are your Documents (document icon) folder and Pictures folder (pictures icon). Lastly, the gear/cog icon will take you to the Settings screen, and the power icon will allow you to Disconnect from a Remote Desktop session, shut down the computer, or restart the computer.

In the below image, you can see what each of the icons represents. To expand this section, click on the icon that resembles a hamburger at the top.  

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3c87a129-9ec1-47ab-be9f-fe6211356277)

- This section will show all `Recently added` apps/programs at the top and all the installed apps/programs (that are configured to appear in the Start Menu). In this section, you'll also see the apps/programs will be listed in alphabetical order. Each letter will have its own section. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4ccae272-8d49-4e66-b92f-e57c88b1fe76)

In the above image, the first box is where the recently added apps/programs will appear. The second box is where all the installed apps/programs will appear. 

`Note`: In your VM, Google Chrome will not show up as a Recently Added program anymore.

If you have a LONG list of installed apps/programs, you can jump to a particular section in the list by clicking on the letter headings to launch an alphabet grid. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0d680aa2-d916-497c-86e3-0be37da75047)

`Note`: The white letters match the letter headings. 

- The right side of the Start Menu is where you will find icons for specific apps/programs or utilities. These icons are known as `tiles`. Some tiles are added to this section by default. If you right-click any of the tiles, you guessed it; a menu will appear to allow you to perform more actions on the selected tile; such as resizing the tile, unpinning from Start Menu, view its Properties, etc. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/906f0997-7227-4dfb-9d2f-b9cdf84fcde8)

Apps/programs can be added to this Start Menu section by right-clicking the app/program and selecting Pin to Start. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5250b8fd-5069-4842-a142-ba5410fbe202)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/06866d63-d326-496b-99aa-faad85c6d06c)

### The Taskbar

Some of the components are enabled and visible by default. The Toolbar (6), for example, was enabled for demonstration purposes.  

If you're like me and want to disable some of these components, you can right-click on Taskbar to bring up a context menu that will allow you to make changes.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2f150045-0b36-4e4c-9e32-9366785c4173)

Any apps/programs, folders, files, etc., that you open/start will appear in the taskbar. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/db6e4fba-1f8b-4c9b-9b78-8fdd43c81c42)

Hovering over the icon will provide a preview thumbnail, along with a tooltip. This  tooltip is handy if you have many apps/programs open, such as Google Chrome, and you wish to find which instance of Google Chrome is the one you need to bring in to focus. 

When you close any of these items, they will disappear from the taskbar (unless you explicitly pinned it to the taskbar). 

### The Notification Area

The Notification Area, which is typically located at the bottom right of the Windows screen, is where the date and time are displayed. Other icons possibly visible in this area is the volume icon, network/wireless icon, to name a few. Icons can be either added or removed from the Notification Area in Taskbar settings. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6eb7245a-0c38-43b9-8733-5450cbbcc628)

From there, scroll down to the Notification Area section to make changes. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/765bb9fb-ecd4-4ccc-8e58-7136dc994b9c)

Here are Microsoft's brief documents for the [Start Menu](https://support.microsoft.com/en-us/windows/see-what-s-on-the-start-menu-a8ccb400-ad49-962b-d2b1-93f453785a13) and  [Notification Area](https://support.microsoft.com/en-us/windows/customize-the-taskbar-notification-area-e159e8d2-9ac5-b2bd-61c5-bb63c1d437c3#WindowsVersion=Windows_10).

`Tip`: You can right-click any folder, file, app/program, or icon to view more information or perform other actions on the clicked item. 

### Which selection will hide/disable the Search box?

`Hidden`

### Which selection will hide/disable the Task View button?

`Show Task View button`

### Besides Clock and Network, what other icon is visible in the Notification Area?

`Action Center`

## The File System

The file system used in modern versions of Windows is the `New Technology File System` or simply [NTFS](https://learn.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview).

Before NTFS, there was `FAT16/FAT32` (File Allocation Table) and `HPFS` (High Performance File System). 

You still see FAT partitions in use today. For example, you typically see FAT partitions in USB devices, MicroSD cards, etc. but traditionally not on personal Windows computers/laptops or Windows servers.

NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.   

NTFS addresses many of the limitations of the previous file systems; such as: 

- Supports files larger than 4GB

- Set specific permissions on folders and files

- Folder and file compression

- Encryption ([Encryption File System](https://learn.microsoft.com/en-us/windows/win32/fileio/file-encryption) or EFS)

If you're running Windows, what is the file system your Windows installation is using? You can check the Properties (right-click) of the drive your operating system is installed on, typically the C drive (C:\).

You can read Microsoft's official documentation on FAT, HPFS, and NTFS [here](https://learn.microsoft.com/en-us/troubleshoot/windows-client/backup-and-storage/fat-hpfs-and-ntfs-file-systems). 

Let's speak briefly on some features that are specific to NTFS. 

On NTFS volumes, you can set permissions that grant or deny access to files and folders.

The permissions are:

- Full control

- Modify

- Read & Execute

- List folder contents

- Read

- Write

The below image lists the meaning of each permission on how it applies to a file and a folder. (credit [Microsoft](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/bb727008(v=technet.10)?redirectedfrom=MSDN))

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/84a69d9b-5c53-457d-ba54-86d4e8084d31)

How can you view the permissions for a file or folder?

- Right-click the file or folder you want to check for permissions.

- From the context menu, select `Properties`.

- Within Properties, click on the `Security` tab.

- In the `Group or user names` list, select the user, computer, or group whose permissions you want to view.

In the below image, you can see the permissions for the `Users` group for the Windows folder. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/000d2bba-3cc9-4abd-bb57-549ff90dcf12)

Refer to the Microsoft documentation to get a better understanding of the NTFS permissions for `Special Permissions`.

Another feature of NTFS is `Alternate Data Streams (ADS)`.

`Alternate Data Streams` (ADS) is a file attribute specific to Windows `NTFS` (New Technology File System).

Every file has at least one data stream (`$DATA`), and ADS allows files to contain more than one stream of data. Natively [Window Explorer](https://support.microsoft.com/en-us/windows/find-and-open-file-explorer-ef370130-1cca-9dc5-e0df-2f7416fe1cb1) doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but [Powershell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.4&viewFallbackFrom=powershell-7.1) gives you the ability to view ADS for files.

From a security perspective, malware writers have used ADS to hide data.

Not all its uses are malicious. For example, when you download a file from the Internet, there are identifiers written to ADS to identify that the file was downloaded from the Internet.

To learn more about ADS, refer to the following link from MalwareBytes [here](https://www.malwarebytes.com/blog/news/2015/07/introduction-to-alternate-data-streams). 

`Bonus`: If you wish to interact hands-on with ADS, I suggest exploring Day 21 of Advent of Cyber 2.

### What is the meaning of NTFS?

`New Technology File System`

## The Windows\System32 Folders

The Windows folder (`C:\Windows`) is traditionally known as the folder which contains the Windows operating system. 

The folder doesn't have to reside in the C drive necessarily. It can reside in any other drive and technically can reside in a different folder.

This is where environment variables, more specifically system environment variables, come into play. Even though not discussed yet, the system  environment variable for the Windows directory is `%windir%`.

Per [Microsoft](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.4&viewFallbackFrom=powershell-7.1) , "Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders".

There are many folders within the 'Windows' folder. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6e009435-cc6f-45f4-b550-9ad6a1240198)

One of the many folders is `System32`. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/43a729be-ecf0-4c59-b615-0429c8d6dca5)

The System32 folder holds the important files that are critical for the operating system.

You should proceed with extreme caution when interacting with this folder. Accidentally deleting any files or folders within System32 can render the Windows OS inoperational. Read more about this action [here](https://www.howtogeek.com/346997/what-is-the-system32-directory-and-why-you-shouldnt-delete-it/). 

`Note`: Many of the tools that will be covered in the Windows Fundamentals series reside within the System32 folder. 

### What is the system variable for the Windows folder?

`%windir%`

## User Accounts, Profiles, and Permissions

User accounts can be one of two types on a typical local Windows system: `Administrator` & `Standard User`. 

The user account type will determine what actions the user can perform on that specific Windows system. 

- An Administrator can make changes to the system: add users, delete users, modify groups, modify settings on the system, etc. 

- A Standard User can only make changes to folders/files attributed to the user & can't perform system-level changes, such as install programs.

You are currently logged in as an Administrator. There are several ways to determine which user accounts exist on the system. 

One way is to click the `Start Menu` and type `Other User`. A shortcut to `System Settings > Other users` should appear. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/af8c1514-6a06-43e2-bde6-f711989c6ae2)

If you click on it, a Settings window should now appear. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e2a2d30a-5d31-443e-b7bf-4b9752612acc)

Since you're the Administrator, you see an option to `Add someone else to this PC`.

`Note`: A Standard User will not see this option.  

Click on the local user account. More options should appear: `Change account type and Remove`. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/72362998-31a4-44ac-8993-7bd6458f1fc7)

Click on Change account type. The value in the drop-down box (or the highlighted value if you click the drop-down) is the current account type. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7030fd7a-224e-455d-b665-6f3d4ffa72e5)

When a user account is created, a profile is created for the user. The location for each user profile folder will fall under is C:\Users.

For example, the user profile folder for the user account Max will be C:\Users\Max.

The creation of the user's profile is done upon initial login. When a new user account logs in to a local system for the first time, they'll see several messages on the login screen. One of the messages, User Profile Service, sits on the login screen for a while, which is at work creating the user profile. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/dfb20d36-8135-4e01-ad3d-fdf44b338ae4)

Once logged in, the user will see a dialog box similar to the one below (again), indicating that the profile is in creation.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/43084cff-139b-46f0-a76d-81b9e5a12ff5)

Each user profile will have the same folders; a few of them are:

- Desktop

- Documents

- Downloads

- Music

- Pictures

Another way to access this information, and then some, is using `Local User and Group Management`. 

Right-click on the Start Menu and click `Run`. Type `lusrmgr.msc`. See below

`Note`: The Run Dialog Box allows us to open items quickly. 

Back to lusrmgr, you should see two folders: `Users` and `Groups`. 

If you click on Groups, you see all the names of the local groups along with a brief description for each group. 

Each group has permissions set to it, and users are assigned/added to groups by the Administrator. When a user is assigned to a group, the user inherits the permissions of that group. A user can be assigned to multiple groups.

`Note`: If you click on `Add someone else to this PC` from `Other users`, it will open `Local Users and Management`. 

### What is the name of the other user account?

`tryhackmebilly`

### What groups is this user a member of?

`Remote Desktop Users,Users`

### What built-in account is for guest access to the computer?

`Guest`

### What is the account status?

`Account is disabled`

## User Account Control

The large majority of home users are logged into their Windows systems as local administrators. Remember from the previous task that any user with administrator as the account type can make changes to the system.

A user doesn't need to run with high (elevated) privileges on the system to run tasks that don't require such privileges, such as surfing the Intrnet, working on a Word document, etc. This elevated privilege increases the risk of system compromise because it makes it easier for malware to infect the system. Consequently, since the user account can make changes to the system, the malware would run in the context of the logged-in user.

To protect the local user with such privileges, Microsoft introduced `User Account Control` (UAC). This concept was first introduced with the short-lived [Windows Vista](https://en.wikipedia.org/wiki/Windows_Vista)  and continued with versions of Windows that followed.

`Note`: UAC (by default) doesn't apply for the built-in local administrator account. 

How does UAC work? When a user with an account type of administrator logs into a system, the current session doesn't run with elevated permissions. When an operation requiring higher-level privileges needs to execute, the user will be prompted to confirm if they permit the operation to run. 

Let's look at the program on the account you're currently logged into, the built-in administrator account—Right-click to view its Properties.

In the Security tab, we can see the users/groups and their permissions to this file. Notice that the standard user is not listed. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7c6f2b21-b21b-461d-81e4-3b456908ebaf)

Log in as the standard user and try to install this program. To do this, you can remote desktop into the machine as the standard user account. 

`Note`: You have the username and password for the standard user. It's visible in `lusrmgr.msc`.

Before installing the program, notice the icon. Do you see the difference? When you're logged in as the standard user, the shield icon is on the program's default icon. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3788e6b8-73e1-4b50-8819-feaf32adf4ca)

This shield icon is an indicator that UAC will prompt to allow higher-level privileges to install the program.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/67d0593a-5ff1-4a0b-922d-1a78e3ea605f)

Double-click the program, and you'll see the UAC prompt. Notice that the built-in administrator account is already set as the user name and prompts the account's password. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fc5fadf3-b79f-4e39-84ad-cacaf1cddb94)

After some time, if a password is not entered, the UAC prompt disappears, and the program does not install. 

This feature reduces the likelihood of malware successfully compromising your system. You can read more about [UAC](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/how-it-works) here.

### What does UAC mean?

`User Account Control`

## Settings and the Control Panel

On a Windows system, the primary locations to make changes are the Settings menu and the Control Panel.

For a long time, the Control Panel has been the go-to location to make system changes, such as adding a printer, uninstall a program, etc. 

The Settings menu was introduced in Windows 8, the first Windows operating system catered to touch screen tablets, and is still available in Windows 10. As a matter of fact, the Settings menu is now the primary location a user goes to if they are looking to change the system. 

There are similarities and differences between the two menus. Below are screenshots of each.

`Settings`:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9365eed3-86f4-446b-9dda-6ececdc74ab4)

`Control Panel`: 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/90ab4545-a576-4ceb-9ccc-b21854eaa3a4)

`Note`: The icons for Settings might be different in the version of Windows on your personal device. 

Both can be accessed from the Start Menu. See below.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f36485f4-9360-4b1f-b0f3-7d4baeb4013c)

Control Panel is the menu where you will access more complex settings and perform more complex actions. In some cases, you can start in Settings and end up in the Control Panel.

For example, in Settings, click on `Network & Internet`. From here, click on `Change adapter options`. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5a09c761-284d-48d5-8c11-e9ca2025f486)

Notice that the next window that pops up is from the Control Panel. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d3e55bf4-d43e-4fec-b53d-4bab9542164e)

If you're unclear which to open if you wish to change a setting, use the Start menu and search for it. 

In the example below, the search was 'wallpaper.' Notice that few results were returned. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d02906cd-4e1a-4112-bb32-748cc9735953)

If we click on the Best match, a window to the Settings menu appears to make changes to the wallpaper. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d5550b35-4b9b-4282-92df-9f3064047be3)

### In the Control Panel, change the view to Small icons. What is the last setting in the Control Panel view?

`Windows Defender Firewall`

## Task Manager

The last subject that will be touched on in this module is the `Task Manager`.

The Task Manager provides information about the applications and processes currently running on the system. Other information is also available, such as how much CPU and RAM are being utilized, which falls under `Performance`. 

You can access the Task Manager by right-clicking the taskbar. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/29bc6d31-3d34-47d8-b3c1-0692584c334c)

Task Manager will open in Simple View and won't show much information. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6635bd5c-c7ab-45a4-999f-c3a265909a9e)

Click on `More details`, and the view changes.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ac53934a-d92a-4845-83df-edb16605fb95)

You can refer to this [blog post](https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/) for more detailed information about the Task Manager.

If you wish to learn more about the core Windows processes and what each process is responsible for, visit the Core Windows Processes room. 

### What is the keyboard shortcut to open Task Manager?

`Ctrl+Shift+Esc`

goodbye, thank you for reading until now //~//






