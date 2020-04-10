---
layout: post
title:  "Attacking the Network Stack"
date:   2020-04-10 19:30:00 +0500
categories: network
---
Hello!

In this post, I will be exploring various network attacks that can be carried out on each of the *Open Systems Interconnection* (OSI) model's network layers.

![OSI](https://i.imgur.com/dBqoFni.png)
[Image Source: Medium.com]

## Layer 1 (Physical)

### Attacks

#### Wiretapping

1. Wiretapping involves attaching a physical probe/connection onto an electrical/optical cable with the intent of gaining access to all the bits that travel on the cable. Radio-based mediums can be tapped too using compatible radio-frequency antenna or receiver.

<img src="https://i.imgur.com/a6a1Dje.jpg" alt="Wiretap" width="200">

[Image Source: Amazon.com]

##### Service Disruption

Service Disruption involves causing direct damage to the physical media carrying network packets with the intent of causing a *denial-of-service*. Service disruption methods include: damaging cables, injecting bogus signals into the medium using a wiretap or by broadcasting on the same radio frequency.

### Defences

#### Physical Defense

Networking devices and equipment should be placed in physically-secure environments.

#### Damage minimization

Implement protocols in higher layers that prevent information from being easily readable even if it packets were stolen. For example: **encryption**.

## Layer 2 (Data Link)

### Attacks

#### Passive Sniffing

On networks where all devices are on a single collision domain such as a hub-based or a WiFi-based network, an attacker can place their network card in *promiscuous* mode and use a tool like as *tcpdump* to accept even those layer 2 **frames** that are not addressed to the attacker. It is important to mention that very few modern networks use single-collision domains so this method is not as effective as it perhaps once was.

![Sniffing](https://i.imgur.com/qjtNMst.png)

[Image Source: Lifewire.com]

#### Session Hijacking using ARP Spoofing

The *Address Resolution Protocol* (ARP) is used for the **translation of layer 3 addresses to layer 2 addresses**. Since ARP is fairly simple and provides no means of authenticating whether the entries in ARP Request and Response packets are legitimate, an attacker can easily send out fake ARP announcements with the attacker's own MAC address and a victim's IP address and thereby redirect all traffic that was destined for the victim, to the attacker.

#### ARP Flooding

Switches are not vulnerable to passive sniffing but are vulnerable to ARP Flooding. In this technique, an attacker will flood the network with ARP announcements with the hope of overflowing the switch's *Content Addressable Memory* (CAM) table. The CAM table stores mappings of assosciated MAC addresses to the switch's physical ports. Once the CAM table overflows, most switches go into a **single-collision** domain mode and hence broadcast packets to all nodes in the network to ensure traffic flow is uninterrupted, thereby making the switch vulnerable to sniffing.

#### WEP Cracking

The *Wired Equivalent Privacy* (WEP) is a security standard that uses **RC4 encryption** for securing WiFi-networks using a pre-shared key. WEP repeatedly sends a 24-bit information vector, ideally, the information vector should never repeat. However, the few number of bits means it will eventually repeat thereby making WEP vulnerable.

![WiFi Cracking](https://i.imgur.com/KT0bvic.jpg)

[Image Source: null-byte.wonderhowto.com]

#### Wireless Jamming

An attacker can interfere with a target network's radio signal by spamming the target network's radio frequency/channel thereby causing a *denial-of-service*.

#### WPS Brute-force

A WiFi router with *WiFi Protected Setup* (WPS) enabled is susceptible to brute force attack. The WPS procedure involves the exchange of an 8 digit PIN that is divided in two halves of 4 digits each. Since the 8 digit PIN essentially behaves like two 4-digit PINs, there are only a total of `2 * (10^4) = 20000` combinations. The small number of combinations can be brute forced easily.

### Defences

#### WPS

1. Timeout host if incorrect WPS pins are tried repeatedly.
2. Disable WPS altogether if the WiFi access point does not support timeouts.

#### WEP

Only use *WPA2-Enterprise*, *WPA2-Personal* or *802.1xEAP* security for wireless networks.

#### Switches versus Hubs

Use switches instead of hubs since each host connected to a switch is in its own collision domain and therefore safe from passive sniffing.

#### DAI and Rate-limiting of ARP packets

1. *Dynamic ARP Inspection* (DAI) enables real-time inspection of ARP packets to verify their authenticity.
2. Rate-limit hosts (ARP packets only) to prevent them from flooding the network with bogus ARP announcements.

## Layer 3 (Network)

### Attacks

#### DDoS attack using IP Spoofing

Most *Distributed Denial of Service* (DDoS) attacks are carried out by spoofing the **source IP address** field in the IP headers of datagrams. This allows attackors to overload a target system by making it receive replies from a third-party system which had received a datagram from the attacker with the source IP address of the victim.

![IP spoofing for DDoS](https://i.imgur.com/HdE1z7N.png)

[Image Source: Cloudflare.com]


#### DDoS attack using ICMP Flooding

A distributed denial of service attack can be carried out using the Internet Control Message Protocol (ICMP) Echo messages to flood a target computer or network.

#### IP Route Spoofing

Since *Routing Information Protocol* (RIP) uses plain-text authentication messages, an attacker can spoof IP headers, get authenticated and distribute fake RIP route advertisements thereby preventing networks from correctly routing datagrams.

This can also used for *Man-in-the-Middle* (MitM) attacks: the attacker will send out a false RIP route advertisement that diverts traffic through the attacker's network.

#### IP Fragmentation Attack

The IP layer is also responsible for fragmenting datagrams such that each datagram fits the data link layer's *Protocol Data Unit* (PDU) size. Attackers can leverage this in various ways:

1. **Overlapping IP fragment:** two fragments of the same datagram having overlapping *fragmentation offset* can cause some operating systems to crash during datagram reconstruction.
2. **Intrusion Detection System (IDS) bypass:** by fragmenting IP datagrams to very small sizes, an attacker can bypass IDS detection since the framents will be too small to sufficiently match the known attack vectors of the IDS.

#### MitM using ICMP Redirection

The *Internet Control Message Protocol* (ICMP) Redirect message is used by routers to inform a host if a better, lower cost path is available. An attacker can leverage this by generating fake ICMP Redirect messages and force a target host to route packets through a compromised router.

![MitM](https://i.imgur.com/8yNS35W.jpg)

[Image Source: Greycampus.com]

###### Network mapping and OS fingerprinting

The *Internet Control Message Protocol* (ICMP) can be used for tracing the path a datagram takes in a network by varying the *Time-To-Live* (TTL) field. Also, different specially-crafted IP headers cause operating systems to respond differently, this can allow an attacker to discover the name and version of a target's operating system.

#### Defences

1. Disable responses to ICMP Echo messages to prevent network mapping.
2. Source IP sanity checks: border routers should validate each outgoing IP datagram has the same network mask as that of the network. This will prevent outgoing IP spoofing attacks.
3. Use *Open-Shortest-Path-First* (OSPF) routing with MD5 authentication instead of *Routing Information Protocol* (RIP).
4. Use of *Internet Protocol Security* (IPSec) for authentication and encryption of IP datagrams.
5. Drop ICMP Redirect messages.

## Layer 4 (Transport)

### Attacks

#### TCP SYN Flood

The attacker uses one or more hosts to repeatedly send lots of TCP SYN segments to a target server using a **different TCP destination port** each time. The source machine, in order to complete the TCP 3-way handshake, replies with a SYN-ACK packet to each of these requests. Since TCP is stateful, this not only consumes bandwith but also some memory on the target machine. TCP SYN flood attacks are commonly used in conjunction with IP Spoofing.

#### TCP RST Injection Attack

The attacker monitors (using previously discussed sniffing methods) the TCP packets on the target connection to guess the sequence number and windows size and then sends a forged packet with the TCP **RST** flag set to true. This results in the server terminating the TCP connection. This attack is commonly used for denying service or preventing someone from connecting to server when the attacker already has ways of snooping (to look at sequence numbers) into a network.

#### Port Scan

An attacker can send both TCP and UDP segments to discover any services that might be running on the target server.

#### OS fingerprinting

Similar to operating system (OS) fingerprinting at the Network layer, an attacker can identify a target's OS name and version by sending specially-crafted TCP segments and then analysing the values set in the TCP header of the responses.

### Defences

### Snort Rules or IDS

Snort Rules are typically already implemented in most *Intrusion Detection Systems* (IDS), snort rules are a set of rules and patterns that are compared to each incoming packet. For example, to detect a TCP port scan, a snort rule could be as follows: detect TCP SYN packets sent to a consecutive sequence of ports followed immediately by TCP RST segments.

![Snort Rules](https://i.imgur.com/xocMnEn.png)

[Image Source: Blog.snort.com]

#### TLS/SSL

*Transport Layer Security* (TLS) and *Secure Socket Layer* (SSL) provide **cryptographic security** for communication between hosts. SSL was originally developed by a private company but was later adapted by the *Internet Engineering Task Force* for standardization resulting in the TLS.

TLS works as follows:
1. **Handshake:** A client informs the server about the hashing and cipher algorithms it supports. (e.g. *SHA-1*)
2. The server choose a cipher and hash function and informs the client.
3. The server sends a digital certificate containing the server's name, certificate issuing authority and the server's public key.
4. The client's browser validates the certificate by checking whether the issuing authority is trusted.
5. The client generates a session key by encrypting a random-number using the server's public key and sends it to the server.
6. The server uses their private key to decrypt the random-number.
7. The random number is used for encrypting all subsequent communication, a secure connection is now estabilished.

While the name *Transport Layer Security* suggests it exists at the Transport layer, it is generally thought to exist **in-between** the Transport and Application layers.

## Layer 5, 6 and 7 (Session, Presentation and Application)

### Attacks

#### DNS Cache Poisoning

In this attack, an attacker adds fake IP address entries against domain names in a compromised DNS server. Any user or other downstream DNS server that uses the poisoned server for DNS entries will be poisoned with the fake DNS records too. This allows an attacker to redirect user traffic to compromised servers. Since end-users continue to see the actual domain in their web browser, an attacker could phish passwords and other sensite information using well-crafted replicas of popular websites.

DNS cache poisioning is often carried out by governments, ISPs and organizations to restrict access to certain websites by redirecting users to a *"Site blocked"* web page.

#### Password Sniffing

Many Application layer protocols such as *File Transfer Protocol* (FTP), Telnet, *Internet Message Access Protocol* (IMAP) and *Post Office Protocol* (POP) exchange usernames and passwords in plain-text.

An attacker could gain access to a target user's credentials by sniffing the application's packet and analyzing the packet's username and password fields.

![Password sniffing](https://i.imgur.com/S13UQ5x.png)

[Image Source: Wpwhitesecurity.com]

#### Brute Force Attack

Many Application layer services are vulnerable to brute force attacks. Even if passwords are encrypted, an attacker can iterate through every possible password and try authenticating. Even though a brute force attack can take a very long time to complete, it is always guranteed to find the right password since theoretically the attacker will be iterating through the complete space of possible passwords.

#### Dictionary Attack

Application layer protocols that are vulnerable to brute force attacks are also vulnerable to dictionary attacks. In this attack, an attacker will try authenticating by iterating through a dictionary of commonly-used passwords. Although a dictionary attack is much faster than a brute force, it is not always guranteed to succeed.

#### SQL and Code Injection

May web applications pass user input into SQL queries without proper sanitization/escaping. This allows an attacker to inject SQL code into the web application, execute custom SQL queries crafted by the attacker and hence extract sensitive information from the application's database.

Similarly, user input may be passed into server code without proper sanitization/escaping thereby allowing an attacker to execute injected code. (Code injection)

#### XSS

*Cross-site scripting* (XSS) is a type of code injection in which an attacker targets users of a vulnerable website instead of the website itself.

There are mainly two types of XSS:

1. **Reflected XSS:** the attacker's mailicious code is injected via the request URL. The URL containing the mailicious string is then distributed using other methods such as public forums or email. This XSS type is called *reflected* because the mailicious code is being executed on a user's machine by being *reflected* of a vulnerable website.

2. **Stored XSS:** the attacker's mailicious code is stored on the server itself. The mailicious code is executed in a victim's browser when the victim interacts with the content containing the mailicious code. 

#### And countless more application-specific attacks........

#### Defences

1. Make use of *Domain Name System Security Extensions* (DNSSec) as it provides **cryptographically-secured authentication** of DNS data.
2. **Never store plain-text password!**
3. Implement **timeouts** when a fixed number of incorrect password attempts have been made. This is a good defence against both brute force and dictionary attacks!
4. To prevent SQL/code injection and XSS, **ALWAYS** escape user input!
5. Extensive code review and testing of applications for vulnerabilities!

![Code Review](https://i.imgur.com/QDF9mRw.png)

[Image Source: Scnsoft.com]