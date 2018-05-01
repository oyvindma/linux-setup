# linux-setup
Just some notes and gotchas for later

## How to spin down external usb drives:

### Identify drive
Source: http://www.howtoeverything.net/linux/hardware/how-distinctively-identify-hard-drive-and-why-important
sudo lshw -class disk

Together with the size you should now which drive is which now. Now you'll need to know the unique identifiers your system gave your disks. Get a list with the following command:

sudo ls /dev/disk/by-id

The hard disk you are searching for should be listed in a format that is comparable to

scsi_SATA_PRODUCT_SERIAL or ata_PRODUCT_SERIAL

the -part1 -part2 etc. ones are the partitions and not the drives. If you want to use hdparm to configure the drive, always use the drive and not the partition identifiers.  

### Permanently configure to spin down
http://www.howtoeverything.net/linux/hardware/permanently-configure-hard-drives-spin-down-after-given-idle-time

hdparm -B /dev/sd?

The drives that are set to a value higher than 127 (except 255) won't spin down, the drives that show "not supported" either manage this value on their own or are that old that they are not supporting power management at all. Talking about sata disks, the former is the most possible reason.

To set the drive to the highest power saving level with the highest performance and yet allow it to spin down after 15 minutes type the following command in your /etc/rc.local file. Product and Serial being the output from sudo lshw -class disk

hdparm -B127 -S180 /dev/disk/by-id/ata-PRODUCT_SERIAL

if this results in your hard drive spinning down every minute or so and not after the time you chose, instead use:

hdparm -B255 -S180 /dev/disk/by-id/ata-PRODUCT_SERIAL

sudo gedit /etc/rc.local
