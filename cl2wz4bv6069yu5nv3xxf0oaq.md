# Replacing EFS with Amazon FSx for NetApp ONTAP to reduce storage costs

### First let's cover the basics, then we will compare the costs

### Amazon EFS
Amazon Elastic File System is a file system that you can mount from many instances. It automatically grows and shrinks, as you add and remove files with no need for management or provisioning. 

That means you do not have to specify the size while creating EFS and you pay as you use. 

For simplicity, you can consider EFS as a network shared drive, that a lot of instances can mount and use to read and write files.

And all the connected instances get the changes instantly.


## Amazon FSx for NetApp ONTAP

### What is ONTAP
ONTAP is NetApp's proprietary operating system used in storage disk arrays[1]

### Who is Netapp
NetApp is a hybrid cloud data services and data management company working in the storage domain for the last 30 years.

### What is FSx
FSx is a high-performance file system in the cloud, there are four widely-used file systems: **NetApp ONTAP**, **OpenZFS**, **Windows File Server**, and **Lustre**.

## Let's compare the two services FSx NetApp ONTAP and EFS


You can mount both these filesystems from EC2, ECS and EKS

FSx ONTAP           | | Amazon EFS
:-------------------------:|:-------------------------:
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651117204638/LZHYJVeSN.png) | | ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651117292533/VgKQHIm0F.png)

Both filesystems can be mounted using Network File System versions 4.0 and 4.1 (NFSv4)

Both the filesystem can be mounted with almost the same commands 

Both services offer a lot of features to replicate data from one region to another. You can check the service pages for more details.

```
sudo mount -t nfs svm-111.fs-11111.fsx.ap-south-1.amazonaws.com:/vol2 /fsx
```
```
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 172.11.2.206:/ efs
```
### Now lets compare the cost

One of the main benefit of using FSx NetApp ONTAP is you get Compression and deduplication, and according to the documentation you get around 65% savings for general-purpose workloads 

| Type      | EFS |  FSx ONTAP     |
| :---        |    :----:   |          ---: |
| Storage Cost(Multi AZ)      | $0.30/GB/Month       | $0.250 per GB-month   |
| Throughput Cost(Multi AZ)   | $6 per MBps-month        | $1.2 per MBps-month       |

*Let's say we have a 1 TB filesystem and we need 256 MB/s throughput, let's calculate the costs.
*

|       | EFS |  FSx ONTAP     |
| :---        |    :----:   |          ---: |
| Total Storage Charge (monthly)     | 307.20 USD     | 256.00 USD   |
| Total Throughput, IOPS, and Requests Charge (monthly)   | 1,228.80 USD | 307.20 USD      |


|       | EFS |  FSx ONTAP     |
| :---        |    :----:   |          ---: |
| Total monthly cost:    |  1,536.00 USD  |  563.20 USD  |

## Total saving of = 972.8 USD

Considering you are selecting Standard storage in EFS and Multi-AZ deployment with FSX.

This cost difference is without considering any compression & deduplication as that number is highly dependent on the data saved.

You can save more, with compression & deduplication enabled with Single AZ deployment with FSX


You can check more details here :

> https://aws.amazon.com/efs/pricing/	
> 
> https://aws.amazon.com/fsx/netapp-ontap/pricing/

[1]
A disk array is a disk storage system which contains multiple disk drives. It is differentiated from a disk enclosure, in that an array has cache memory and advanced functionality, like RAID, deduplication, encryption and virtualization.

> Let's wrap up this article, Please comment if you switched from EFS to FSx ONTAP after getting to know this huge difference in the cost.

My this year's target is two read 3 books and publish 2 blogs each month. I share my progress each month on Twiter. You can follow me, and learn something new with me.

%[https://twitter.com/nkalra0123/status/1488765699417784320]
