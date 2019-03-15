---
title: Azure Virtual Machine Inbound Security
comments: true
---
I wanna share my experience about playing with Azure Virtual Machine. Thanks to MSDN Subscription, i have a lot credits left to use.

Recently, I installed Jenkins and Docker on my Azure VM. No problem __but__ the problem is not that.  I am confused with azure new portal. Honestly, the Azure's new portal really __annoy me__.

At first, newly created virtual machine will only allow you connect through ssh (port 22). So if you trying to access other than that, it will be blocked by azure load balancer. CMIIW.

I need to dig Google to find how to configure inbound security on my azure virtual machine. And found the reference. 

1. First, you should choose your VM from dashboard.
2. Then click resource group link.
![first](http://i.imgur.com/1KcHhEV.jpg)
3. Click network security group (shield icon)
![first](http://i.imgur.com/zEHWrHs.jpg)
4. next you got the inbound security setting!
![second](http://i.imgur.com/VQYA8fg.jpg)

and the rest, you can configure, what port can be accessed, the source, the destination and more. 

Cheers!
