# Creating-and-Configuring-Filesystems

# Tier 3 Task: Creating and Configuring Filesystems

Description:

The task is to create and configure filesystems on a CentOS system, ensuring optimal organization and management of data.

Step-by-Step Guide: Partitioning, Formatting, and Configuring Filesystems in CentOS

# 1. Identify the Available Storage Devices

First, you need to check which storage devices are available on the system. Use the lsblk command to list all block devices.

lsblk

The output will show all available disks and partitions. Look for the newly added disk. For example, you should see something like this:

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0   40G  0 disk 
├─sda1   8:1    0   500M  0 part /boot
└─sda2   8:2    0  39.5G 0 part /
sdb      8:16   0   20G  0 disk

In this example, /dev/sdb is the new storage device.

<img width="255" alt="lklk" src="https://github.com/user-attachments/assets/037eac55-d35a-4852-91fe-aa8248584e6c">


# 2. Select a Suitable Storage Device

Select /dev/sdb as the storage device for partitioning and formatting. Confirm that the disk has no existing partitions using:

sudo fdisk -l /dev/sdb

This will show the partition table of /dev/sdb. If it is empty, you are ready to proceed.

# 3. Use a Partitioning Tool to Create Partitions

Use fdisk to create a new partition on /dev/sdb.

Start the partitioning tool:

sudo fdisk /dev/sdb

Follow these steps within the fdisk prompt:

Press n to create a new partition.
Select p for a primary partition.
Choose partition number 1 (default).
Press Enter to accept the default first and last sectors (to use the whole disk).
Press w to write the changes and exit fdisk.

<img width="255" alt="lklk" src="https://github.com/user-attachments/assets/e1869250-0a43-474c-9d5e-aad0e7e25e8d">


Verify the new partition:

lsblk

You should now see /dev/sdb1 listed under /dev/sdb.

<img width="253" alt="mls" src="https://github.com/user-attachments/assets/885b4534-309c-4fea-adbe-cc1c1b5211fc">


# 4. Choose the Appropriate Filesystem Type

For this task, we will use the ext4 filesystem, as it is a widely used and reliable filesystem in Linux environments.

# 5. Format the Partition with ext4

To format the newly created partition /dev/sdb1 with the ext4 filesystem, use the following command:

sudo mkfs.ext4 /dev/sdb1

<img width="458" alt="sxsx" src="https://github.com/user-attachments/assets/d5eb5bfe-88fd-4039-bb6b-e73063b5deb0">


Once the formatting is complete, verify the filesystem type with:

sudo file -s /dev/sdb1

This should indicate that the partition is now formatted as ext4.

<img width="459" alt="sxsxxs" src="https://github.com/user-attachments/assets/ce5f7912-469e-4e77-97a7-264f7ecc00ea">


# 6. Advanced Configuration

No advanced configurations (like encryption or compression) are required for this specific task, so we will skip this step.

# 7. Mount the Newly Created Filesystem

Create the mount point directory /mnt/data where the partition will be mounted:

sudo mkdir -p /mnt/data

Next, mount the partition /dev/sdb1 to the newly created mount point:

sudo mount /dev/sdb1 /mnt/data

Verify that the partition is mounted successfully:

df -h

You should see /dev/sdb1 mounted at /mnt/data.

<img width="323" alt="cdcd" src="https://github.com/user-attachments/assets/6b6cb968-3cf5-4e50-a3d4-532ad6ebb7d2">


# 8. Configure Automatic Mounting at Boot

To ensure that the filesystem is automatically mounted at boot time, we need to update the /etc/fstab file.

Open /etc/fstab with a text editor (e.g., nano):

sudo nano /etc/fstab

Add the following line at the end of the file:

/dev/sdb1  /mnt/data  ext4  defaults  0  2

This line configures /dev/sdb1 to mount automatically to /mnt/data using the ext4 filesystem at boot.

<img width="455" alt="mlm" src="https://github.com/user-attachments/assets/33b1894e-e6b0-4713-9ca3-06299b160bd1">


Save and close the editor.

Test the /etc/fstab configuration by unmounting the filesystem and using mount -a to mount all filesystems listed in /etc/fstab:

sudo umount /mnt/data
sudo mount -a
Then, check again to ensure the partition is mounted:

df -h

<img width="322" alt="cscs" src="https://github.com/user-attachments/assets/09066090-a4b7-4190-8cc4-91ce6d762c33">

# 9. Verify the Successful Creation and Configuration

At this point, you need to verify the following:

Filesystem Creation: Check the filesystem type on the partition:

sudo file -s /dev/sdb1

<img width="450" alt="cdcdcd" src="https://github.com/user-attachments/assets/7082ea8a-1bb6-4874-a895-a7b39a95482a">


It should return ext4 as the filesystem type.

Mount Status: Ensure that /dev/sdb1 is mounted correctly to /mnt/data:

df -h /mnt/data

The mount point should display /mnt/data along with the used/available space.

<img width="299" alt="asas" src="https://github.com/user-attachments/assets/7d2e3c6d-6bac-4dc5-85d6-536902b8a463">

# 10. Conduct Thorough Testing

Perform basic checks and tests to ensure the filesystem works as expected:

10.1. Write and Read Test
Create a test file in the mounted filesystem to verify that the partition is writable:

sudo touch /mnt/data/testfile
sudo ls -l /mnt/data
You should see the newly created testfile in the directory.

<img width="325" alt="zaza" src="https://github.com/user-attachments/assets/cb6fd023-05fb-4548-b5e8-17399e3245c1">


10.2. Data Integrity Test
Write some data into the test file and read it back:

sudo echo "Test data for verification" > /mnt/data/testfile
cat /mnt/data/testfile

This verifies that you can write and read data successfully on the filesystem.

10.3. Disk Usage Test

Check the available disk space on the new mount point to confirm that it's correct:

df -h /mnt/data

This ensures that the partition is recognized with the correct size and available space.

<img width="296" alt="dsd" src="https://github.com/user-attachments/assets/b9ef82a0-33fc-43ab-bde6-be154eb281a1">


# Summary: Partitioning, Formatting, and Configuring Filesystems on CentOS

Identify Available Storage Devices: Use lsblk to identify the new storage device (e.g., /dev/sdb).

Select the Storage Device: Confirm /dev/sdb is the correct new disk.

Create Partitions: Use fdisk to create a single primary partition (/dev/sdb1).

Choose Filesystem Type: Select ext4 as the filesystem type.

Format the Partition: Format /dev/sdb1 with ext4 using sudo mkfs.ext4 /dev/sdb1.

Advanced Configuration: No advanced configuration needed (e.g., encryption or compression).

Mount the Filesystem: Create the mount point (/mnt/data) and mount the partition with sudo mount /dev/sdb1 /mnt/data.

Automatic Mounting: Edit /etc/fstab to add:

/dev/sdb1  /mnt/data  ext4  defaults  0  2
This ensures automatic mounting at boot.

Verify Configuration: Verify the filesystem is mounted and functioning properly with df -h.

Test the Filesystem: Perform write/read tests and check disk usage to ensure data integrity and accessibility.

This process successfully partitions, formats, mounts, and configures the new storage device, ensuring it is set up for automatic mounting and tested for proper functionality.






