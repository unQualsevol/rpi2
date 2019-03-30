# rpi2
## Requirements
Only boot from SD, the main partition is on a Hard drive by USB

Have a partition mounted at start to be used as storage

Fixed IP

Transmission Daemon Installed

Git installed

FlexGet installed and configured to read RSS

Samba to access to edit files

Kodi installed

Kodi starts by default

Can exit, reboot or shutdown from Kodi

## Enable SSH
Enter sudo raspi-config in a terminal window

Select Interfacing Options

Navigate to and select SSH

Choose Yes

Select Ok

Choose Finish


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

## Have a partition mounted at start to be used as storage

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

## Fixed IP

sudo nano /etc/dhcpcd.conf

include

interface eth0

static ip_address=192.168.0.10/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1

## Transmission daemon Installed

sudo apt-get install transmission-daemon

Before changing any configuration always turn off the service:

sudo systemctl stop transmission-daemon

Edit configuration file

sudo nano /etc/transmission-daemon/settings.json

change:
  
"download-dir": "/home/pi/elements/Torrent",
"incomplete-dir": "/home/pi/elements/inprogress",
"incomplete-dir-enabled": truee,

"rpc-password": "new password",
"rpc-username": "new username",
    
Change these two to allow restricted access via IP

"rpc-whitelist": "127.0.0.1,192.168.*.*",
"rpc-whitelist-enabled": true,

## Git installed

sudo apt-get install git

create ssh key (if needed)

https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

Setup your new key in your Git
## FlexGet installed and configured to read RSS
https://flexget.com/InstallWizard/Linux

Check python version
python -V

Keep using Python 2.7, I had lots of problems with 3.5

Change default version
https://linuxconfig.org/how-to-change-from-default-to-alternative-python-version-on-debian-linux

Install PIP

sudo apt-get install python3-pip

Upgrade setuptools

sudo pip install --upgrade setuptools

Install in a virtualenv

sudo pip install virtualenv

Create the virtualenv

virtualenv ~/flexget/

Install FlexGet in the virtualenv

cd ~/flexget/
bin/pip install flexget

Setup flexget to be accessible everywhere

sudo update-alternatives --install /usr/local/bin/flexget flexget /home/pi/elements/flexget/bin/flexget 1

Create your config.yml configuration file in ~/.flexget

As example of config.yml: https://github.com/unQualsevol/rpi_flexget_conf

As we want to use transmission we need to install transmissionrpc

pip install --upgrade transmissionrpc

## Samba to access to edit files

https://www.raspberrypi.org/magpi/samba-file-server/

Install samba

sudo apt install samba samba-common-bin

Edit file

sudo nano /etc/samba/smb.conf

append:


[name]
	comment = Elements
	path = path_to_shared_folder
  Browseable = yes
  Writeable = Yes
  only guest = no
  create mask = 0777
  directory mask = 0777
  Public = yes

Restart samba

sudo /etc/init.d/samba restart

