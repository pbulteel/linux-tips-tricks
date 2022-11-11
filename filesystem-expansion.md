# File system Expansion

When you've gone through your [file system checks](filesystem-checks.md) and discovered that you need more space, you're going to have to expand the disk or move data around.

If your server is a virtual machine, this is relatively easy. You could add another disk or VMware allows you to expand a disk (vmdk) and increase it's size. You then need to deal with it on the OS. On a physical host where you can only add more disks you can do this too. You add the disk and then expand the LVM volume to take over that disk. Then you adjust your partitions accordingly.

## Commands we're (potentially) going to use

`fdisk` - to check disk sizes, number of partitions and partition types

`partprobe` - to have the disk sizes and partitions and number of disks refreshed

`partx` - a different way to have the disk sizes and partitions and number of disks refreshed

`vgextend` - to extend the volume group onto another partition

`pvscan` - check that the pysical volume has expanded

`lvextend` - to extend the logical volume increasing the partition space

`expand2fs` - for the underlying filesystem (ext3 or ext4) to expand

`xfs_info` - get information of your xfs filesystem, if that's what you're using.

`xfs_growfs` - to extend and grow the xfs partition (equivalent of expand2fs for ext3/4)

## Increasing space by adding another disk

Use fdisk -l to check the sizes of the physical disks and what type of partition they are.

``` bash
fdisk -l /dev/sda
```

If you're not rebooting the machine, you will need to make the machine aware of the new device(s) and their size. The following loop does that.

``` bash
for disk in $(ls -1 /sys/class/scsi_device/); do
    echo 1 > /sys/class/scsi_device/${disk}/device/rescan
done

for host in $(ls -1 /sys/class/scsi_host/); do
    echo "- - -" > /sys/class/scsi_host/${host}/scan
done
```

Check the drive sizes again and that there's space available. You can create another disk partition and expand the logical volume onto it. Some distributions use specific volume group names, so my examples below show 2 such situations.

``` bash
fdisk -l /dev/sda

fdisk /dev/sda 
n
p
3
<enter>
<enter>
t
8e
w

partprobe -s 

partx -v -a /dev/sda
```

To find out what values you'd need for some of the volume group, volume name and volume path options below you could run the following commands.

``` bash
sudo lvscan | cut -d "'" -f 2
sudo lvs --noheadings -o lv_path | tr -d '  '
sudo vgs --noheadings -o vg_name | tr -d '  '
sudo lvs --noheadings -o lv_name | tr -d '  '
sudo pvs --noheadings -o pv_name | tr -d '  '
```

``` bash
vgextend VolGroup00 /dev/sda3 (centos)
vgextend cl /dev/sda3 (ubuntu)

pvscan

lvextend /dev/mapper/cl /dev/sda3
lvextend /dev/mapper/vg-name /dev/sda3
```

Now you have to resise the actual filesystem. This depends greatly on the OS and filesystem you use.

### ext4

``` bash
resize2fs /dev/mapper/cl-var
```

### xfs

``` bash
xfs_info
(lvextend first)
xfs_growfs /usr
```

Example workflow for a VM with a disk space issue.

``` bash
fdisk -l
for disk in $(ls -1 /sys/class/scsi_device/); do
    echo 1 > /sys/class/scsi_device/${disk}/device/rescan; 
done
for host in $(ls -1 /sys/class/scsi_host/); do
    echo "- - -" > /sys/class/scsi_host/${host}/scan; 
done
lvdisplay
pvdisplay
ls /sys/class/scsi_device/
echo 1 > /sys/class/scsi_device/2\:0\:0:\0/device/rescan
vgdisplay 
fdisk -l /dev/sda   #see additional space
fdisk /dev/sda   #create new primary partition, type lvm (n p  enter enter t (part number) 8e)
partprobe -s
pvcreate /dev/sda4
vgdisplay
vgextend local /dev/sda4
pvscan
lvextend /dev/mapper/local-varlog 
vgs

sudo lvresize -L +1G /dev/mapper/local-varlog 
sudo resize2fs /dev/mapper/local-varlog
```

## Shinking

Sometimes what you need to do is reduce the size of one partition (volume) to make space available for another partition. This works best if the partition you are making smaller consists of empty space. An example scenario is one where you accidentally created a large /home partition but as a server, you're not using that space. You can re-allocate that by first reducting /home and then increasing a different one.

``` bash
umount /dev/mapper/local-varlog
lvresize --resizefs --size SIZE /dev/mapper/local-varlog
```

SIZE is your intended new size for the LV and file system, such as 200G. `--resizefs` lets lvresize also resize underlying filesystem together with the logical volume. It is better than resize2fs + lvresize since it is not unlikely for an admin to accidently use inconsistent parameter for resize2fs and lvresize.
