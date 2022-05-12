## The packet flow of a wireless client associating to an access point
<img src="./img/wifi_monitoring/wireless_packet_flow.png" width="300px"/>

* EAPOL data packets are used for the 4 way handshake
* QoS Data packets contains encrypted data

## The Probe request, Authentication, Association asnd Handshake packets:
<img src="./img/wifi_monitoring/wireless_auth_packets.png" height="250px"/>

## Wireshark filters
* To fliter the packets form the client: wlan.addr == <mac addr>
* Filer for protocols: bootp icmp

## How to decrypt IEE 802.11 traffic
  
Wireshark -> Preferences -> Protocols -> IEE 802.11\
Edit Decryption keys and add:
* Key type: wpa-pwd
* Key: <preshared key>:<ssid>



## Caputre WiFi packests using Kali linux:
 
Check the WiFi device driver
```
$ lsusb
Bus 002 Device 003: ID 0cf3:9271 Qualcomm Atheros Communications AR9271 802.11n

$ lspci
```
  
```
$ sudo airmon-ng
PHY     Interface       Driver          Chipset
phy0    wlan0           ath9k_htc       Qualcomm Atheros Communications AR9271 802.11n
```

```
$ sudo airmon-ng check
Found 2 processes that could cause trouble.
Kill them using 'airmon-ng check kill' before putting
the card in monitor mode, they will interfere by changing channels
and sometimes putting the interface back in managed mode

    PID Name
    549 NetworkManager
    650 wpa_supplicant
```

```
$ sudo airmon-ng check kill

Killing these processes:

    PID Name
    650 wpa_supplicant
```

```
$ sudo airmon-ng check
```
  
```
# sudo airmon-ng start <interface name> <channel id>
$ sudo airmon-ng start wlan0 10
PHY     Interface       Driver          Chipset

phy0    wlan0           ath9k_htc       Qualcomm Atheros Communications AR9271 802.11n
                (mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
                (mac80211 station mode vif disabled for [phy0]wlan0)
```

Confirm the card is in monitor mode:
```
$ iwconfig
```

To capture packets without wireshark
```
$ airodump-ng --channel 11 --bssid 90:99:90:99:90:99 wlan0mon
```

To stop the monitor mode:
```
$ sudo airmon-ng stop wlan0mon
```
