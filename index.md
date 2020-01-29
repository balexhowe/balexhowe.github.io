---
layout: page
title: Implementing Backup and Restore with NetApp Data Availability Services
---

## 1 Introduction 

NetApp Data Availability Services (NDAS) provides simplified orchestration of data management workflows for the hybrid cloud, allowing you to rapidly transform your secondary data into value-generating assets. Deployed as a cloud-based app, NDAS creates and manages data protection workflows from ONTAP source and target storage systems (that correspond to the SnapMirror primary and secondary systems), and replicates copies to the cloud using an intelligent format.

NDAS takes advantage of existing ONTAP SnapMirror technology, and marries it with a new patent-pending hybrid cloud data transfer engine to convert on-premises data into an intelligent cloud-resident data format. This Copy-to-Cloud technology then transfers storage-efficient NetApp Snapshot copies in a secure manner to an S3 cloud object store using S3 protocol. The orchestrator app tracks protected data in a cloud-native catalog, which also provides search capabilities; you can locate granular data objects and restore them on-premises to the original location, or to an alternate location.

Once on-premises ONTAP clusters are updated to meet NDAS prerequisites, the NDAS app provides a simplified UI to walk the IT generalist user through a series of steps to configure a hybrid cloud data protection environment. The interface is intuitive to users with more general IT skills, and provides the ability to apply existing data protection policies with confidence. Users do not require extensive data protection or storage backgrounds!
