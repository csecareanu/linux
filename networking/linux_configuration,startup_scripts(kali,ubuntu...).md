## Content

### All distributions
* [How to reconfigure your system every time it boots up](#reconfigure_system_at_boot_up)

### Kali
* [Add persistence to a live Kali Linux USB drive on Mac](#add_persistence_live_kali)
* [Driver install on Kali Linux](#driver_install)

### Ubuntu

## How to reconfigure your system every time it boots up <a name="reconfigure_system_at_boot_up"/>
There are several ways to reconfigure your system every time it boots up. 
* First, remember that every Linux distribution has its own set of init scripts called by init (8). The most common configurations include the **/etc/rc.d/** directory, or the alternative, **/etc/init.d/**.
* The second is the **rc.local** script, usually included in all distributions (e.g in /etc/rc.local on Ubuntu), which is known as the place where user configuration setups are done. This script is executed at the end of each multiuser runlevel.

## Add persistence to a live Kali Linux USB drive on Mac <a name="add_persistence_live_kali"/>

Assuming you have a functioning Live USB, then boot into that. (On a Mac, that means holding down Option on startup.) On Kali’s boot screen, then choose the default top choice to load.

1. Open up terminal/console and use `fdisk -l` to make sure you know which is your live USB; it will be something like `/dev/sdb`
2. in terminal, type `fdisk /dev/sdb` (use YOUR correct path here)
3. in fdisk, now type `n` for new partition
4. then select `p` for primary, and accept the next three defaults for partition number, first sector and last sector
5. finally, in fdisk, type `w` to save your changes and exit the program
6. now back at the main prompt, type `reboot now` (not 100% sure this last step is required)

You have now created a third partition on your USB with the space not required for Kali. Next up is making that partition encrypted, formatted, and persistent.

1. Startup that Kali Live USB again and get to the terminal
2. type `cryptsetup -vy luksFormat /dev/sdb3` (again this should be the path to your newly created USB partition). The parameters are v for verbose and y for verify-passphrase.
3. It will ask ‘are you sure?’ which requires a `YES` in caps. You will then be prompted for a passphrase. This will be required whenever you first open that partition, so **don’t forget it!!**
4. After entering and re-entering your passphrase, cryptsetup will encrypt the partition.
5. Now open the partition by typing `cryptsetup luksOpen /dev/sdb3 my_usb` and entering your passphrase
6. Create the filesystem with `mkfs.ext3 -L persistence /dev/mapper/my_usb` and expect a bit of time for its creation
7. When the prompt returns, label the partition with `e2label /dev/mapper/my_usb persistence`

Now let’s create the mount point and config:
1. `mkdir -p /mnt/my_usb`
2. `mount /dev/mapper/my_usb /mnt/my_usb`
3. `echo “/ union” > /mnt/my_usb/persistence.conf`

Then we’ll unmount the partition and close the encrypted channel
1. `umount /dev/mapper/my_usb`
2. `cryptsetup luksClose /dev/mapper/my_usb`

Finally, type `reboot now`
You are now ready to reboot into Kali Live with Encrypted Persistence!\
So select that option at the load screen.\
During boot up, you will be prompted to unlock your partition.

## Driver install on Kali Linux <a name="driver_install"/>
```
# check dirver Vendor (lsusb if the netowrk card is on usb or lspci if it is an internal one)
lsusb
lspci

apt-get update
apt-get install synaptic -y
```

Open Synaptic Package Manager application
* Press `search` button
* In the search field type driver vendor name (broadcom)
* Right click on `broadcom-sta-dkms and` choose `Mark for Instalation`
* Press `Mark` button


Open Synaptic Package Manager application
* Press `search` button
* In the search field type: `linux-image`
* Make sure that linux-headers-4.15.0-kali2-amd64 has the same version with linux-image-4.15.0-kali2-amd64 (4.15)\
Remove the wrong package (linux-image-4.14.0-kali3-amd64) and install the right one

Restart the computer and then check with `iwconfig` if the driver is loaded.
