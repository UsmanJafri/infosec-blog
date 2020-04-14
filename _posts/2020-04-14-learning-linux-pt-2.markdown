---
layout: post
title:  "Learning Linux! (Part 2)"
date:   2020-04-14 21:15:00 +0500
categories: linux
---
Hello!

I've been taking the [LinuxCBT Ubu12x Edition](https://www.linuxcbt.com/products_linuxcbt_ubu12x_edition) course and learned a few useful tricks that I would like to share here!

Please note that the following content has been tested on **Ubuntu 18.04.4 LTS**.

## Make a HTTP Server!

1. Open a Terminal.

2. Run the following command to install the **Apache** web server.

    > sudo apt update && sudo apt install apache2

3. Run the following command to check whether the Apache service is running:

    > sudo systemctl status apache2

    ![sudo systemctl status apache2](https://i.imgur.com/3vqA80Q.png)

4. **[OPTIONAL]** The Apache server should have started automatically. If not, use the following command to manually start it:

    > sudo systemctl start apache2

5. Run the following command to check what port the Apache server is listening on:

    > sudo lsof -i | grep "apache2"

    By default, the Apache service listens on port 80.

    ![sudo lsof -i | grep "apache2"](https://i.imgur.com/T5mS59t.png)

6. Open a web-browser and visit `http://localhost:80`. You should see the *Apache2 Ubuntu Default Page*

    ![Apache2 Ubuntu Default Page](https://i.imgur.com/VuBw9p9.png)

7. **Read the *Configuration Overview* section very carefully!**

8. Replace the contents of `/var/www/html` with a website of your choice! For example, if we create a file `/var/www/html/index.html` with the content:


    ```
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
        <h1>
            Hello world!
        </h1>
    </html>
    ```

    The following site is served:

    ![Hello world!](https://i.imgur.com/KVG5KTG.png)

9. Finally to stop the Apache service run:

    > sudo systemctl stop apache2

10. It is very likely that the TCP port 80 is closed for access from the internet. In order to make your website accessible from the internet, you will have to forward TCP port 80 through your router's management interface. Since this process varies from router to router, instead of discussing the port forwarding process here, I would recommend you view the excellent guides at [PortForward](https://portforward.com/router.htm).
    
## Disk Partitions

1. Open a Terminal.
2. Run the following command, to view all partitions and connected drives.

    > sudo fdisk -l

    ![sudo fdisk -l](https://i.imgur.com/BVGyoyB.png)
    
    Note that, entries `/dev/sda1/`, `dev/sda2/` and `/dev/sda3/` are partitions of the same physical drive.

    Entries `/dev/sda1/` and `/dev/sdb1/` are different physical drives.

3. Run the following command to open the default partitioning tool bundled with Ubuntu.

    > sudo parted

    ![sudo parted](https://i.imgur.com/WIndody.png)
    
4. Run the following command to select a physical disk. Replace `/dev/sda` with the a physical drive's name you want to create a partition in. The name can be found from **Step 1**:

    > select /dev/sda

5. Run the following command to give the new partition a `msdos` type label:

    > mklabel msdos

    Other label types such as `bsd`, `gpt` and `mac` are also viable options, however `msdos` is the most universally supported.

6. Run the following command to display available space in the selected drive:

    > print free

    Please note the **Start** and **End** values of free space where you would like to create the parition. **This will be required later!**

    ![print free](https://i.imgur.com/2lSRa7a.png)

7. Run the following command to create a **Primary** parition. Note that you can create a `logical` partition if you do not intend to make the new partition bootable.

    > mkpart primary

8. You will now be prompted to enter a partition type. For Linux use, `ext4` is recommended. However, personally, I prefer `ntfs` because it allows access to the partition from both Linux and Windows-based systems in a multi-boot environment. `ext4` isn't supported by Windows, however `ntfs` is fully-supported by both Windows and Linux

    ![mkpart primary](https://i.imgur.com/1Rz5oVX.png)

9. Next you will be promted to enter a **Start** and **End** values for the partition. Enter the values you noted in **Step 6**. Press **Enter**, to finish creating the partition!

## Auto-mount a partition at startup

1. Open a Terminal.
2. Run the following command, to view all partitions and their UUIDs:

    > sudo blkid
    
    Note down the UUID of the partition you would like to automatically mount at startup.

3. Run the following command to find the current user's user ID:

    > id -u $USER

    Note down the printed user ID.

4. Run the following command to find the current user's group ID:

    > id -g $USER

    Note down the printed group ID.
    
5. Open the `/etc/fstab` in a code-editor of your choice. I will be using *Visual Studio Code*:

    > code /etc/fstab

6. Copy-paste the following line at the end of the file:

    > UUID=<YOUR_UUID> <YOUR_MOUNT_PATH> <PARTITION_TYPE> defaults 0 0

    - Replace `<YOUR_UUID>` with the UUID from **Step 2**.
    - Replace `<YOUR_MOUNT_PATH>` with the path at which you would like to mount your partition. I personally like to mount partitions in `/media/username/`.
    - Replace `<PARTITION_TYPE>` with the type of the partition.

7. If your partition type is `ntfs`, please replace `defaults` with the following string:

    > uid=<YOUR_UID>,gid=<YOUR_GID>,umask=0022,auto,rw

    - Replace `<YOUR_UID>` with the user ID from Step 3.
    - Replace `<YOUR_GID>` with the group ID from Step 4.

8. Save and close the file!
