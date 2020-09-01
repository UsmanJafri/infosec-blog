---
layout: post
title:  "Event IDs to watchout for in Windows Event Logs"
date:   2020-09-01 19:50:00 +0500
categories: evidence
---
Hello!

It has been a long, long time since my last blog post and to make this long break worthwhile, I have some very exciting to share in this post! **Today, we will be taking a look at some Event IDs to look out for in Windows Event Logs and the malicious activity these events represent!**

## What are Windows Event Logs

Microsoft Windows has a built-in suite of tools called the Windows Event Logs for logging various system related events including but not limited to Application logs, Security logs and System logs.

Windows includes a handy tool called the ***Event Viewer*** for viewing Windows logs. The Event Viewer was released in 1993 and since then is a standard pre-installed tool on all versions of Windows.

For logs other than Security, there are five event levels, listed in decreasing order of severity:

1. Critical *(Most severe)*
2. Error
3. Warning
4. Information
5. Verbose *(Least severe)*

For a security analyst the Security logs are of extreme importance followed closely by the System logs. In the next section we will be discussing a handful of Event IDs that a security personnel should watch out for.

Thanks to excellent Security log encyclopedia at Ultimate Windows Security!

1. **An attacker attempted a Port Scan:**

    A. Repeated instances of this with changed destination port
    - **Source:** Security
    - **Event ID:** 5156
    - **Description:** 	The Windows Filtering Platform has allowed a connection

    B. Repeated instances of this with changed destination port
    - **Source:** Security
    - **Event ID:** 5157
    - **Description:** The Windows Filtering Platform has blocked a connection

2. **An attacker trying to login:**

    A. Note that these events are generated for normal, non-malicious logons too. Hence, it is important to analyse these events with other contextual information such as logon outside normal work hours, successful logon after many failed attempts, logon from unusual/uncommon source IP address, etc.
    - **Source:** Security
    - **Event ID:** 4624
    - **Description:** An account was successfully logged on

3. **An attacker covering their tracks**

    A.
    - **Source:** Security
    - **Event ID:** 4719
    - **Description:** System audit policy was changed

    B.
    - **Source:** Security
    - **Event ID:** 1102
    - **Description:** The Security log file was cleared

    C.
    - **Source:** System
    - **Event ID:** 104
    - **Description:** The Application log file was cleared

    D.
    - **Source:** Security
    - **Event ID:** 4699
    - **Description:** A scheduled task was deleted

    E.
    - **Source:** Security
    - **Event ID:** 4701
    - **Description:** A scheduled task was disabled

4. **An attacker using scheduled tasks for Persistence and/or Execution**

    A.
    - **Source:** Security
    - **Event ID:** 4698
    - **Description:** A scheduled task was created

    B. 
    - **Source:** Security
    - **Event ID:** 4700
    - **Description:** A scheduled task was enabled

    C. 
    - **Source:** Security
    - **Event ID:** 4702
    - **Description:** A scheduled task was updated

5. **An attacker using a service for Persistence and/or Execution:**

    A.
    - **Source:** Security
    - **Event ID:** 4697
    - **Description:** A service was installed in the system

    B.
    - **Source:** System
    - **Event ID:** 7045
    - **Description:** A service was installed in the system

6. **An attacker opened a listener:**

    A. 
    - **Source:** Security
    - **Event ID:** 5154
    - **Description:** The Windows Filtering Platform has permitted an application or service to listen on a port for incoming connections

7. **An attacker editing a registry key (could be for various reasons including Persistence):**

    A.
    - **Source:** Security
    - **Event ID:** 4657
    - **Description:** A registry value was modified

8. **An attacker creating a new process:**

    A.
    - **Source:** Security
    - **Event ID:** 4688
    - **Description:** A process has been created

9. **An attacker creating a new user:**

    A.
    - **Source:** Security
    - **Event ID:** 4720
    - **Description:** A user account was created

    B.
    - **Source:** Security
    - **Event ID:** 4722
    - **Description:** A user account was enabled

    C. An attacker adding the newly created user to a local group
    - **Source:** Security
    - **Event ID:** 4735
    - **Description:** A security-enabled local group was changed

    D. *[Domain Controller only]* An attacker adding the newly created user to a group
    - **Source:** Security
    - **Event ID:** 4737
    - **Description:** A security-enabled global group was changed

10. **An attacker changing user account password:**

    A. 
    - **Source:** Security
    - **Event ID:** 4723
    - **Description:** An attempt was made to change an account's password

    B. 
    - **Source:** Security
    - **Event ID:** 4724
    - **Description:** An attempt was made to reset an accounts password

    C. 
    - **Source:** Security
    - **Event ID:** 4738
    - **Description:** A user account was changed

11. **An attacker trying to guess a user's password:**

    A. 
    - **Source:** Security
    - **Event ID:** 4740
    - **Description:** A user account was locked out

    B. Repeated occourence of this indicates a password guessing attempt
    - **Source:** Security
    - **Event ID:** 4625
    - **Description:** An account failed to log on

12. **An attacker deleting a user:**

    A. 
    - **Source:** Security
    - **Event ID:** 4725
    - **Description:** A user account was disabled

    B. 
    - **Source:** Security
    - **Event ID:** 4726
    - **Description:** A user account was deleted

13. **An attacker trying to read a file:**

    A. 
    - **Source:** Security
    - **Event ID:** 4663
    - **Description:** An attempt was made to access an object

    B. 
    - **Source:** Security
    - **Event ID:** 4656
    - **Description:** A handle to an object was requested

14. **A attacker gained special privileges:**

    A. 
    - **Source:** Security
    - **Event ID:** 4673
    - **Description:** A privileged service was called

    B. 
    - **Source:** Security
    - **Event ID:** 4672
    - **Description:** Special privileges assigned to new logon

15. **Intel Gathering**

    A.
    - **Source:** System
    - **Event ID:** 6009
    - **Description:** The operating system version, build number, service pack level was enumerated.