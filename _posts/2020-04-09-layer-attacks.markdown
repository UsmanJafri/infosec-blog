---
layout: post
title:  "Attacking the Network Stack"
date:   2020-04-06 20:30:00 +0500
categories: network
---
Hello!

In this post, I will be exploring various **network attacks** that can be carried out on each of the network stack's layers.

## Layer 1 (Physical)

#### Attacks

###### Wiretapping

Wiretapping involves attaching a physical probe/connection onto an electrical or optical cable with the intent of gaining access to all the bits that travel on the wire. Radio-based mediums can tapped too, using compatible radio-frequency antenna/receiver.

###### Service Disruption

Service Disruption involves causing direct damage to the physical media carrying network packets with the intent of causing a denial-of-service. Service disruption methods include: damaging cables, injecting bogus signals into the medium using a wiretap or by broadcasting on the same radio frequency.

#### Defences

###### Physical Defense

Physical networking devices should be placed in physically-secure environments.

###### Damage minimization

Implement protocols in higher layers that minimize loss even if data is stolen. For example: transfer of encrypted data packets.

## Layer 2 (Data Link)

#### Attacks

###### Passive Sniffing

On networks where all devices are on a single collision domain such as a hub or WiFi-based network, an attacker can place their network card in promiscuous mode and use a tool like as *tcpdump* to accept even those layer 2 packets that are not addressed to the attacker. Note that very modern networks use single-collisions domains so this method is not as effective as it once was.

###### ARP Spoofing

The Address Resolution Protocol (ARP) is used for the translation of layer 3 addresses to layer 2 addresses. Since ARP is fairly simple and provides no means of authenticating whether the entries in ARP Request and Response packets are legitimate, an attacker can easily send out fake ARP announcements with their own MAC address and a victim's IP address and thereby redirect all traffic that was destined for the victim, to the attacker.

###### ARP Flooding

Switches are not vulnerable to passive sniffing but are vulnerable to ARP Flooding. In this technique, an attacker will flood the network with ARP announcements with the hope of overflowing the switch's Content Addressable Memory (CAM) table. The CAM table stores mappings of assosciated MAC addresses to the switch's physical ports. Once the CAM table overflows, most switches go into a single-collision domain mode and hence broadcast packets to all nodes in the network to ensure traffic flow is uninterrupted, thereby opening the switch to passive sniffing.

###### WEP Cracking

The Wired Equivalent Privacy (WEP) is a security standard that uses RC4 encryption for securing WiFi-networks using a pre-shared key. WEP repeatedly sends a 24-bit information vector, ideally, the information vector should never repeat. However, the few number of bits means it will eventually repeat thereby making WEP vulnerable.

##### Wireless Jamming

An attacker can interfere with a target network's radio signal by spamming the same radio frequency and channel thereby causing a denial-of-service.

###### WPS Brute-force

A WiFi router with WiFi Protected Setup (WPS) enabled is susceptible to bruteforce attack. The WPS procedure involves the exchange of an 8 digit PIN in two parts of 4. Since the 8 digit PIN essentially behaves like two 4-digit PINs, there are only a total of `2 * (10^4) = 20000` combinations. The small number of combinations can be bruteforced easily.

#### Defences

###### WPS

1. Enable timeout when a host tries an incorrect WPS PIN.
2. Disable WPS altogether if the WiFi Access Point does not support timeouts.

###### WEP

Only use WPA2-Enterprise of WPA2-Personal security for wireless networks.

###### Switches versus Hubs

Use switches instead of hubs to prevent passive sniffing.

###### DAI and Rate-limiting of ARP packets

1. Dynamic ARP Inspection (DAI) enables real-time inspection of ARP packets to ensure their authenticity.
2. Rate limit hosts (ARP packets only) to prevent them from flooding the network with bogus ARP announcements.

## Layer 3 (Network)

#### Attacks

###### DDoS attack using IP Spoofing

Most Distributed Denial of Service attacks are carried out by spoofing the source IP address field in the IP headers of data datagrams. This allows attackors to overload a target system by making it receive replies from a third-party system which had received a datagram from the attacker with the source IP address of the victim.

###### DDoS attack using ICMP Flooding

A distributed denial of service attack can be carried out use the Internet Control Message Protocol (ICMP) echo messages to flood a target computer or network.

###### IP Route Spoofing

Since Routing Information Protocol (RIP) uses plaintext authentication messages, an attacker can spoof IP headers, get authenticated and distribute fake RIP route advertisements thereby preventing networks from correctly routing datagrams.

###### IP Fragmentation Attack

The IP layer is also responsible for fragmenting data datagrams such that it can fit the physical layer's Protocol Data Unit (PDU) size. Attackers can leverage this in various ways:

1. **Overlapping IP fragment:** two fragments of the same datagram having overlapping fragmentation offset can cause some operating systems to crash during datagram reconstruction.
2. **Intrusion Detection System (IDS) bypass:** by fragmenting IP datagrams to very small sizes, an attacker can bypass IDS detection since the framents will be too small to sufficiently match the attack vectors known by the IDS.

###### Network mapping and fingerprinting

The Internet Control Message Protocol (ICMP) can be used for tracing the path a datagram takes in a network by varying the Time-To-Live (TTL) field. Also, different specially-crafted IP headers cause OSes to respond differently, this can allow an attacker to discover the name and version of the operating system.

#### Defences

1. Disable ICMP Echo replies to prevent network mapping.
2. Source IP sanity checks: border routers should validate each outgoing IP datagram has the same network mask as that of the network. This will prevent outgoing IP spoofing attacks.
3. Use Open-Shortest-Path-First (OSPF) routing with MD5 authentication instead of Routing Information Protocol (RIP).
4. Use of IPSec for authentication and encryption of IP datagrams.

## Layer 4 (Transport)

#### Attacks

###### TCP SYN Flood

The attacker uses one or more hosts to repeatedly send lots of TCP Syn packets to a target server using a different TCP destination port. The source machine, in order to complete the TCP 3-way handshake, replies with a SYN-ACK packet to each of these requests. Since TCP is stateful, this not only consumes bandwith but also some memory on the target machine. TCP SYN flood attacks are commonly used in conjunction with IP Spoofing.

###### TCP RST Injection Attack

The attacker monitors (using previously discussed sniffing methods) the TCP packets on the target connection to guess the sequence number and windows size and then sends a forged packet with the TCP RST flag set to true. This results in the server terminating the TCP connection. This attack is commonly used for denying service or preventing someone from connecting to server when the attacker already has ways of snooping (to look at sequence numbers) into a network.

###### Port Scan

An attacker can scan both TCP and UDP ports to discover any services that might be running on the target server.

###### Defences