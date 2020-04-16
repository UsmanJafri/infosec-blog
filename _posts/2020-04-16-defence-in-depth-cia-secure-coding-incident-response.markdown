---
layout: post
title:  "Defense-in-Depth, CIA, Secure-coding and The Six-Step Process for Incident Response!"
date:   2020-04-16 21:20:00 +0500
categories: network
---
Hello!

I've been taking the [SEC401: Security Essentials](https://www.sans.org/course/security-essentials-bootcamp-style) course by [SANS](https://www.sans.org) and learned a few useful things about cyber security and defense that I would like to share here!

## What is *Defense-in-Depth*?

Defense-in-Depth, commonly abbreviated to *DiD*, is a cyber security design principle in which multiple defense mechanisms are layered in successions to protect information systems from cyber attacks. In the DiD design principle, there is no single all-in-one security system, rather a series of defensive measures that work in unison to handle cyber attacks. When one layer falls, the layers behind it still stand!

#### Defense-in-Depth in medieval castles!

The Defense-in-Depth design principle is also commonly known as the **medieval castle approach**. This is because castles in medieval times were also protected by layers of defenses. The only feasible way to get through these defenses without heavy losses was to lay siege, until the residents run out of food and supplies. Some common layered defenses that medieval castles possesed include:

1. Castles were built on the highest point of a geographical region, such a hills, cliffs or the highest point of valleys. This not only gave the castle-dwellers the ability to spot distant armies but also made attacks difficult.

2. Castles were usually surrounded by deep ditches or moats with a single point of entry, a drawbridge.

3. Even if an attacking army managed to cross the moat or lower the drawbridge, they would be rained with arrows!

4. Heavy-iron gate! If the attackers some-how made it to the entrance of the castle, they would still have to figure out how to lift the main gate. The gate was usually operated by a pulley system that is only accessible from the insides of the castle.

5. And many more.....

![Castle](https://i.imgur.com/HbaKm2X.jpg)

[Image credits: Dreamstine.com]

#### Defense-in-Depth in modern systems!

Here are some possible layers for a Defense-in-Depth model for security of modern information systems:

1. **Virtual Private Network (VPN)** - an organization's information resources should exist inside a virtual network and therefore will only be accessible to users who are members of the VPN.

2. **Firewall** - to filter and block traffic from unauthorized sources.

3. **Intrusion Detection System (IDS)** - to detect, identify and log attack attempts. Logged information from Intrusion Detection Systems can be very useful in identifying sources of attacks, what information the attackers were after and what part of the DiD system the attackers targetted and hence need reinforcement.

3. **Anti-virus and anti-malware tools** - pretty self-explanatory!

4. **Access-control** - each user should only have access to the set of information resources that the user needs, not more! Hence in the event of a user's account getting compromised, the loss of information is minimum.

5. **Logging** - LOG EVERYTHING! Maintenance of detailed logs can be very useful for identifying security breaches and also identifying what information was stolen.

![VPN](https://i.imgur.com/hCO5TaC.png)

[Image credits: Medium.com]

## What is CIA?

#### No, not ***that*** CIA!

#### Confidentiality, Integrity and Availability

The key goal of any information security system is to protect the CIA: Confidentiality, Integrity and Availability of the application or orgranization in question.

Whenever a new application is designed or an organization's resources system is created, it is important to analyze and weigh the importance of each of the three members of CIA. A risk-analysis should be performed to see which of the three are most at risk and need to be addressed.

For example, for a banking system, ideally all three should be ensured but of the three, Integrity and Confidentiality are **much more** important than Availability. A bank would not want any of its records to be tampered with (hence the importance of Integrity) or have financial customer records leaked (hence the importance of Confidentiality) even if it is at the cost of the banking system being down (hence Availability is not very important).

![CIA](https://i.imgur.com/xTfJYrA.png)

[Image credits: Medium.com]

## Secure Coding 101

One of the best ways to defend information systems and applications from cyber attacks is to design and implement them in such a way that there are no security vulnerabilities. 

Although, it is *impossible* to know for sure whether a snippet of code is vulnerable, it is important to strive towards writing secure code by following a few general guidelines, which are valid for all programming languages:

1. **Variable Initilization:** *Always initialize variables to default values!* Some languages may automatically initialize variables for you, however this does not protect your code from logical vulnerabilities!

2. **Input Sanitization:** *Always sanitize user input to allow acceptable characters only! Everything else should be rejected and filtered out.*

3. **Error Handling:** *Handle all errors to ensure your code always exits gracefully!*

4. **Libraries > rewriting code:** If a popular library with many users is available for something that you plan on implementing, ***use the library!*** A popular library is more-likely to be well-tested and patched for known vulnerabilities.

5. **Updates:** always update the code compiler/interpreter and any external libraries that you may be using to ensure you receive the latest security vulnerability patches.

6. **Privilige Management:** for server-side code that interacts directly with users, make sure the code requires the lowest-level of privilige possible to execute. `root` or `sudo` priviliges should never be used, ideally a separate user account should be created for the server-side code with **ONLY bare-minimum priviliges** granted.

7. **Obfuscation != Security:** never hard-code account credentials such as username and passwords or other sensitive information in code even if you think it is "well-hidden" using obfuscatio or other techniques. Hiding something does not mean it will not eventually get discovered!

8. **Testing:** carry out extensive code review and testing for vulnerabilities!

![Testing](https://i.imgur.com/5S7AQjo.png)

[Image credits: Scnsoft.com]

## The Six-Step Process for Incident Response

Having a well-defined protocol in-place for responding to cyber security incidents is crucial to ensure a timely-response with minimum damage.

Here are the six steps for a responding to a cyber security incident:

1. **Preparation**

    Preparation involves contructing a set of rules, policies and guidelines for responding to various, possible security threats **before** an actual incident occours.
    
    For example, in-case of a physical data-centre breach, which security agenices need to be informed, how can stolen information or physical equipment be tracked, how can it be recovered? All these questions and their answers are pondered-upon before an actual incident.

2. **Identification**

    Identification involves analysing possible signals of an intrusion and deciding what action needs to be taken.

    Identification can be done by studying Intrusion Detection System (IDS) logs, analysing alarms raised by firewalls, unexpected system crashes or reboots, degraded system performance and much more.

    At this stage, a primary incident handler is assigned to manage an incident response team, witnesses and evidence are gathered to determine whether the event under investigation is an incident and needs further follow-up.

3. **Containment**

    Containment involves isolating the source of the incident and containing any damage that it may have caused.

    The first step of containment is to secure everything. A backup of all affected systems should be made to ensure all forensic evidence is intact. Next, evidence needs to be gathered, analysed and documented. All accounts that were affected should be locked for analysis and then have their passwords reset to ensure the attackers cannot re-enter.

4. **Eradication**

    Eradication involves patching the vulnerabilities that exploited in the incident. If it was a virus or worm attack, affected systems should be scanned and wiped to ensure no traces of the malware are left.

    Even after the problem is addressed a vulnerability analysis should be performed again to ensure the attacker did not open and leave any other backdoors open.

5. **Recovery**

    Recovery involves restoring the system back to its operational state.

    For example, if the system was wiped to clean out any viruses, the system is restored using safe external backups. It is imperative to ensure that the backups being used are secure and do not possess the vulnerablities or malware that was utilized in the original incident.

6. **Lessons Learned**

    This stage involves documenting everything that occoured in the incident to identify areas-of-improvements and conclusions that could help the organization safeguard themselves from future attacks.

![IR](https://i.imgur.com/h6jqdiI.jpg)

[Image credits: Infocyte.com]