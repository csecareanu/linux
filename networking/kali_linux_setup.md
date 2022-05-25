## Content
* [Add persistence to a live Kali Linux USB drive on Mac](#add_persistence_live_kali)
* [Driver install on Kali Linux](#driver_install)

## Add persistence to a live Kali Linux USB drive on Mac <a name="add_persistence_live_kali"/>

Assuming you have a functioning Live USB, then boot into that. (On a Mac, that means holding down Option on startup.) On Kali’s boot screen, then choose the default top choice to load.

Open up terminal/console and use fdisk -l to make sure you know which is your live USB; it will be something like /dev/sdb
in terminal, type fdisk /dev/sdb (use YOUR correct path here)
in fdisk, now type n for new partition
then select p for primary, and accept the next three defaults for partition number, first sector and last sector
finally, in fdisk, type w to save your changes and exit the program
now back at the main prompt, type reboot now (not 100% sure this last step is required)
You have now created a third partition on your USB with the space not required for Kali. Next up is making that partition encrypted, formatted, and persistent.

Startup that Kali Live USB again and get to the terminal
type cryptsetup -vy luksFormat /dev/sdb3 (again this should be the path to your newly created USB partition). The parameters are v for verbose and y for verify-passphrase.
It will ask ‘are you sure?’ which requires a YES in caps. You will then be prompted for a passphrase. This will be required whenever you first open that partition, so don’t forget it!!
After entering and re-entering your passphrase, cryptsetup will encrypt the partition.
Now open the partition by typing cryptsetup luksOpen /dev/sdb3 my_usb and entering your passphrase
Create the filesystem with mkfs.ext3 -L persistence /dev/mapper/my_usb and expect a bit of time for its creation
When the prompt returns, label the partition with e2label /dev/mapper/my_usb persistence
Now let’s create the mount point and config:
mkdir -p /mnt/my_usb
mount /dev/mapper/my_usb /mnt/my_usb
echo “/ union” > /mnt/my_usb/persistence.conf
Then we’ll unmount the partition and close the encrypted channel
umount /dev/mapper/my_usb
cryptsetup luksClose /dev/mapper/my_usb
Finally, type reboot now
You are now ready to reboot into Kali Live with Encrypted Persistence!
So select that option at the load screen.
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
* Press search button
* In the search field type driver vendor name (broadcom)
* Right click on broadcom-sta-dkms and choose Mark for Instalation
* Press Mark button


Open Synaptic Package Manager application
* Press search button
* In the search field type: `linux-image`
* Make sure that linux-headers-4.15.0-kali2-amd64 has the same version with linux-image-4.15.0-kali2-amd64 (4.15)\
Remove the wrong package (linux-image-4.14.0-kali3-amd64) and install the right one

Restart the computer and then check with `iwconfig` if the driver is loaded.
