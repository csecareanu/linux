* Display Available Interfaces `tcpdump -D`
* Capture and Save Packets in a File: `tcpdump -w 0001.pcap -i eth0`
* Read Captured Packets File: `tcpdump -r 0001.pcap`
* Capture Packets from Specific Interface: `tcpdump -i eth0`
* Capture Only N Number of Packets: `tcpdump -c 5 -i eth0`
* Print Captured Packets in ASCII: `tcpdump -A -i eth0`
* Display Captured Packets in HEX and ASCII (capture the data of each packet, including its link level header in HEX and ASCII format): `tcpdump -XX -i eth0`
* Capture IP Address Packets: `tcpdump -n -i eth0`
* Capture only TCP Packets: `tcpdump -i eth0 tcp`
