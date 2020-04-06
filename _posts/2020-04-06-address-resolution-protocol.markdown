---
layout: post
title:  "Understanding Address Resolution Protocol"
date:   2020-04-06 12:43:11 +0500
categories: network
---
Hello!

I've been taking the [CompTIA Networks+](https://www.cbtnuggets.com/it-training/comptia-network-plus-007) course by [CBT Nuggets](https://www.cbtnuggets.com/) and recently viewed the *Common Protocols* lecture where I learned about **Address Resolution Protocol** (ARP). In this post, I'll be discussing ARP in-depth.

## What is ARP?

Address Resolution Protocol is used for the translation of IP addresses (layer 3) to data link layer addresses (layer 2) of a host in both, the *TCP/IP* and *Open Systems Interconnection* internet protocols models. The most-common type of link layer address is the *Media Access Control* (MAC) address. All network interface devices such as WiFi adapters and Ethernet cards have a MAC address burnt into them.

![OSI vs TCPIP](https://i.imgur.com/pHQNmM1.png)

[Image credits: guru99.com]

## Why is it needed?

**Example time!** Lets say there are two hosts, *Jon* and *Ygritte*, on the same network. Jon wants to send an **HTTP** packet to Ygritte. From our previous knowledge about the TCP/IP model, we know that Jon requires HTTP, TCP, IP, data-link and physical layer headers while constructing the packet. But how does Jon know what MAC address to write in the data-link header?

Although the network layer of both OSI and TCP/IP models provides global delivery of packets, actual communication of data packets between any two **directly-connected** hosts and/or routers takes place at the data link layer. For this communication to happen, routers and hosts need a mechanism to identify and address each other at the physical level, this is where ARP and MAC addresses come in.

## How does ARP work?

#### The basic protocol

Carrying on from the same example of Jon trying to send Ygritte an HTTP packet on the same network, here is how Jon will use ARP to find out Ygritte's MAC address:

1. Jon checks his **ARP cache** to see if he has Ygritte's MAC address in-memory. (Jon *knows nothing*, he obviously finds his cache empty.)
2. Jon **broadcasts** an **ARP Request** packet to all the devices on the network, specifying the following:
    * His own MAC address as source.
    * `ff:ff:ff:ff:ff:ff` as the destination MAC address, since Jon does not know Ygritte's MAC yet. (The `ff:ff:ff:ff:ff:ff` address signifies a broadcast message.)
    * Ygritte's IP address as destination.
3. Each device on the network sees Jon's broadcast, compares the destination IP address in the packet to their own. If the IPs do not match, the device ignores Jon's broadcast. (*sad*)
4. When Ygritte's sees the broadcast and finds that the destination IP address in the packet matches her own, she sends Jon an **ARP Response** packet specifying her MAC address. (*YAY! <3*)
5. Jon receives the reply and adds Ygritte's MAC address to his ARP cache. (*Jon finally knows something!*)

![ARP](https://i.imgur.com/y3a41Hw.png)

[Image credits: tcpip.com]

#### ARP Announcements

Whenever a host's IP or MAC address changes, the host will broadcast an **ARP Announcement** packet containing the updated IP against the host's MAC address to ensure there are no stale entries in the ARP caches of the networked devices.

## ARP security concerns

The Address Resolution Protocol is fairly simple and provides no means of authenticating whether the entries in ARP Request and Response packets are legitimate, this is obviously a major security concern! An attacker can easily send out fake ARP announcements with their own MAC address and a victim's IP address and thereby redirect all traffic that was destined for the victim, to the attacker. This is called **ARP posioning** or **ARP spoofing**.

![Jon Snow](https://i.imgur.com/xyDW7eN.gif)

[Image credits: knowyourmeme.com]
