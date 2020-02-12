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


## 3.2 Register Disk and Object Targets on NetApp Data Availability Services <a name="register-disk-and-object-targets-on-netapp-data-availability-services"></a>

The disk target is your NetApp ONTAP secondary cluster, and the cloud target is the Amazon S3 bucket that will contain the backups.

1. Click **Register Disk Target**.
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-1.png "Figure 3.2-1")
  _Figure 3.2-1:_**  
NDAS generates a single use configuration key that is used to securely register the ONTAP secondary cluster, and initiate discovery of the peered primary environment. 
2. Click **Copy to clipboard**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-2.png "Figure 3.2-2")
  _Figure 3.2-2:_**
3. In the "Register Disk Target" dialog click **OK**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-3.png "Figure 3.2-3")
  _Figure 3.2-3:_**  
4. Open a new tab on your browser.  
5. Navigate to the bookmarks folder called **NetApp**.  
6. Select **Oncommand System Manager - cluster2**.  
7. Login with the following credentials:   
- Login `admin` 
- Password `Netapp1!`  
8. Click **Sign In** . 
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-4.png "Figure 3.2-4")
  _Figure 3.2-4:_**  
9. Click **Configuration** on the left pane.
10. Select **Cloud registration**.
11. Paste the copied configuration key from NetApp Data Availability Service.
12. Click **Register**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-5.png "Figure 3.2-5")
  _Figure 3.2-5:_**  
13. The Cloud Registration page shows a green check mark, and a value of "Connected", indicating that cloud registration completed successfully.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-6.png "Figure 3.2-6")
  _Figure 3.2-6:_**  
14. Select the **NetApp Data Availability** browser tab.  
15. Select **Targets** on left pane.  
16. Click **Authenticate Disk Target**.
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-7.png "Figure 3.2-7")
  _Figure 3.2-7:_** 
17. Authenticate the secondary cluster (cluster2) using the following credentials:  
- Administrative username: `admin`  
- Password: `Netapp1!`
18. Click **Authenticate**.  
**Note:** When cluster authentication is completed successfully a message will appear at the top of the screen, NDAS will automatically discover the peered ONTAP primary cluster and its associated storage. This might take up to one minute.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-7.png "Figure 3.2-7")
  _Figure 3.2-7:_** 
19. Once Discovery is complete and you see the sections for "SVMs on Target Clusters" and "Peered Cluster Authentication", click **Authenticate Peered Clusters**.
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-8.png "Figure 3.2-8")
  _Figure 3.2-8:_**  
20. Select the **radio button** next to cluster1.  
21. Click **Authenticate**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-9.png "Figure 3.2-9")
  _Figure 3.2-9:_**  
22. Authenticate the primary cluster (cluster1) using the following credentials:  
- Administrative username: `admin`  
- Password: `Netapp1!`  
23. Click **Authenticate**.   
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-10.png "Figure 3.2-10")
  _Figure 3.2-10:_** 
24. Confirm cluster peering is authenticated with a status of "Healthy".  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-11.png "Figure 3.2-11")
  _Figure 3.2-11:_** 
25. Navigate to the **Dashboard**.  
**Note:** Now you can see that the ONTAP cluster discovery is complete. The "Protected to Disk" pane shows that 3 primary volumes are discovered. One of them already has an existing SnapMirror relationship. The Protection Environment is also partially filled out. And you can see Information messages in the Alerts pane about the NDAS policies created. The final configuration step is to Register your Cloud object store.  
26. Under "Protected to Cloud" click **Register Cloud Target**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-12.png "Figure 3.2-12")
  _Figure 3.2-12:_**
27. NDAS can write its backups to either an AWS S3 object store, or an on-premises NetApp StorageGRID object store.  
28. Select Use **AWS CLOUD**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-13.png "Figure 3.2-13")
  _Figure 3.2-13:_**  
29.  Minimize browser window.  
30. Open the README.txt on the desktop.  
31. Make note of the credential targets in which will be used for cloud target.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-14.png "Figure 3.2-14")
  _Figure 3.2-14:_**  
32. Maximize the browser to add the credentials for cloud target.  
33. Enter the following credential copied from the "Readme.txt" file saved on the desktop.  
34. Paste the following under Add Cloud Target:  
- Object Store Name: `myndas`    
- Access Key ID: `FKFKYCFNWPUCLANY7U7M`    
- Secret Access Key: `wMh5hxWIBZQt+avXxZ7y+p0/TFMWAH4YtcUR0A5G`  
35. Click **Add**.  
**Note:** Ignore the "Save Password" pop-up from browser.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-15.png "Figure 3.2-15")
  _Figure 3.2-15:_** 
36. Once the Cloud Target is created, a message will appear.  
37. Click back to **Targets**.  
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-16.png "Figure 3.2-16")
  _Figure 3.2-16:_**
38. In Targets you can see the object store is configured to NetApp Data Availability Services.    
![alt text]({{ site.baseurl }}/assets/images/Figure3.2-17.png "Figure 3.2-17")
  _Figure 3.2-17:_**  
  
  
  
  