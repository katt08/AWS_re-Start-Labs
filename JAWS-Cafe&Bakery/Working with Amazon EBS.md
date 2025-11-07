Working with Amazon EBS
Lab overview
Amazon Elastic Block Store (Amazon EBS) is a scalable, high-performance block-storage service that is designed for Amazon Elastic Compute Cloud (Amazon EC2). In this lab, you learn how to create an EBS volume and perform operations on it, such as attaching it to an instance, creating a file system, and taking a snapshot backup.

 

Schematic diagram showing an EC2 instance with an attached EBS volume and a snapshot created from the EBS volume 

 

Objectives
By the end of this lab, you will be able to do the following:

Create an EBS volume.

Attach and mount an EBS volume to an EC2 instance.

Create a snapshot of an EBS volume.

Create an EBS volume from a snapshot.

Duration
This lab requires approximately 45 minutes to complete.

 

Accessing the AWS Management Console
At the top of these instructions, choose  Start Lab to launch your lab. 

Tip: If you need more time to complete the lab, choose  Start Lab again to restart the timer for the environment.

The status of the lab resources is be displayed on the upper-left corner:

AWS  indicates that AWS lab resources are currently being created.

AWS  indicates that AWS lab resources are ready.

Wait for the lab to be ready before proceeding.

At the top of these instructions, choose AWS  to open the AWS Management Console on a new browser tab. The system automatically signs you in.

Tip: If a new browser tab does not open, a banner or icon at the top of your browser will indicate that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and choose Allow pop-ups.

Arrange the AWS Management Console so that it appears alongside these instructions. Ideally, you should be able to see both browser tabs at the same time to follow the lab steps.

 

Task 1: Creating a new EBS volume
In this task, you create and attach an EBS volume to a new EC2 instance.

On the AWS Management Console, in the Search bar, enter and choose EC2 to open the EC2 Management Console.

In the left navigation pane, choose Instances.

An EC2 instance named Lab has already been launched for your lab.

Note the Availability Zone for the Lab instance. It looks similar to the following: us-west-2a

Tip: You might have to scroll to the right to see the Availability Zone column.

In the left navigation pane, for Elastic Block Store, choose Volumes.

You see an existing (8 GiB) volume that the EC2 instance is using.

Choose Create volume, and configure the following options:

Volume type: Choose General Purpose SSD (gp2).

Size (GiB): Enter 1. 
Note: You might be restricted from creating large volumes.

Availability Zone: Choose the same Availability Zone as your EC2 instance (which is us-west-2a in this case).

In the Tags -optional section, choose Add tag, and configure the following options:

Key: Enter Name.

Value: Enter My Volume.

Choose Create volume. 

A new volume appears with the status of Creating in the Volume state column. This status soon changes to Available. You might need to choose Refresh  to see your new volume.

Tip: You might have to scroll to the right to see the Volume state column.

 

Task 2: Attaching the volume to an EC2 instance
You now attach your new volume to an EC2 instance.

Select My Volume.

From the Actions menu, choose Attach volume.

From the Instance dropdown list, choose the Lab instance.

For the Device name field select /dev/sdb. Commands that you run later in this lab include this device identifier. 

Choose Attach volume.

The Volume state of your new volume is now In-use.

 

Task 3: Connecting to the Lab EC2 instance
In this task, you use EC2 Instance Connect to connect to the Lab EC2 instance. 

On the AWS Management Console, in the Search bar, enter and choose EC2 to open the EC2 Management Console.

In the navigation pane, choose Instances.

From the list of instances, select the Lab instance.

Choose Connect.

On the EC2 Instance Connect tab, choose Connect.

This option opens a new browser tab with the EC2 Instance Connect terminal window.

Note: If you prefer to use an SSH client to connect to the EC2 instance, see the guidance to Connect to Your Linux Instance.

You use this terminal window to complete the tasks throughout the lab. If the terminal becomes unresponsive, refresh the browser or use the steps in this task to connect again.

 

Task 4: Creating and configuring the file system
In this task, you add the new volume to a Linux instance as an ext3 file system under the /mnt/data-store mount point.

To view the storage that is available on your instance, in the EC2 Instance Connect terminal, run the following command:

df -h
You should see output similar to the following:

devtmpfs        464M     0  464M   0% /dev
tmpfs           473M     0  473M   0% /dev/shm
tmpfs           473M  464K  472M   1% /run
tmpfs           473M     0  473M   0% /sys/fs/cgroup
/dev/nvme0n1p1  8.0G  1.7G  6.4G  21% /
tmpfs            95M     0   95M   0% /run/user/0
tmpfs            95M     0   95M   0% /run/user/1000
These results show the original 8 GB disk volume. Your new volume is not yet shown.

