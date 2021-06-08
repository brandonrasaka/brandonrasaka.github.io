---
layout: post
title: Physical Network Testing Environment
---

My work at Marcraft has been setting up a physical testing environment for Industrial Control Systems (ICS) cyber security. Most of the time, when people set up a cyber security testing environment, it is a virtual one, either through virtualization software like VMware or VirtualBox. However, we wanted to be as realistic as possible, so we made sure to use real, physical devices and cables.

This was a great learning experience for me, especially while having attended school in the COVID era -- all my classes were online, so I never got to touch all of these devices in class. There are many experiences you just can't get when using VMs or learning programs like TestOut.

![Network Topology](/assets/images/marcraft-lab-topology.png)

Here I describe the setup, and you can examine the network topology above.

## ISP Simulator

The ISP Simulator is a Raspberry Pi with two Ethernet interfaces. One of them is built-in and the other is an Ethernet-to-USB adapter. The ISP Simulator is set up with DNS and DHCP settings for both Ethernet interfaces, allowing it to simulate an ISP.

## Attack PC

The Attack PC is a laptop that I partitioned and set up to dual boot Windows 10 and Kali Linux operating systems.

## Attack Router

The Attack Router is a Netgear WNDR3400v3 router that serves as the gateway for the Attacker’s home network, with a private IP address of 192.168.1.1. It has a rule to forward incoming traffic to port 4444 to the Attack PC, to be used for a Meterpreter shell. When connected to the ISP Simulator, it obtained a public IP address of 10.2.50.16.

## Edge Router

The Edge Router is the main gateway router for the target, which is also a Netgear router. When connected to the ISP Simulator, it obtained a public IP address of 10.2.51.144, and its private IP address is 192.168.2.1. Because the target website is hosted from behind this router, it allows port 80 through to the web server at 192.168.2.25. It is also set up for remote management capability, which can be accessed via port 8443.

## Cisco Switch

This is a Cisco Catalyst 3750. At the time of this writing, it has been programmed with only one VLAN, which includes all 24 ports.

## ICS Router

This is another Netgear router that basically acts as a Layer 3 switch. Wireless transmission and DHCP have been disabled.

## Industrial Process

The industrial process is an actual, working miniaturized factory with multiple motors running a conveyor belt, a turn wheel, and a gantry. It includes and is controlled by the two programmable logic controllers (PLCs). The Industrial Process also includes multiple IR beam sensors for detecting work pieces that flow through the process.

## PLC 1 and PLC 2

These are Allen-Bradley Micro820 PLCs that I programmed with Ladder Logic using Connected Components Workbench (CCW). They each have statically-assigned IP addresses.

## Engineering Station

The Engineering Station, together with the PLCs, make up a supervisory control and data acquisition (SCADA) system. The Engineering Station can program the PLCs and remotely control them using a human-machine interface (HMI). It has a static private IP address of 192.168.2.4 and has SSH enabled. Another function of the Engineering Station is to write logs for each IR beam sensor and send them to the Data Historian for long-term storage. The logs are delivered with SMB.

## Data Historian

The Data Historian is a Windows 10 PC with a WAMP database server for storing the logs. It receives the log files via SMB from the Engineering Station, then auto-uploads them to the server.

## Windows Server/Virtual Switch

This is a physical server running Windows Server 2019. Its primary function is to allocate resources for various virtual machines. As of the time that I’m writing this, there is only one VM, which hosts the Web Server.

## Web Server

This is a Windows 10 VM that and uses XAMPP to host the target’s website which, in reality, is the Damn Vulnerable Web Application.

* * *

# Problems I ran into:

## MySQL/phpMyAdmin upload

I really wanted to get the Engineering Station to automatically upload the logs directly to the database server, but couldn’t get it to work. So, I came up with an alternate solution until I can get it to work right. On the Data Historian, I enabled sharing on the folder to store the logs locally, then on Engineering Station, I mapped a network drive to the shared folder. The way the HMI was written, it doesn’t actually generate multiple log files, rather it appends the data to an existing txt file. So, I wrote a PowerShell script that uploads the file to the database, and MySQL only accepts the new records (based on the timestamp). Then I added a scheduled task to run the script once each day, in the middle of the night.

## Installing new software on Hyper-V VMs
Prior to this project, the only virtualization software I had used was Oracle VirtualBox, but now I’m using Hyper-V. Turns out Hyper-V transferring files to the VM is a real pain in Hyper-V, and using a USB drive is even worse! And because this is an air-gapped network, I didn’t want to bridge that gap and connect to the Internet. So, I would download necessary files on an Internet-connected machine, load them onto a thumb drive, connect it to the physical server, and drop them on its desktop. Then I used RDP to copy the files over to the VM.

That was too cumbersome, so I set up an FTP server on the VM. After downloading the files that I needed to the Internet-connected machine, I would disconnect it from the Internet-facing router and connect it to the air-gapped network and use FTP to send the files. Not very efficient, but it worked and it kept my network off the Internet.

# Lessons learned while building this network:

* If you aren’t getting traffic to a device you expected to get traffic to, check the following:
    * Are the Ethernet cables connected?
        * Are they really? Make sure they didn’t come partially unplugged due to broken clips!
        * Are they connected to the right devices? Check the topology!
        * Are they connected to the right physical ports? Check the topology!
    * Are the routers/switches plugged in and turned on?
    * Are the PCs routed with the correct IP address? Check the topology!