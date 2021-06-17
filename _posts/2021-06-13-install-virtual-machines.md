---
layout: post
title: "Home Virtual Lab: Installing Virtual Machines on VirtualBox"
permalink: /:categories/:year/:month/:day/:title/
---

I was lucky enough to buy a couple of extra computers from an auction, but usually having extra computers and networking computers for experimenting and penetration testing just isn’t feasible for students. So, here are instructions on how to install virtual machines in preparation to set up a virtual lab using Oracle VirtualBox.

Download and install VirtualBox from [Oracle VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads). I’m using a Windows 10 computer as my host machine, so my instructions and screenshots will reflect that, but download the right one for your host.
Before we get to actually creating virtual machines, you’re going to need the files for creating them. There are a couple ways you can go about it. You can find the ISO images for the operating system you want to install, or you can find already-made VMs that have been compressed as an OVA file.

## Install Ubuntu VM with ISO

Download the ISO image from [Ubuntu Downloads](https://ubuntu.com/download/desktop), then open the VirtualBox Manager, then click the **New** option in the menu.

You can name your VM whatever you want, but if you at least start its name with Ubuntu, then VirtualBox recognizes it and selects the Type and Version for you. Otherwise, you’ll have to select _Type:_ **Linux** and _Version:_ **Ubuntu** in the drop-down menus. Accept the default Machine Folder, unless you have a good reason for doing otherwise. Click **Next**.

Next you’ll select how much of your system’s memory you want to allocate to your VM. Remember, the more you give the VM, the less your host system can use. Also, if you have multiple VMs running at the same time, each of them will have their portion of your memory, thus reducing the amount available to your host machine even more. For example, if you have 8 GB total, and you give 2 GB to VM1, and 4 GB to VM2, then you’re only left with 2 GB for your host machine. So allocate wisely. Depending on your usage, you can set your Ubuntu VM memory to as little as 1 GB (1024 MB) and be just fine.

Now you’ll create a virtual hard disk, which is a separate file that acts like a hard disk. Different operating systems will need different amounts of storage. Ubuntu needs at least 10 GB, but you can go bigger if you want. On the next step you’ll decide which format to use for your virtual disk, either VDI (the default for VirtualBox), VHD (also used for Hyper-V), or VMDK (also used for VMware). Then you’ll select either a fixed size to your virtual disk, or if you want VirtualBox to dynamically adjust the storage on it, up to the amount you indicated in the previous step. If you select dynamic, then it won’t use as much storage on your physical disk until it needs the extra space.

Great! Now you have a virtual machine, but it’s still just an empty husk with no operating system. You told VirtualBox what OS you’re going to install, but you haven’t actually installed it yet. Now click Start from the menu and, when prompted, navigate to where you downloaded the ISO and select it. This is equivalent to putting a boot disk on a USB stick into a physical machine to install a new OS. In a moment, the Ubuntu installation guide will load. I’m not going to detail those steps here, but you can find a good guide at [Ubuntu Installation Guide](https://ubuntu.com/tutorials/install-ubuntu-desktop#5-prepare-to-install-ubuntu).

### Guest Additions

After you successfully install Ubuntu, now you have a working VM. However, there is one other thing that is a good idea to do. After the reboot, then in the VirtualBox menu above the VM screen, click **Devices > Insert Guest Additions CD image…** This is a set of features included in your Oracle VirtualBox installation that makes VMs easier to use. When you do this, Ubuntu will think you just inserted a CD into its CD ROM drive. This will enable you to resize your VM screen to your whole physical screen, change it to a good resolution, copy/paste text and files, etc.

Once finished, reboot the machine, then "eject" the Guest Additions CD.

Congratulations! You now have a virtual machine running Ubuntu! The process for installing any other OS is basically the same: acquire the ISO, create a virtual hard disk and allocate memory, attach the ISO to the virtual machine, follow the installation instructions, then add the Guest Additions package.

## Install Kali VM with OVA

The benefit of using an ISO is the ability to fine-tune and customize your installation. However, it is fairly easy to accidentally mess something up. The good news is that if you mess up a VM, just delete and try again. But that's still a hassle. Instead, you can download a machine that's already set up and ready to go. That's what we're going to do now with Kali.

Visit the [Kali Website](https://www.kali.org/get-kali/) and select the Viurtual Machines option. Select your architecture, then download the VirtualBox file or torrent. In VirtualBox, select the **Import** option from the menu, instead of the _New_ option. Click the _Browse_ icon and navigate to the OVA file you just downloaded.

On the right side, you will see a list of settings already installed in the virtual machine that you can import. However, you can modify some of these settings to suit your needs. For example, this OVA was set up with 2 GB of memory and 2 CPUs. If you wanted (and if you had the resources to do so), you could edit those to 4 GB of memory and 4 CPUs. Down below you can select where to store the virtual disk that will get created when you import. When you have your settings how you want them, click **Import**. To import Kali, you must first agree to their terms.

Congratulations! You now have a second VM, and that one was _much_ easier!

Now onto networking them together so they can access the Internet and talk to each other: check out my [Home Virtual Lab: Networking VMs]({% post_url 2021-06-14-network-vms-together %}) project.