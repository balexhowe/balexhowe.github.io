---
layout: page
title: 
---

## [1 Introduction](#1-introduction)

NetApp Data Availability Services (NDAS) provides simplified orchestration of data management workflows for the hybrid cloud, allowing you to rapidly transform your secondary data into value-generating assets. Deployed as a cloud-based app, NDAS creates and manages data protection workflows from ONTAP source and target storage systems (that correspond to the SnapMirror primary and secondary systems), and replicates copies to the cloud using an intelligent format.

NDAS takes advantage of existing ONTAP SnapMirror technology, and marries it with a new patent-pending hybrid cloud data transfer engine to convert on-premises data into an intelligent cloud-resident data format. This Copy-to-Cloud technology then transfers storage-efficient NetApp Snapshot copies in a secure manner to an S3 cloud object store using S3 protocol. The orchestrator app tracks protected data in a cloud-native catalog, which also provides search capabilities; you can locate granular data objects and restore them on-premises to the original location, or to an alternate location.

Once on-premises ONTAP clusters are updated to meet NDAS prerequisites, the NDAS app provides a simplified UI to walk the IT generalist user through a series of steps to configure a hybrid cloud data protection environment. The interface is intuitive to users with more general IT skills, and provides the ability to apply existing data protection policies with confidence. Users do not require extensive data protection or storage backgrounds!

## [2 Lab Environment](#2-lab-environment)

![alt text]({{ site.baseurl }}/assets/images/en_SL10547.png "Lab Environment")
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