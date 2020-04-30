---
layout: post
title:  "More AWS: Elastic Load Balancing & Setting up the AWS Command Line"
date:   2020-04-30 17:15:00 +0500
categories: aws
---
Hello!

As you may have noticed from my previous posts, I have spent the better part of this week playing around with everything **Amazon Web Services (AWS)** has to offer. In today's blog, I will be sharing a few more demos that I felt are interesting and may be useful to share with others. Let's get started!

## Creating an Internet-facing Elastic Load Balancer (ELB)

1. We created an EC2 instance to host an Apache-based web server in the last blog. Head over [there](https://usmanjafri.github.io/infosec-blog/aws/2020/04/29/aws-ec2.html) again, and make a new EC2 instance and set up an Apache server.

    *Why do we need 2 instances, you ask?*

    Having a load balancer that distributes load across a single instance is sort of pointless, ofcourse.

2. From the **AWS Services** page navigate to **EC2** under the **Compute** category.

    ![](https://imgur.com/otJPvz8.jpg)

3. On the **EC2 Dashboard** page, click **Load balancers**.

    ![](https://imgur.com/yIsCCXA.jpg)

4. On the **Load Balancers** page, click **Create Load Balancer**.

    ![](https://imgur.com/zgHALXl.jpg)

5. You will now have three Elastic Load Balancer types to choose from:

    ![](https://imgur.com/jrgHYSs.jpg)

    i. **Application Load Balancer:** are also known as *layer 7 load balancers*. These handle HTTP and HTTPS traffic and offer rich HTTP feature support (such as utilizing the HTTP *X-Forwarded-For* header) as compared to *Network* and *Classic* load balancers. Application load balancers have higher latency than Network load balancers but are much more intelligent.

    ii. **Network Load Balancer:** are also known as *layer 4 load balancers*. These support applications that utilize TLS, TCP and UDP transport protocols. Network load balancers are genrerally used in latency-critical applications.

    iii. **Classic Load Balancer:** are a mix of both *Application* and *Network* load balancers because they offer support for both layer 7 load balancing for HTTP/HTTPS and layer 4 load balancing for TCP-based applications.

    I will be using Classic Load Balancer for the purpose of this demonstration. Click **Create** in the **Classic Load Balancer** tile.

6. On the **Define Load Balancer** page, do the following:

    ![](https://imgur.com/MggDX5J.jpg)

    i. Enter a **Load Balancer name**.

    ii. Under **Create LB Inside**, Select the Virtual Private Cloud containing your EC2 instances.

    iii. Uncheck **Create an internal load balancer** since we want to create an internet-facing load balancer.

    iv. Under **Listener Configuration**, add two **Load Balancer Protocols**: `HTTP` and `HTTPS`.

    v. Finally, press **Next: Assign Security Groups**.

7. On the **Assign Security Groups** page, select the Security Group you created while creating your EC2 instances and then click **Next: Configure Security Settings**.

    If you do not know what this, please see **Step 8** of *Creating an EC2 Instance* demo in my last blog post [here](https://usmanjafri.github.io/infosec-blog/aws/2020/04/29/aws-ec2.html).

    ![](https://imgur.com/4Mykw9I.jpg)

8. SSH into your all of your EC2 instances and do the following:

    i. If you do not know how to SSH, please see the *Connecting to an EC2 instance via SSH* section of my previous blog post [here](https://usmanjafri.github.io/infosec-blog/aws/2020/04/29/aws-ec2.html).

    ii. Navigate to `/var/www/html`.

    ```
    cd /var/www/html
    ```

    iii. Create a file named `lb-ping.html`.

    ```
    sudo touch lb-ping.html
    ```

    This html file will be periodically retrieved by the load balancer to ensure your server instance is healthy and running!

    ![](https://imgur.com/BiVkkjJ.jpg)

9. On the **Configure Health Checks** page, do the following:

    i. Set **Ping Protocol** to `HTTP`.

    ii. Set **Ping Port** to `80`.

    iii. Set **Ping Path** to `lb-ping.html`. We made this file in the previous step.

    iv. Click **Next: Add EC2 Instances**.

    ![](https://imgur.com/uikMRnk.jpg)

10. Now, on the **Add EC2 Instances** page, do the following:

    i. Add the two EC2 instances you created earlier.

    ii. Enable **Connection draining**. This provides fault tolerance when an EC2 instance goes down for updates or maintenance. The load balancer will prevent a clients connection from breaking by automatically serving remaining HTTP resources from another EC2 instance.

    iii. Enable **Cross-Zone Load Balancing** if you created instances that are in different Availablility Zones. By default, a Classic Load Balancer will only distributed requests in its own Availability Zone only, to make use of EC2 instances in the same region but in different Availability Zones, Cross-Zone Load Balancing should be enabled.

    iv. Finally press **Next: Add Tags**.

11. On the **Add Tags** page, press **Create Tag** to create a new key-value tag. These are useful for management purposes. Click **Review and Create** when done.

12. Finally on the **Review** page, please review the load balancer's configuration carefully. Once everything is verified, click **Create** to create the load balancer instance.

## Setting up Amazon Command Line

1. From the **AWS Services** page navigate to **IAM** under the **Security, Identity, & Compliance** category.

    ![](https://imgur.com/4MFxYkO.jpg)

2. On the **IAM Dashboard**, select **Users**.

    ![](https://imgur.com/TO4019E.jpg)

3. Click **Add user**.

    ![](https://imgur.com/8rkBcx2.jpg)

4. On the **Add user** page, do the following:

    ![](https://imgur.com/o042h4W.jpg)

    i. Choose a **User name**.

    ii. Under **Access type** check Programmatic access since we want to use the AWS CLI. However, uncheck **AWS Management Console access** as this is not required.
    
    iii. Finally click **Next: Permissions**.

5. On the **Set permissions** page, add the newly-created user to a security group, if any. Otherwise, you can attach security policies to the account directly. I will be enabling the **AdministratorAccess** policy for this account.

    ![](https://imgur.com/FhauAaK.jpg)

6. On the **Add Tags** page, press **Create Tag** to create a new key-value tag. These are useful for management purposes. Click **Next: Review** when done.

    ![](https://imgur.com/yF40c79.jpg)

7. Finally on the **Review** page, please review the new user's configuration carefully. Once everything is verified, click **Create user**.

    ![](https://imgur.com/6WCH3fs.jpg)

8. Download the CSV file containing your Access Key ID and Secret key and store it a secure place.

    ![](https://imgur.com/D78835u.jpg)

9. Head-over to the [Amazon Command Line Interface homepage](https://aws.amazon.com/cli/) and download a binary suitable for your OS and architecture.

    I will be downloading the Windows 64-bit binary.

    ![](https://imgur.com/5C09Z8r.jpg)

10. Installing the AWS CLI binary should straight-forward, a couple of *Nexts* on Windows and you should be good to go.

    ![](https://imgur.com/XDACSDg.jpg)

11. To test whether the installation was successful, open a Command Prompt window and try the following:

    ```
    aws
    ```
    ```
    aws --version
    ```

    You should see the following output:

    ![](https://imgur.com/TACxcqO.jpg)

12. Next enter the following in the Command Prompt window:

    ```
    aws configure
    ```
    
    You will be prompted to enter the following:

    i. **AWS Secret Key ID** - you saved this in ***Step 8***.

    ii. **AWS Secret Access Key** - *Step 8 too!*

    iii. **Default region name** - From this [list](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html), choose the region closest to you.

    iv. **Default output format** - you may either: `json`, `yaml`, `text` or `table`.

    ![](https://imgur.com/D944qjR.jpg)

13. You should now have AWS CLI set up. To learn what you can with the AWS CLI, please see the [official documentation](https://docs.aws.amazon.com/cli/latest/reference/)!