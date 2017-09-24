# *NIX-common_issues
## apt update [DEBIAN/UBUNTU]
Following are some common errors that may arise while running `$ apt update`.

It is very easy to upgrade and manage software and packages with systems based on Debian or Ubuntu using `$ apt update` or `$ apt-get update`. But sometimes it can be rather painful.

### Solving an expired key (KEYEXPIRED) with apt

* Full list of known keys can be viewed using:
```
$ apt-key list
```

* List only *expired* repository keys:
```
$ apt-key list | grep "expired"
```
* Update the gpg key. Run:
```
$ sudo apt-key adv --keyserver keys.gnupg.net --recv-keys <KEY>
```
* Update
After renewing the expired key, `apt update` can be run again an you can install any available upgrades
```
$ sudo apt-get update && apt-get upgrade
```

## issues with $PATH env variable
Installing some programs might require you to add the directory to your system's `$PATH` variable.  
You can change $PATH using `export` command.
Example: 
```
$ export PATH=newPATH 
```
However, this will completely replace `$PATH` with `newPATH`.  
Items can be added (+=) to an already populated `$PATH` variable using a `:` (colon-separated paths).  
Example:
```
$ export PATH=$PATH:newPATH
```
newPath can be concatenated at the end preceded by `$PATH:` like above, or it can be added to the beginning of the path.
Example:
```
$ export PATH=newPATH:$PATH
```
Doing this, the terminal will first search for the program at `newPATH` first.

One of the consequences of having an inaccurate `$PATH` variable is that the shell will not be able to find and execute programs without a full path.

Also, setting paths in `$PATH` on terminal is temporary. It needs to be set everytime you start a shell. In order to eliminate this, the usual way would be to edit the `.bashrc` file. This gets invoked everytime a new shell session is started, hence `newPath` will be available in `$PATH` everytime a terminal is opened.  
Example:
* Open .bashrc in gedit (sudo, if not su)
```
$ sudo gedit ~/.bashrc
```
Add `export PATH=newPATH:$PATH` or `export PATH=$PATH:newPATH` to this file. Save and exit.
* Now reload .bashrc in order for it to work with the new configuration.
```
$ source ~/.bashrc
```

## BAD SUPERBLOCK
### The following steps will guide you through the steps involved in order to get back (curropted) data due to a BAD SUPERBLOCK.

Sometimes, upon re(start/boot), you might encounter an unusual startup that'd take you to a terminal saying: 
` /dev/sda7: Input/output error `
` mount: /dev/sda7: can't read superblock `

Linux ext2/ext3 filesystem stores superblocks at different backup location, so it is possible to get back data from the corrupted partition.
If you end up at the terminal upon the boot-up of your system, follow the below steps:
## Mount partition using alternate superblock: 
#### Find out superblock location for /dev/sda7:
```
# dumpe2fs /dev/sda7 | grep 'superblock'
```
**OUTPUT:** 
> Primary superblock at 0, Group descriptors at 1-6
  Backup superblock at 32768, Group descriptors at 32769-32774
  Backup superblock at 98304, Group descriptors at 98305-98310
  Backup superblock at 163840, Group descriptors at 163841-163846
  Backup superblock at 229376, Group descriptors at 229377-229382
  Backup superblock at 294912, Group descriptors at 294913-294918
  Backup superblock at 819200, Group descriptors at 819201-819206
  Backup superblock at 884736, Group descriptors at 884737-884742
  Backup superblock at 1605632, Group descriptors at 1605633-1605638
  Backup superblock at 2654208, Group descriptors at 2654209-2654214
  Backup superblock at 4096000, Group descriptors at 4096001-4096006
  Backup superblock at 7962624, Group descriptors at 7962625-7962630
  Backup superblock at 11239424, Group descriptors at 11239425-11239430
  Backup superblock at 20480000, Group descriptors at 20480001-20480006
  Backup superblock at 23887872, Group descriptors at 23887873-23887878
 
### Repair *NIX Filesystem using any alternate superblock (#32768)
``` 
# fsck -b 32768 /dev/sda2 -a
```
> When filesystem errors prevent a reboot, you are instructed to run fsck on the affected filesystem. If there are lots of errors, you may be stuck typing y for hours. [-a] instructs fsck to default all answers to YES.

**OUTPUT:**
>fsck 1.40.2 (24-Sep-2017)
e2fsck 1.40.2 (24-Sep-2017)
/dev/sda7 was not cleanly unmounted, check forced.
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
Free blocks count wrong for group #241 (32254, counted=32253).
Fix? yes

>Free blocks count wrong for group #362 (32254, counted=32248).
Fix? yes

>Free blocks count wrong for group #368 (32254, counted=27774).
Fix? yes
..........
/dev/sda2: ***** FILE SYSTEM WAS MODIFIED *****
/dev/sda2: 59586/30539776 files (0.6% non-contiguous), 3604682/61059048 blocks
