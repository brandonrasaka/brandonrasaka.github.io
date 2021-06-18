---
layout: post
title: "Home Virtual Lab: Networking Virtual Machines Together on VirtualBox"
permalink: /:categories/:year/:month/:day/:title/
---

I was lucky enough to buy a couple of extra computers from an auction, but usually having extra computers and networking computers for experimenting and penetration testing just isn’t feasible for students. In my [previous project]({% post_url 2021-06-13-install-virtual-machines %}), I gave instructions on how to install virtual machines in preparation to set up a virtual lab using Oracle VirtualBox. Here I'm going to explain how to actually allow them to talk to each other and to the Internet.

> I'll expand on this project page later to explain some of the _why_'s, but I wanted to get the steps out there right away.

Assuming you already have 2 VMs, we're going to start by allowing each of them to talk to the Internet.

*   If the VM is running, shut it down (don't just save the machine state, but actually turn it off)
*   With the VM highlighted in VirtualBox, go to **Settings > Network** and make sure the **Enable Network Adapter** box is checked. By default, it should be.
*   Also by default, the _Attached to_ drop-down menu should be on **NAT**. If not, select **NAT**. This will allow the VM to get to the Internet
*   Go ahead and click **OK** to save the settings, then start the VM to check
*   Open a web browser and check if you can reach the Internet
*   Now run `ifconfig` (for Linux) or `ipconfig` (for Windows) to discover your IP address. It should be `10.0.2.15`, since that's the default for the _NAT_ network option

Alright, good. Now it's time to let them talk to each other.

*   Shut down the VMs again, then open up the Network settings for each one
*   In VirtualBox, click **File > Host Network Manager**
*   Click **Create** to make a new adapter. This is basically like getting a new NIC for your computer, but it's also like getting a new router to connect multiple computers together.
*   For the purposes of this tutorial, just select the **Configure Adapter Automatically** option, and on the _DHCP Server_ tab, check the box to **Enable Server**. If you wanted, you could set the network manually, and then also set the IP addresses on each machine manually. You would want to do this if you were hosting a web server, or an FTP server, or something, but in this case the automatic settings are just fine.
*   Click **Apply**
*   Now, for each VM, open the Network Settings again
*   Click on the **Adapter 2** tab and enable it with the checkbox
*   Now, attach _this_ adapter to the Host, but select **Host-only Adapter** from the drop-down menu
*   Click the **Advanced** option, then beside the _MAC Address_ box, click the Refresh icon
*   Click **OK** and then start up your VMs
*   This time when you find the IP addresses, you should still have the `10.0.2.15` address, but it should also have another address in the same range as the Host-only adapter you set up earlier. Find those IP addresses, and then try to ping them from each other.

CONGRATULATIONS! You now have a virtual lab you can use for some basic penetration testing!