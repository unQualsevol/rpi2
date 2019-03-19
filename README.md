# rpi2
## Requirements
Only boot from SD, the main partition is on a Hard drive by USB
Have a partition mounted at start to be used as storage
Fixed IP
Transmission Daemon Installed
FlexGet installed and configured to read RSS
Samba to access to edit files
Kodi installed
Kodi starts by default
Can exit, reboot or shutdown from Kodi

## Only boot from SD, the main partition is on a Hard drive by USB
TODO: refresh how to create the SD and modify the boot file

Mount the hard drive where the partition needs to be.

mount /dev/sd** /mnt

copy the content of the root partition to the new location

rsync -avx / /mnt  

backup  /boot/cmdline.txt

cp /boot/cmdline.txt /boot/cmdline.txt.bak

edit /boot/cmdline.txt

set root=/dev/sd** to point to the partition to load.

Restart the rpi it should boot the system from 

# Have a partition mounted at start to be used as storage

Install NTFS driver if needed:

sudo apt-get install ntfs-3g

find the partition to load

sudo blkid

e.g. /dev/sd**: LABEL="blablabla" UUID="code" type="ntfs" PARTUUID="code"

or

sudo fdisk -l

Create a location for mount point:

sudo mkdir /mnt/volume

Give proper permission:

sudo chmod 775 /mnt/volume

Mount the USB Drive and then check if it is accessible at /mnt/volume

sudo mount /dev/sd** /mnt/volume

Edit the fstab, but first backup it

sudo cp /etc/fstab /etc/fstab.backup
sudo nano /etc/fstab

/dev/sda1 /mnt/volume ntfs defaults 0 0

Reboot

sudo reboot

# Fixed IP

sudo nano /etc/dhcpcd.conf

include

interface eth0

static ip_address=192.168.0.10/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1

# Transmission daemon Installed
