## Content
* [The packet flow of a wireless client associating to an access point](#packet_flow)
* [The Probe request, Authentication, Association and Handshake packets](#connect_packets)
* [Wireshark filters](#wireshark_filters)
* [Beacon packet](#beacon_packet_info)
* [How to decrypt IEE 802.11 traffic](#decript_traffic)
* [Caputre WiFi packests using Kali linux](#capture_using_kali_linux)
* [Caputre WiFi packests using MAC OS](#capture_using_macos)

## The packet flow of a wireless client associating to an access point <a name="packet_flow"/>
<img src="./img/wifi_monitoring/wireless_packet_flow.png" width="300px"/>

* EAPOL data packets are used for the 4 way handshake
* QoS Data packets contains encrypted data

## The Probe request, Authentication, Association and Handshake packets <a name="connect_packets"/>
<img src="./img/wifi_monitoring/wireless_auth_packets.png" height="250px"/>

## Wireshark filters <a name="wireshark_filters"/>
* To fliter the packets form the client: wlan.addr == _mac addr_
* Filer for protocols: bootp icmp

## Beacon packet info <a name="beacon_packet_info"/>

```
# Packet type is beacon:
> IEEE 802.11 Beacon frame, Flags: ........C

# Data rate is 6 Mb/s, the channel number is 6
> Radiotap Header v0, Length 56
Data Rate: 6.0 Mb/s
Channel frequency: 2437 [BG 6]

# SSID
> IEEE 802.11 Wireless Management
   > Tagged parameters (270 bytes)
      > Tag: SSID parameter set: DIRECT-DIRECT-DIRECT-DIRECT-DIRE
         > SSID: DIRECT-DIRECT-DIRECT-DIRECT-DIRE
```
  
## How to decrypt IEE 802.11 traffic <a name="decript_traffic"/>

The decryption is necessary for:
* the data filed from QoS packets which is encrypted
* also the ICMP packets are encrypted
  
Wireshark -> Preferences -> Protocols -> IEE 802.11
* Edit Decryption keys and add:
   * Key type: wpa-pwd
   * Key: _preshared key_:_ssid_
* Check radion button
   *  Ignore the Protection bit: Yes - with IV



## Caputre WiFi packests using Kali linux <a name="capture_using_kali_linux"/>
 
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
lo        no wireless extensions.

eth0      no wireless extensions.

wlan0mon  IEEE 802.11  Mode:Monitor  Frequency:2.457 GHz  Tx-Power=20 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:off
```

To capture packets without wireshark
```
$ airodump-ng --channel 11 --bssid 76:6f:f7:88:90:65 --output-format pcap -w my_pcap wlan0mon
```

To stop the monitor mode:
```
$ sudo airmon-ng stop wlan0mon
```
  
## Caputre WiFi packests using MAC OS <a name="capture_using_macos"/>
1. Option+Click on the Wi-Fi menu item in the OS X menu bar.
2. Choose “Open Wireless Diagnostics” from the list to open the wi-fi utility.
3. Ignore the splash screen and pull down the “Window” menu, choose “Sniffer” from the list of options in the Wireless Diagnostics menu.
4. Select the Wi-Fi Channel and channel Width to sniff and capture packets for and click “Start”.
5. When satisfied with the length of the packet capture, or when sufficient network traffic has been sniffed, click on “Stop” to end the packet trace and to save the captured packet file to the Desktop.
6. In the Finder, navigate to /var/tmp/ and look for the resulting .pcap file.
