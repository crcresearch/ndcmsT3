# Storage

All storage is allocated to users and attributed to their account sponsor (faculty advisor). To see the storage used by a particular faculty advisor simply type their ND ID into the [storage reporting tool](http://www.crc.nd.edu/info/storage.html).

## AFS Storage

The CRC's AFS cell consists of several different AFS servers. Both user home storage and research group storage space resides within the CRC AFS cell. Some note:

- All storage allocation is tracked per faculty sponsor.

- Default of 100 GB user space per account.

  > - This can be increased through request. Any amount higher than 500GB must be confirmed by the faculty sponsor of the account.

### AFS Limitations and Notes

There are a few limitations to be aware of within AFS.

- A single directory can only contain a certain number of files. This number depends on the length of the filenames of each file within said directory. For example, ~32,000 is generally an average number of files which can be within a single directory. If filenames are long, this max number will be smaller.

  > - If you hit the maximum number of files within a single directory, you'll see an error such as:
  >
  >       touch: cannot touch 'path/to/dir/file_name.txt': File too large

- A single volume within AFS can be at a maximum of 2 TB. Should you need more than 2 TB, multiple volumes will have to be created and any data will need to be split between multiple volumes.

- If you have a running job, you will not see the results of creation / deletion etc of files from your job until it completes. If you would like to see file updates in real time, see the `synchronizing_files_afs` page.

### AFS Group Storage

Often research teams require a shared access storage space. The two primary considerations in this regard are understanding the file structure respective of quotas and the access list.

We can mount volumes anywhere in the directory tree thus quotas only map to the volume mount point. In this example any file or subdirectory you place in directory researchgroupname will be part of the associated 2TB quota EXCEPT the dataset01 sub-directory which has a separate 2TB quota because it is the mount point of a new volume.

Thus 10TB could be allocated as:

    /afs/crc.nd.edu/group/researchgroupname/dataset01

    /afs/crc.nd.edu/group/researchgroupname/dataset02

    /afs/crc.nd.edu/group/researchgroupname/dataset03

    /afs/crc.nd.edu/group/researchgroupname/dataset04

with each `dataset0X` directory having 2TB of quota and the parent directory having capacity for 2TB of files in addition to the 4 `dataset0X` directories.

### AFS Website

If you would like to obtain a CRC AFS website (e.g. crc.nd.edu/~NETID), please send a request for one to <CRCSupport@nd.edu> from your netID email address. You will also need to make sure that there is a folder with the name www within your CRC Home directory.

> [!WARNING]
> All websites within the CRC AFS webspace should be considered to be INSECURE and are world-readable. You should not place any sensitive or confidential material in your webspace. Anything placed in your webspace can be viewed by anyone in the world.

### AFS Permissions / ACLs

AFS does not use normal POSIX file permissions. To manage access to your files / directories, you must apply ACLs (Access Control Lists). For details on single user ACLs and the creation of ACL groups (better for labs etc), see the `user_acls`.

------------------------------------------------------------------------

## Panasas (scratch) Storage

For high performance scratch space, the CRC utilizes Panasas ActiveStor. To obtain scratch space, please send a request to <CRCSupport@nd.edu>. Some important notes about scratch365:

- Your scratch directory is located at `/scratch365/$USER`.
- scratch365 is designed to be **TEMPORARY** storage space and it is very important that you understand this space is **NOT BACKED UP**. A failure in the scratch filesystem could result in permanent data loss.
- scratch365 is subject to a 365 day **PURGE POLICY**. Data older than 365 days will automatically **BE DELETED** from the filesystem.
- Important data and results that you wish to keep should be moved to AFS and/or downloaded to a local computer as soon possible. We recommend doing this at least once a week for files that are frequently updated.

### Panasas Best Practices

The Panasas resource is shared among several hundred users. In order to minimize the impact of your simulations on others please try to follow these guidelines. We encourage you to reach out to us if you have any questions or concerns.

- Delete temporary or intermediate files that are not needed for future analysis. This also applies to data that can easily be regenerated.
- Avoid using `--color` options to `ls` and other similar programs when working in scratch. While color output can be very helpful, programs like `ls` that offer this option put extra load on the filesystem as each file in the directory must have its contents examined in order to determine the file type and other attributes. Many people set up aliases for `ls` and other commands to automatically include color options. When in doubt, use the full path like so:

``` shell
/bin/ls -l /scratch365/$USER
```

- Use the `tar` command to store large amounts of data as a single file. Panasas and other high performance file systems excel at storing large files, particularly in lieu of thousands (or millions) of small files. The `tar` command allows you to efficiently replace many files and directories with a single, combined file.

``` shell
#  Create a tar file named mydata.tar and remove existing files after adding them to the .tar:
tar --remove-files --verbose --create -f mydata.tar ./directory-with-lots-of-files other-file different-file

#  See the contents of an existing tar file:
tar --list -f mydata.tar

#  To unpack a tar file named mydata.tar and then remove the existing tar file:
tar --verbose --extract -f mydata.tar; rm -i mydata.tar
```

- Minimize the number of files in a single directory, preferably with no more than 5,000 files per directory. Many commands that work on files must parse the entire contents of a directory before the command is able to complete. This is usually a linear process, so the more files there are in a directory the longer and more resources the action will take.
  - It is much more efficient to have 100 directories that contain 1,000 files each rather than a single directory with 100,000 files.
  - If you must have thousands of files per directory, consider using the command `tar` to encapsulate them into a single file when they are not actively being used.
- When you are writing your own applications that include file I/O:
  - Only `open()` and `close()` files a single time inside of your application and never inside of loops.
  - Try to avoid using file I/O operations inside of operational `for()` or `while()` loops.
    - If this is necessary, program your application to read and write your data in large **chunks**. Use local variables to store many values.
      - It is much more efficient to read or write 10 operations of 2 megabytes each than 1,000 operations of 20 kilobytes each.
- Potentially make use of the local hard drive.
  - Every compute node has a local hard drive in it with a writable folder located in /tmp. You may find that copying files to a unique directory in /tmp and running your program from there will improve performance of your job and/or reduce load on the Panasas.

``` shell
#  Create a temporary directory on /tmp, do some work, copy the data out of /tmp, then remove the directory.
MY_TEMP=$(mktemp -d)
cp /scratch365/$USER/collection_of_small_files.tar "$MY_TEMP"
# do work here
cp /$MY_TEMP/results /scratch365/$USER
/bin/rm -r $MY_TEMP
```

Danger

Note that the local hard drive, /tmp, typically only has a couple 100 GBs of available storage. Filling up the local hard drive will cause the server to crash, so it is important to use the space sparingly and remove any files that you create there.

------------------------------------------------------------------------

## Local /tmp Space

Every compute host has a local disk which contains the OS and a small amount of space available to users. This local disk will normally be faster for higher I/O bound jobs, in addition to relieving network congestion to the other networked file systems available.

The amount of storage available on the local disk varies by which machine the job is running on. Some note:

- Local disk is mounted on `/tmp`. Anyone can use this area.
- File and directory access is controlled by normal Unix permissions.
- If you plan on using this space be sure to clean up after yourself.
- Be mindful of the amount of space used on `/tmp`, if it runs out the machine will crash.

## Storage Backups

Backup in this context refers to an independent copy of user files regularly changing in either a distributed filesystem (AFS) or local filesystem on a dedicated research machine. This is an automated cyclic process where changing data is incrementally backed up (daily, weekly, monthly). Backups are only maintained for 3 months (not persistent).

For backups, the CRC utilizes a [SpectraLogic T950 system](https://spectralogic.com/products/spectra-t950/).

------------------------------------------------------------------------

## Storage Comparison

Below is a scrollable comparison between the different storage options within the CRC.

[TABLE]
