## Driver install on Kali Linux
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
