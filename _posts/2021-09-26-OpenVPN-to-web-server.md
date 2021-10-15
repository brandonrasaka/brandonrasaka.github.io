---
layout: post
title: "OpenVPN Connection to Private Web Server"
permalink: /:categories/:year/:month/:day/:title/
---

I have a personal project writing a work of fiction and I recently decided to update how I keep my notes. I used to just use Google Keep Notes because I could easily write down ideas on my phone. But it makes for terrible review of my notes when I'm actually writing the story and building the fictional world. I wanted something with better organization, but also something I could share with my writing group, and _only_ my writing group. I don't want to share it with the rest of the world until it is ready for publication.

So I decided to use the [jekyll-rtd-theme](https://github.com/rundocs/jekyll-rtd-theme) to create a documentation website, then host it on a local web server, then set up a VPN connection so my writing group can view it.

## The Website

### Template Modifications

### Hosting Locally

## The VPN

I followed the instructions on Digital Ocean for this and I don't think I can write better instructions than they. However, for the sake of demonstrating what I did and how my instance differed from their instructions, I will outline the main points of what I did.

### Certificate Authority

* Install Easy-RSA on dedicated machine

* Set up PKI

* Create CA

(_From_ [_Digital Ocean: How To Set Up and Configure a Certificate Authority (CA) On Ubuntu 20.04_](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04))

### OpenVPN

* Install Easy-RSA on machine to host the OpenVPN server

* Create PKI

* Request server certificate and private key from CA

* Sign request

* Set up TLS obfuscation

* Generate OpenVPN client certificate key pair

* Configure OpenVPN server

(_From_ [_Digital Ocean: How To Set Up and Configure an OpenVPN Server on Ubuntu 20.04_](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-ubuntu-20-04))
