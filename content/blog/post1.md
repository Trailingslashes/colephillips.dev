---
title: "Building an Active Directory lab in Proxmox"
date: 2023-07-25
weight: 1
tags: ["homelab", "windows", "active directory", guide]
author: "Cole Phillips"
showToc: true
TocOpen: true
draft: false
hidemeta: false
comments: false
description: "Active directory attack lab setup"
canonicalURL: "http://colephillips.dev/blog/page/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/logo-active-directory.png" # image path/url
    alt: "Active Directory logo" # alt text
    caption: "" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/Trailingslashes/colephillips.dev/tree/main/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 1. Lab Requirements

For this lab, you will need at least 60GB of hard drive space for each VM, and a minimum of 16GB RAM. In the following section, I will provide links for the necessary ISO downloads. The required virtual machines are as follows:

- 1 Windows Server (2019) VM
- 2 Windows 10 Enterprise VM's

## 2. ISO Downloads

Microsoft evaluation center has all the necessary 64 bit ISOs to get started - [Link to ISOs](https://www.microsoft.com/en-us/evalcenter)
Select Windows -> Windows 10 Enterprise. Select "Download the ISO"
![Microsoft Eval screenshot](https://imgur.com/3Pwrtb6.png)
Regrettably, Microsoft requires you to complete a form before initiating the download. This step needs to be repeated for Windows Server. Once you're done, navigate back to the eval center and select "Windows Server" from the navigation bar.

![Eval Center](https://imgur.com/vUqPjCr.png)

## 3. Upload the ISOs To Proxmox

As of this writing, I'm currently using Proxmox version 8.0.3. Any recent version will do just fine.

I have installed Proxmox on a 1TB SSD. Your setup may look different from mine. Under Datacenter head to local(pve). Select ISO Images. From there, you can upload the eval ISOs we downloaded earlier.
![ISO images location](https://imgur.com/1eqTu1S.png)

Select the file and upload.
![uploading screenshot](https://imgur.com/l2rOwY7.png)

## 4. VM Configuration

Select the Create VM button to get started.
Name the VM under the General Tab.
![VM - General](https://imgur.com/Rk62dou.png)
Select the ISO.
![VM - ISO](https://imgur.com/gGQoQsK.png)
Leave everything as default under system.
![VM - System](https://imgur.com/zI6dd50.png)
Defaults are also fine here. For this tutorial, I'm not too worried about disk performance, but if you are, VirtIO is the best for performance. You are required to install VirtIO drivers for Windows if you decide to select this option. For more info on this, go here - [Proxmox Documentation](https://pve.proxmox.com/wiki/Performance_Tweaks)
![VM - Disks](https://imgur.com/7QeeakN.png)
Usually, you should select for your VM a processor type which closely matches the CPU of the host system. The one downside is that if you need to do a live migration between different hosts, your VM might end up with a different CPU and [microcode](https://en.wikipedia.org/wiki/Microcode) version. In this case, I will leave the default CPU `x86-64-v2-AES`. `x86-64-v2-AES` is a generic type that simply copies the cpu at hand. `x86-64-v2-AES` is also the new minimum for [RHEL 9](https://developers.redhat.com/blog/2021/01/05/building-red-hat-enterprise-linux-9-for-the-x86-64-v2-microarchitecture-level#).
![VM - CPU](https://imgur.com/QEBu44S.png)
4GB of RAM will be enough for this lab.
![VM - RAM](https://imgur.com/owy47mY.png)
For our network, I have created two bridges - vmbr0 and vmbr1. 0 is for internet and 1 is for internal networks only. I will be using our internal network only for this lab. Intel E1000 is fine. If I was building this lab for performance, I would go with the VirtIO network card.
![VM - Network](https://imgur.com/AtsKptr.png)
![My network Config](https://imgur.com/ByDr8d4.png)
Select Finish.
![VM - Finish](https://imgur.com/G8rm1Qh.png)
For our Windows 10 Enterprise VMs, the process is the same. We will select the same settings from our Windows Server 2019 domain controller. Create two VMs.
![VM - Windows 10](https://imgur.com/K6ky246.png)

## 5. Configure The Domain Controller

Start the VM, then double click on the VM to bring up the VNC console window.
Select Next, Install now.
![Windows server install](https://imgur.com/YBB52Do.png)
Select Standard Evaluation (Desktop Experience)
![Windows server](https://imgur.com/tn69Shj.png)
Select Next, and then select the terms.
Select Drive 0 and click Next.
![windows server](https://imgur.com/tG233Dr.png)
Once the files have finished copying to the disk, create an admin password, and then select finish.
First, let's rename the our DC. Right click on "This PC" and select Properties.
![PC Settings](https://imgur.com/uPkpuOL.png)
Select Change settings.
Select Change.
I'll name my domain controller DC1. Select OK. Reboot.
![DC1](https://imgur.com/gQSqqtt.png)
Upon login, server manager appears. Select Manage at the top -> Add roles and features.
![Roles and features](https://imgur.com/JQnVhSx.png)
Select Next, then Role-based installation.
Under server Roles, select Active Directory Domain Services.
![Roles and features](https://imgur.com/BP826Or.png)
Select Next, Install.
Once the install is finished, you can select Close.
In server manager, select the yellow exclamation mark next to manage.
It's time to promote the server to a domain controller.
![Promote server](https://imgur.com/PoYuYxk.png)
Add a new forest. I'll use DC1.local.
![New forest](https://imgur.com/bjIFM4r.png)
For the [DSRM](https://en.wikipedia.org/wiki/Directory_Services_Restore_Mode) password, I will use the same password as the admin account.
![DSRM](https://imgur.com/Fg4iufC.png)
Ignore the DNS option for now.
Enter a NetBIOS name.
![netbios name](https://imgur.com/T6MCJSW.png)
Click Next on Paths.
Click Next on Review Options.
Once the prerequisites pass, you may install AD DS. Reboot the VM once the installation has finished.
Tip: To send a Ctrl+Alt+Delete in Proxmox, click the arrow on the left hand side of the window.
![Proxmox](https://imgur.com/UJhF37M.png)
Click the A button, select Ctrl, Alt, Delete (last button in the menu).
![Proxmox](https://imgur.com/lEk4AC1.png)
Let's move on to our Windows 10 PCs.

## 6. Setting Up The User Machines

Start PC1 and 2.
Select Next, and install now.
![Windows](https://imgur.com/jRv1R41.png)
Accept the terms, then select Next.
![Windows](https://imgur.com/jr92nhE.png)
The installer will reboot the machine once the install is finished.
Choose your region, keyboard layout. Because I'm using an internal network with no internet, I will select "I don't have internet" at the bottom left.
![windows install](https://imgur.com/8HtF76v.png)
Select continue with limited setup.
The Windows machines will be joined to the domain later. You may enter any username for now.
![Setup local user](https://imgur.com/KOleFLk.png)
Setup the security questions.
Uncheck all under privacy settings.
![privacy settings](https://imgur.com/ivSvjYs.png)
Just like the DC, we need to rename our VM using the same process.
Let's set up the network.
I need to setup the IP addresses for both machines manually because of my internal network setup.
`DC - 10.0.1.10/24`
`PC1 - 10.0.1.11/24`
`PC2 - 10.0.1.12/24`
![network configuration](https://imgur.com/oVNC9Pb.png)
Let's perform a ping from our DC to the Windows 10 VMs.
![Ping check](https://i.imgur.com/lVXCfkl.png)
Everything looks good.
Now we are ready to setup the domain users,groups and policies.

## 7. Setting Up Users, Groups and Policies

On our DC, head to Tools -> Active Directory Users And Computers.
![AD users and computers](https://i.imgur.com/TOpjRea.png)
Time to create some users for our lab environment.
Bob Frank with a password of `P@$$w0rd!1`
Alice Frank with a password of `P@$$w0rd!2`
Ava Holland with a password of `P@$$w0rd!3` Ava will be our new Domain Admin.

Right click on your domain -> New -> User.
![New user creation](https://imgur.com/BWhYF1b.png)
Enter in Bobs details.
![Bobs details](https://imgur.com/UwH16fV.png)
Enter a password. I'm going to set the password to never expire.
![Password setup](https://imgur.com/KOinEZm.png)

Right click on Ava and select properties.
![Ava properties](https://imgur.com/21xBQoV.png)
Select the Member Of tab. Select Add. Search and add all the entries listed below:

- Administrators
- Domain Admins
- Enterprise Admins
- Group Policy Creator
- Schema Admins
  ![Ava admin](https://imgur.com/8uE0jbS.png)
  Click Apply, then Done.

## 8. Joining Our Machines To The Domain

Open a console to PC1. Click the start menu and search for Access work or school.
![join domain](https://imgur.com/xX7aYwT.png)
Click on Connect -> Join this device to a local Active Directory domain.
![join domain](https://imgur.com/URaa0X3.png)
Our domain name is DC1.local. A new window will popup asking for the administrator account. I will use Ava Holland (aholland).
![join domain](https://imgur.com/9MR3ES0.png)
Select Skip. This feature isn't required.
![join domain](https://imgur.com/ljhfIMS.png)
Reboot the machine and repeat the process for PC2.
Our machines are now joined to the domain:
![join domain](https://imgur.com/269IaOi.png)

Now you have a basic AD lab setup. Happy hacking!
