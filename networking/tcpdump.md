* **Display** Available **Interfaces** `tcpdump -D`
* Capture and **Save Packets** in a File: `tcpdump -w 0001.pcap -i eth0`
* **Read** Captured Packets **File**: `tcpdump -r 0001.pcap`
* Capture Packets from **Specific Interface**: `tcpdump -i eth0`
* Capture **Only N** Number of **Packets**: `tcpdump -c 5 -i eth0`
* **Print** Captured Packets **in ASCII**: `tcpdump -A -i eth0`
* **Capture the data of each packet**, including its link level header in HEX and ASCII format: 
`tcpdump -XX -i eth0`
* Capture **IP Address** Packets: `tcpdump -n -i eth0`
* Capture **only TCP** Packets: `tcpdump -i eth0 tcp`
* Capture Packet **from Specific Port**: `tcpdump -i eth0 port 22
* Capture Packets **from source IP**: `tcpdump -i eth0 src 192.168.0.2`
* Capture Packets **from destination IP**: `tcpdump -i eth0 dst 50.116.66.139`
