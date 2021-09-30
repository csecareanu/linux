# ip COMMAND CHEAT SHEET

## Content <a name="content"/>
[NET-TOOLS COMMANDS](#NET_TOOLS_COMMANDS)\
[IP QUERIES](#IP_QUERIES)\
[MULTICAST ADDRESSING](#MULTICAST_ADDRESSING)\
[MODIFYING ADDRESS AND LINK PROPERTIES](#MODIF_ADDR_AND_LINK_PROP)\
[ADJUSTING AND VIEWING ROUTES](#UPDATE_VIEW_ROUTES)\
[MANAGING THE ARP TABLE](#MANAGE_ARP_TABLE)\
[USEFUL NETWORKING COMMANDS (NOT NECESSARILY PROVIDED FROM IPROUTE)](#USEFULL_NETWORKIG_COMMANDS)\
[COMPARING NET-TOOLS VS. IPROUTE PACKAGE COMMANDS](#COMPARING_NET-TOOLS_VS_IPROUTE)


## NET-TOOLS COMMANDS <a name="NET_TOOLS_COMMANDS">
### arp
The `arp` command allows users to manipulate the neighbor cache or ARP table.
* `arp` Lists the current contents of the ARP cache.
  You should notice the following columns: 
  * Address: on most systems, you see IPv4 address listed (192.168.0.1 or the like)
  * HWtype: is specified as `ether` (Ethernet)
  * HWaddress:  is the translated MAC address
  * Flags: indicates if the address has been learned, manually set by the user, published, or is incomplete
  * Mask:
  * Iface: is simply the name of the interface that is listed
* `arp -i bondX` Display all arp entries entries for a specific interface.
* `arp -a 192.168.0.1` To see all arp entries for a particular address
* `arp -s 192.168.0.1 -i ethX 51:53:00:17:34:09` To add an entry (permanently) to the cache, use the -s option. You need to specify the IP and MAC addresses, as well as the interface.
* `arp -d 192.168.0.1` To remove an entry from the arp cache, use the -d flag, followed by the IP address you wish to remove

## IP QUERIES <a name="IP_QUERIES"/>

## IP QUERIES <a name="IP_QUERIES"/>
### addr
Display IP Addresses and property information (abbreviation of address)
* `ip addr` Show information for all addresses
* `ip addr show dev em1` Display information only for device em1

### link
Manage and display the state of all network interfaces
* `ip link` Show information for all interfaces
* `ip link show dev em1` Display information only for device em1
* `ip -s link` Display interface statistics

[top](#content)
## MULTICAST ADDRESSING <a name="MULTICAST_ADDRESSING"/>

[top](#content)
## MODIFYING ADDRESS AND LINK PROPERTIES <a name="MODIF_ADDR_AND_LINK_PROP"/>

[top](#content)
## ADJUSTING AND VIEWING ROUTES <a name="UPDATE_VIEW_ROUTES"/>

[top](#content)
## MANAGING THE ARP TABLE <a name="MANAGE_ARP_TABLE"/>

[top](#content)
## USEFUL NETWORKING COMMANDS (NOT NECESSARILY PROVIDED FROM IPROUTE) <a name="USEFULL_NETWORKIG_COMMANDS"/>

[top](#content)
## COMPARING NET-TOOLS VS. IPROUTE PACKAGE COMMANDS <a name="COMPARING_NET-TOOLS_VS_IPROUTE"/>

[top](#content)
