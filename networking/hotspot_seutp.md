## Check if Linux detects your WiFi device
In terminal type `lspci`.\
If you use usb wireless adapter type command `lsusb`.
 
## Install end then stop hostapd dnsmasq services

```
sudo apt-get install hostapd dnsmasq

sudo service hostapd stop
sudo service dnsmasq stop
sudo update-rc.d hostapd disable
sudo update-rc.d dnsmasq disable
```


## Edit /etc/dnsmasq.conf
```bash
# Bind to only one interface
bind-interfaces
# Choose interface for binding
interface=wlan0
# Specify range of IP addresses for DHCP leasses
dhcp-range=192.168.30.2,192.168.30.10
```

## Edit /etc/hostapd.conf
```bash
# MAC: 54:6f:f4:05:12:de

# Set interface
interface=wlan0
# Set driver to
driver=nl80211
# Set your desired ssid(Wi-Fi name)
ssid=AP 91414_5Ghz
# Set the access point hardware mode to 802.11g
# hw_mode=a if you want to set the 5 GHz frequency
hw_mode=g
# Select WIFI channel
channel=6
# Ensure to enable only WPA2
wpa=2
wpa_passphrase=5tgghjuk
```

## Now create anywhere you want a file named it hotspot.sh. 

```bash
#!/bin/bash
# Start
# Configure IP address for WLAN
sudo ifconfig wlan0 192.168.150.1
# Start DHCP/DNS server
sudo service dnsmasq restart
# Enable routing
sudo sysctl net.ipv4.ip_forward=1
# Enable NAT
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# Run access point daemon
sudo hostapd /etc/hostapd.conf
# Stop
# Disable NAT
sudo iptables -D POSTROUTING -t nat -o eth0 -j MASQUERADE
# Disable routing
sudo sysctl net.ipv4.ip_forward=0
# Disable DHCP/DNS server
sudo service dnsmasq stop
sudo service hostapd stop
# You will probably need to change eth0 to interface which refers to your wired connection.
```

## Now you can start your hotspot by starting script. Just run it...
```bash
root@kali:~/Desktop# ./hotspot.sh
```
