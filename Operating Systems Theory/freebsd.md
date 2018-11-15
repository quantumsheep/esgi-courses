# FreeBSD
## Installation
In the way to install FreeBSD, we start the bootonly (https://download.freebsd.org/ftp/releases/amd64/amd64/ISO-IMAGES/) in Live CD mode.

- Change the keyboard layout with `kbdcontrol -l <LANG>`, for example `kbdcontrol -l fr`

### Disk partitionning

We actually has a disk in `/dev` which is named `da0`. The following commands use `da0`.

- Initialize the disk with `gpart create -s gpt da0`
- Define the bootloader to a specific disk with `gpart bootcode -b /boot/pmbr d0a`
- Write the bootloader of a 512 Ko size  code with `gpart add -t freebsd-boot -s 512k da0`
- Add EFI part to da0 of a 800 Ko size `gpart add -t efi -s 800k da0`
- Add GPT part to da0 `gpart add -p /boot/gptboot -i 1 da0`

Then, we will split our remaining disk space:
```
gpart add -t freebsd-ufs -s 1g -l fb_root da0
gpart add -t freebsd-ufs -s 4g -l fb_usr da0
gpart add -t freebsd-ufs -s 2g -l fb_var da0
gpart add -t freebsd-swap -s 2g -l fb_swap da0
```

Those commands are used to create partitions, UFS (Unix File System) are the disk parts that will be used to store our data. SWAP is used to make a temporary files disk for data exchange.

When creating this configuration, we got the following disk partitioning (from a 20 Go disk):
```
[ MBR   | Table part (GPT) | da0 1  | da0 2 | / (root) | /usb | /var | Free space ]
  40 Ko   512 Ko             800 Ko   1 Go    1 Go       4 Go   2 Go   11 Go
```

`gpart show -l` returns the following configuration:
```
=>          40 41942960  da0   GPT      (20G)
            40     1024    1  (null)    (512K)
          1064     1600    2  (null)    (800K)
          2664  2097152    3  fb_root   (1.0G)
       2099816  8388608    4  fb_usr    (4.0G)
      10488424  4194304    5  fb_var    (2.0G)
      14682728  4194304    6  fb_swap   (2.0G)
      18877032 23065968       - free -  (11G)
```


Then we initialize the partitions as a file system:
```
newfs gpt/fb_root
newfs -U gpt/fb_usr
newfs -U gpt/fb_var
```

And we mount the partitions to `/tmp/fb`:
```
mount /dev/gpt/fb_root /tmp/fb
mount /dev/gpt/fb_usr /tmp/fb/usr
mount /dev/gpt/fb_var /tmp/fb/var
```
Sometimes, directories doesn't create by themself, we need to use `mkdir` to create the missing directories.


### Files installation
To download the FreeBSD components, we first connect to internet using `dhclient`. To do this, we use `ifconfig` to check interfaces names, we take the non-loopback interface and bound it to the network connection with `dhclient <INTERFACE>`. And you we got internet!

Then we download the components (remember to move to the `/tmp/fb` directory before doing a fetch).

```
fetch http://ftp.fr.freebsd.org/pub/FreeBSD/releases/amd64/11.2-RELEASE/base.txz
fetch http://ftp.fr.freebsd.org/pub/FreeBSD/releases/amd64/11.2-RELEASE/kernel.txz
```
And decompress them:
```
tar -xpzf base.txz -C /tmp/fb
tar -xpzf kernel.txz -C /tmp/fb
```

## Facts
- The bootloader is able to understand the Kernel. The kernel is stocked in the file /dev/ 

## Commands
- kbdmap -> CLI to help the user to change its keyboard layout
- gpart -> Partionning CLI tool
  - `gpart show -l` to list the partitions
  - `gpart add -t <TYPE> -s <SIZE> -l <NAME> <DISK>` to add a partition
  - `gpart delete -i <DISK>` to delete a partition
- newfs -> Define a disk part as a File System
  - `newfs -U <MOUNT_POINT>` activate differed cache writing for the disk part, to improve performance
