---
layout: post
title:  "Learning Linux!"
date:   2020-04-13 19:40:00 +0500
categories: linux
---
Hello!

I've been taking the [LinuxCBT Ubu12x Edition](https://www.linuxcbt.com/products_linuxcbt_ubu12x_edition) course and learned a few useful tricks that I would like to share here!

## Generating SSH Keys

1. Open a Terminal.
2. Run the following command, replacing `johnybravo@email.com` with your email address.

    >ssh-keygen -t rsa -b 4096 -C "johnybravo@email.com"

    This command will generate a public-private key pair using **RSA encryption** with a key size of **4096 bits**.


3. You will be asked to enter a location to save the key, press **Enter** to select the default location:
    
    >Enter file in which to save the key (/home/user/.ssh/id_rsa):

4. You will now be asked to enter a passphrase to secure your SSH key. This is useful if you plan to add your key to a SSH user-agent:

    >Enter passphrase (empty for no passphrase)

    >Enter same passphrase again:

    Using an SSH user agent, you will only have to authenticate using the passphrase once per-session. If you are using a personal computer, you can choose an empty passphrase for a password-less SSH experience. However this is not recommended.

5. Run the following command to check whether the keys were generated.

    >ls ~/.ssh

    You should see two files:

    - `id_rsa` : the private key
    - `id_rsa.pub` : the public key



6. *[OPTIONAL]* If you are on a Linux or Mac system, you can add your SSH key to the default `ssh-agent` using the following command:
    >ssh-add ~/.ssh/id_rsa

7. *[OPTIONAL]* To add your public key to a SSH-enabled server, run the following command replacing `user` with the remote username and `IP` with remote server's IP address:
    
    >ssh-copy-id -i ~/.ssh/id_rsa user@IP

![SSH](https://imgur.com/LnETYmm.png)

## All the stats!

1. Run the following command for detailed hardware information information including vendor name, chassis type, motherboard specification, CPU specification, RAM specification, all connected USB devices, display specification, all connected PCI devices and much more:
    
    >lshw

    For a more elaborate listing of hardware specification it is recommended to run it with `sudo` priviliges:
    
    >sudo lshw

2. Run the following command for information on all network interface cards and their layer 3 addresses:
    
    >ip address show

3. Run the following command for information on all network interface cards and their layer 2 addresses:
    
    >ip link show

4. Run the following command for detailed CPU information including number of cores, number of threads, cache sizes, minimum frequency, maximum frequency and much more:
    
    >lscpu

![CPU](https://imgur.com/c2dOAC7.png)

5. Run the following command to final all PCI devices and their bus IDs:
    
    >lspci

6. Run the following command to final all USB devices and their bus IDs:
    
    >lsusb

![USB](https://i.imgur.com/zQy7zfe.png)

## May  I have your PERMISSION please!

1. Run the following command to take ownership of a file, replacing `johnybravo` with the new owner's username and `file.txt` with the file whose owner is to be updated:
    
    >chown johnybravo file.txt

2. Run the following command to take ownership of a file, replacing `admingroup` with the new owner's groupname and `file.txt` with the file whose owner is to be updated:
    
    >chown :admingroup file.txt

3. Run the following command to update both the owner and group-owner of a file, replacing `admingroup` with the new owner's groupname, `johnybravo` with the new owner's username and `file.txt` with the file whose owner is to be updated:
    
    >chown johnybravo:admingroup file.txt

4. Run the following command to change file permissions, replacing `file.txt` with the file whose permissions are to be updated:
    
    >chmod 777 file.txt

    Note that the first digit represents the permissions for the user owner, the second digit represents the permissions for the group owner and the third digit represents the permissions for the world.

    Each digit can be calculated as the sum of the following permissions:

    | Permission | Value |
    |---|---|
    | No permission | 0 |
    | Execute | 1 |
    | Write | 2 |
    | Read | 4 |