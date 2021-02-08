## How to Manage Linux ACL using Setfacl and Getfacl
### Table of contents

[I. Introducton](#modau)

[II. Getting Start](#batdau)
- [1. Step 1: Check if Kernel has ACL Support](#step1)
- [2. Step 2: Enable ACL on partion](#step2)
- [3. Step 3: Remove permision for other user](#step3)
- [4. Step 4: Providing ACL for an individual User](#step4)
- [5. Step 5: Providing ACL for all users of a group](#step5)
- [6. Step 6: Revoking ACL of a user/group](#step6)
- [7. Step 7: Copying ACL of one file/directory to another](#step7)

[III. Summary](#Tongket)

===========================

<a name="Modau"></a>
## I. Introduction
- The Linux command setfacl allows users to set extensive Access Control Lists on files and directories. Normally, using chmod command, you will be able to set permissions for the owner/group/others. But, in case you may need to provide file permissions for some other users too, that can’t be done using chmod. Setfacl will assist you to get rid of such troubles.

- For example, we cannot set up different permission sets for different users on same directory or file. Thus, Access Control Lists (ACLs) were implemented. You can view the current ACL set on files and directories using getfacl command.

- In order to use setfacl on a file/directory, the residing filesystem should have acl support enabled. If the filesystem doesn’t support acl, you will get “operation not supported” error. In that case, you need to add acl support to the filesystem in /etc/fstab as follows and then remount the filesystem.

<a name="batdau"></a>
## II. Getting Start:

<a name="step1"></a>
## Step 1: Check if Kernel has ACL Support
- Run the following command to check for ACL Support for file system and POSIX_ACL=Y option (if there is N instead of Y, then it means Kernel doesn’t support ACL and needs to be recompiled).

> $ grep -i acl /boot/config*

``` sh
CONFIG_EXT4_FS_POSIX_ACL=y
CONFIG_XFS_POSIX_ACL=y
CONFIG_BTRFS_FS_POSIX_ACL=y
CONFIG_FS_POSIX_ACL=y
CONFIG_GENERIC_ACL=y
CONFIG_TMPFS_POSIX_ACL=y
CONFIG_NFS_V3_ACL=y
CONFIG_NFSD_V2_ACL=y`		
CONFIG_NFSD_V3_ACL=y
CONFIG_NFS_ACL_SUPPORT=m
CONFIG_CEPH_FS_POSIX_ACL=y
CONFIG_CIFS_ACL=y
```

<a name="step2"></a>
## Step 2: Enable ACL on partion

> $ vi /etc/fstab

``` sh
UUID=456346 	/mnt/data 		ext4 defaults,user_xattr,acl
```
`user_xattr # Enable Extended User Attributes`

- Check check ext* formatted partitions

> $ tune2fs -l /dev/sdb | grep "Default mount options:"

``` sh	
Default mount options:    user_xattr acl
```

-  remount

> $ mount -o remount /mnt/data

<a name="step3"></a>
## Step 3: Remove permision for other user:

> $ chmod 750 /data/Public/Admin

<a name="step4"></a>
## Step 4: Providing ACL for an individual User
- Suppose, you want to give full access to the user “admin1” (it can be any user at all) on the directory “Admin”. This can be done using setfacl as follows.

> $ setfacl -m u:admin1:rwx /mnt/data/Public/Admin

<a name="step5"></a>
## Step 5: Providing ACL for all users of a group
- If you want to provide write access permission for all the users of the group “Gadmin” to the folder “Admin”, you can do it the following way.

> $ setfacl -m g:Gadmin:rwx /mnt/data/Public/Admin

``` sh

```

<a name="step6"></a>
## Step 6: Revoking ACL of a user/group
- For removing ACL from any file/directory, we use x and b options as shown below.

> $ setfacl -x ACL file/directory  	# remove only specified ACL from file/directory.

> $ setfacl -b  file/directory   		#removing all ACL from file/direcoty

> $ setfacl -x u:user1 /mnt/data

> $ setfacl -d -R -m other:--- ./Mep

** man setfacl:
-d, --default
       All  operations  apply to the Default ACL. // this folder , subfolders and files

-R, --recursive
       Apply operations to all files and directories recursively. 

-m, --modify
       Options to modify the ACL of a file or directory.

# setfacl -Rdm u:www-data:rwx,u:ddn:rwx shared/web/cache

<a name="step7"></a>
## Step 7: Copying ACL of one file/directory to another
- Suppose, you want to have the same ACL set of test_folder on test_folder1 too, you can set it by copying the ACL as follows.

> $ getfacl test_folder/ > acl.txt

> $ mkdir test_folder1

> $ setfacl -M acl.txt test_folder1/

> $ getfacl test_folder1/

``` sh
# file: test_folder1/
# owner: root
# group: root
user::rwx
user:test:rwx
group::r-x
group:testg:-w-
mask::rwx
other::r-x
```

<a name="tongket"></a>
## III. Summary:
In this tutorial, we have seen the basic usage of getfacl and setfacl tools for Access Control Lists to set and revoke some permissions to test_folder. We also learned how to check for Kernel and filesystem acl support and how to install the required packages. If you have any thoughts or comments about linux acl, please write it down in the comments section below.

**Watch Video here:** 

- [How to Install and configure Samba on Ubuntu 20.04 Part 1:  Public folder](https://youtu.be/2o5zgA8ml38)
- [How to configure and Install Samba on Ubuntu 20.04 Part2: Private Share](https://youtu.be/6s9ZEp3xS94)

**Contact me:**
- Email: manhhungbl@gmail.com
- Skype: spyerx3
- Youtube Channel: youtube.com/howtoused

Thank you very much!