To create an ext3 file system on the new volume, run the following command:

sudo mkfs -t ext3 /dev/sdb
To create a directory to mount the new storage volume, run the following command:

sudo mkdir /mnt/data-store
To mount the new volume, run the following command:

sudo mount /dev/sdb /mnt/data-store
echo "/dev/sdb   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
The last line in this command ensures that the volume is mounted even after the instance is restarted.

To view the configuration file to see the setting on the last line, run the following command:

cat /etc/fstab
To view the available storage again, run the following command:

df -h
The output now contains an additional line similar to the following: /dev/nvme1n1

Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        464M     0  464M   0% /dev
tmpfs           473M     0  473M   0% /dev/shm
tmpfs           473M  464K  472M   1% /run
tmpfs           473M     0  473M   0% /sys/fs/cgroup
/dev/nvme0n1p1  8.0G  1.7G  6.4G  21% /
tmpfs            95M     0   95M   0% /run/user/0
tmpfs            95M     0   95M   0% /run/user/1000
/dev/nvme1n1    975M   60K  924M   1% /mnt/data-store
To create a file and add some text on the mounted volume, run the following command:

sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
To verify that the text has been written to your volume, run the following command:

cat /mnt/data-store/file.txt
   The output displays the text that this command copies to the file. 

 

Task 5: Creating an Amazon EBS snapshot
In this task, you create a snapshot of your EBS volume.

Amazon EBS snapshots are stored in Amazon Simple Storage Service (Amazon S3) for durability. New EBS volumes can be created out of snapshots for cloning or restoring backups. Amazon EBS snapshots can also be shared among Amazon Web Services (AWS) accounts or copied over AWS Regions.

On the EC2 Management Console, choose Volumes, and select My Volume.

From the Actions menu, choose Create snapshot.

In the Tags section, choose Add tag, and then configure the following options:

Key: Enter Name.

Value: Enter My Snapshot.

Choose Create snapshot.

In the left navigation pane, choose Snapshots.

The Snapshot status of your snapshot is Pending. After completion, the status changes to Completed. Only used storage blocks are copied to snapshots, so empty blocks do not use any snapshot storage space.

In your EC2 Instance Connect terminal window, to delete the file that you created on your volume, run the following command:

sudo rm /mnt/data-store/file.txt
Note: If terminal is unresponsive, refresh the browser or reconnect as needed.

To verify that the file has been deleted, run the following command:

ls /mnt/data-store/file.txt
The following message displays: ls: cannot access /mnt/data-store/file.txt: No such file or directory

Your file has been deleted.

 

Task 6: Restoring the Amazon EBS snapshot
If you need to retrieve data stored in a snapshot, you can restore the snapshot to a new EBS volume.

Task 6.1: Creating a volume by using the snapshot
On the EC2 Management Console, select My Snapshot.

From the Actions menu, choose Create volume from snapshot.

For Availability Zone, choose the same Availability Zone that you used earlier.

In the Tags - optional section, choose Add tag, and then configure the following options:

Key: Enter Name.

Value: Enter Restored Volume.

Choose Create volume.

To see your new volume, in the left navigation, choose Volumes.

The Volume status of your new volume is Available.

When restoring a snapshot to a new volume, you can also modify the configuration, such as changing the volume type, size, or Availability Zone.

Task 6.2: Attaching the restored volume to the EC2 instance
Select Restored Volume.

From the Actions menu, choose Attach volume.

From the Instance dropdown list, choose the Lab instance.

For the Device name field, choose /dev/sdc. You use this device identifier in a later task.

Choose Attach volume.

The Volume status of your volume is now In-use.

Task 6.3: Mounting the restored volume
To create a directory for mounting the new storage volume, in the EC2 Instance Connect terminal, run the following command:

sudo mkdir /mnt/data-store2
To mount the new volume, run the following command:

sudo mount /dev/sdc /mnt/data-store2
To verify that the volume that you mounted has the file that you created earlier, run the following command:

ls /mnt/data-store2/file.txt
You should see the file.txt file.

 

Conclusion
Congratulations! You now have successfully done the following:

Created an EBS volume

Attached and mounted an EBS volume to an EC2 instance

Created a snapshot of an EBS volume

Created an EBS volume from a snapshot

 

Lab complete
