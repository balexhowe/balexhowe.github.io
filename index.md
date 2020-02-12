---
layout: page
title: Implementing Backup and Restore with NetApp Data Availability Services
---

## [1 Introduction](#1-introduction)

NetApp Data Availability Services (NDAS) provides simplified orchestration of data management workflows for the hybrid cloud, allowing you to rapidly transform your secondary data into value-generating assets. Deployed as a cloud-based app, NDAS creates and manages data protection workflows from ONTAP source and target storage systems (that correspond to the SnapMirror primary and secondary systems), and replicates copies to the cloud using an intelligent format.

NDAS takes advantage of existing ONTAP SnapMirror technology, and marries it with a new patent-pending hybrid cloud data transfer engine to convert on-premises data into an intelligent cloud-resident data format. This Copy-to-Cloud technology then transfers storage-efficient NetApp Snapshot copies in a secure manner to an S3 cloud object store using S3 protocol. The orchestrator app tracks protected data in a cloud-native catalog, which also provides search capabilities; you can locate granular data objects and restore them on-premises to the original location, or to an alternate location.

Once on-premises ONTAP clusters are updated to meet NDAS prerequisites, the NDAS app provides a simplified UI to walk the IT generalist user through a series of steps to configure a hybrid cloud data protection environment. The interface is intuitive to users with more general IT skills, and provides the ability to apply existing data protection policies with confidence. Users do not require extensive data protection or storage backgrounds!

## [2 Lab Environment](#2-lab-environment)

![alt text]({{ site.baseurl }}/assets/images/Figure2-1.png "Lab Environment")

_Figure 2-1:_**

### Table of Systems

| Host Name        | Operating System           | Role/Function								 | IP Address		|
| ---------------- |----------------------------| -------------------------------------------| -----------------|
| jumphost   	   | Windows Server 2019		| primary desktop entry point for lab		 | 192.168.0.5		|
| dc1			   | Windows Server 2019        | Active Directory / DNS					 | 192.168.0.253	|
| cluster1		   | Ontap 9.5			        | cluster 1 management interface 			 | 192.168.0.101	|
| cluster2		   | Ontap 9.5			        | cluster 2 management			 			 | 192.168.0.102	|

### User IDs and Passwords

| Host Name | User ID			| Password	| Comments							|
| --------- |-------------------| ----------| ----------------------------------|
| jumphost	| DEMO\Administrator| Netapp1!	| Domain Administrator				|
| cluster1	| admin        		| Netapp1!	| Same for individual cluster nodes	|



## [3 Lab Activities](#3-lab-activities)

This lab contains the following activities and tasks:

* [Register NetApp Data Availability Services Account](#register-netapp-data-availability-services-account)
* [Register Disk and Object Targets on NetApp Data Availability Services](#register-disk-and-object-targets-on-netapp-data-availability-services)
* [Data Protection with NetApp Data Availability Services](#data-protection-with-netapp-data-availability-services)
* [Executing a NetApp Data Availability Services File Restore to Original Location](#executing-a-netapp-data-availability-services-file-restore-to-original-location)
* [Executing a NetApp Data Availability Services File Restore to Alternate Location](#executing-a-netapp-data-availability-services-file-restore-to-alternate-location)
* [Executing a NetApp Data Availability Services Volume Restore](#executing-a-netapp-data-availability-services-volume-restore)

## 3.1 Register NetApp Data Availability Services Account <a name="register-netapp-data-availability-services-account"></a>

At this point in the lab NDAS has already been deployed and is ready for login. This lab does not cover the NetApp Data Availability Services launch process.

In this initial lab activity you will register/create a new NDAS account. 

  1. Launch Chrome, which will open NetApp Availability Services as the browser's home page.
  
  **Note:** NDAS is also saved on the top left of the browser in folder called "NetApp".
  ![alt text]({{ site.baseurl }}/assets/images/Figure3.1-1.png "Figure 3.1-1")
  _Figure 3.1-1:_**  
  2. Click Sign Up.
  ![alt text]({{ site.baseurl }}/assets/images/Figure3.1-2.png "Figure 3.1-2")
  _Figure 3.1-2:_**
  3. Sign up with the following credentials:
     - Username **`admin`**
	 - Email **`root@demo.netapp.com`**
	 - New Password **`Netapp1!`**
	 - Re-enter Password **`Netapp1!`**
  4. Click **Sign In**.
  5. Once you click "Sign In", you will be automatically routed back to the login page.
  
  **Note:** Ignore the sync browser pop ups.
  ![alt text]({{ site.baseurl }}/assets/images/Figure3.1-3.png "Figure 3.1-3")
  _Figure 3.1-3:_**
  6. Login with the new account:
  - Login **`admin`**
  - Password **`Netapp1!`**
  7. Click **Sign In**.
    ![alt text]({{ site.baseurl }}/assets/images/Figure3.1-4.png "Figure 3.1-4")
    _Figure 3.1-4:_**
  8. This is the dashboard of NDAS. Since this is the first login, there is no configuration and no job activity is reported. In the Alerts pane, you can see messages about recent logins and any errors will also appear here. The Protection Environment pane is grayed out as the on-premises environment is not configured yet. The only option available is to "Register Disk Target".
      **Note:** Maximize the Chrome browser and use zoom (Ctl-) to see the whole dashboard.
  ![alt text]({{ site.baseurl }}/assets/images/Figure3.1-5.png "Figure 3.1-5")
  _Figure 3.1-5:_**


    