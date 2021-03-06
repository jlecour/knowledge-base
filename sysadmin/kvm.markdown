# KVM #

## Basic usage ##

Debian installation :

~~~~~
qemu-img create -f qcow2 /mnt/kvm/example.qcow2 20G
kvm -hda /mnt/kvm/example.qcow2 -cdrom /mnt/share/comp/debian-6.0.7-a-CD-1.iso -boot d -m 1024 -k fr
~~~~~

## Mount qcow2 images ##

Install NBD client tools :

~~~~~
apt-get install nbd-client
~~~~~

Connect gcow2 image to NBD device `/dev/nbd0` :

~~~~~
qemu-nbd --connect=/dev/nbd0 example1.qcow2
~~~~~

List disk partitions :

~~~~~
fdisk -l /dev/nbd0
~~~~~

Umount NBD device :

~~~~~
nbd-client -d /dev/nbd0
~~~~~

## Optimize VM image size

The `zerofree` utility can be effective to optimize the size of a `sparse` VM
image. It works by zero-ing unused blocks on a ext2/ext3/ext4 filesystem.

Then, `cp --sparse=always` can be used to replace zeros by holes.

