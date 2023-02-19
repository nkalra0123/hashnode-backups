# Learnings from the book "The Linux Command Line"

This is a series of articles of my learnings from the book The Linux Command Line.

**Today** I learned something new about “**file system table**” aka file named /etc/fstab

This file lists the devices (typically hard disk partitions) that are to be mounted at boot time.

If you want to mount disks at some particular location on every boot, you might have edited this file.

I have edited this file multiple times, but never looked at the last 3 options (options, dump and pass)


Lets have a look at this file.

## /etc/fstab: static file system information
 A typical entry looks like this 
<file system> <mount point>   <type>  <options>       <dump>  <pass>
UUID=b1d35006-a107-433e-bed9-88ac3e583d1a /               ext4    errors=remount-ro 0       1

  
The first field (fs_spec).

- **file system**

This field contains the actual name of a device file associated with the physical device, such as /dev/sda1 (the first partition of the first detected hard disk). 
But with many devices that are hot pluggable (like USB drives or EBS disks in AWS), this is generally a text label instead.

> LABEL=<label> or UUID=<uuid> is commonly used here. 
This label (which is added to the storage media when it is formatted) can be either a
simple text label or a randomly generated UUID (Universally Unique Identifier). 

This label is read by the operating system when the device is attached to the
system. That way, no matter which device file is assigned to the actual physical device, it can still be correctly identified.

- **mount point**

The directory where the device is attached to the file system tree.

- **type**

File system type,  Linux allows many file system types to be mounted.
Most file systems are Fourth Extended File System (ext4), but many others are supported, such as FAT16 (msdos), FAT32 (vfat), NTFS (ntfs), CD-ROM (iso9660), etc.

- **options**

File systems can be mounted with various options. It is possible, for example, to mount file systems as readonly or to prevent any programs from being executed from them (a useful security feature for removable media).

Basic filesystem-independent options are:

       defaults
           use default options: rw, suid, dev, exec, auto, nouser, and
           async.

       noauto
           do not mount when mount -a is given (e.g., at boot time)

       user
           allow a user to mount

       owner
           allow device owner to mount

       comment
           or x-<name> for use by fstab-maintaining programs

       nofail
           do not report errors for this device if it does not exist.


- **dump**

A single number that specifies if and when a file
system is to be backed up with the dump command.

This field is used by [dump](https://linux.die.net/man/8/dump) to determine which filesystems need to be dumped. Defaults to zero (don’t dump) if not present.

Have you ever used dump command, I have never seen anyone use that. If yes, share your story.

> For creating a backup you can use dump, and to restore you can use restore command

I created a backup of ext4 fs and was able to successfully restore it with dump and restore commands.

- **pass**

determine the order in which filesystem checks are done at boot time, with the fsck command. The root filesystem should be specified with a fs_passno of 1. Other filesystems should have a fs_passno of 2. 

0 means don't check 

fsck program (short for “file system check”). 

> fsck shares three letters with a different word. that you will probably
be uttering if you find yourself in a situation where you are forced to run fsck.

You can download the book from [here](https://linuxcommand.org/tlcl.php) 

My this year's target is two read 3 books each month. I share my progress each month on Twiter. You can follow me, and learn something new with me.

%[https://twitter.com/nkalra0123/status/1488765699417784320]
