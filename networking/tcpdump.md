* **Display** Available **Interfaces** `tcpdump -D`
* Capture and **Save Packets** in a File: `tcpdump -w 0001.pcap -i eth0`
* **Read** Captured Packets **File**: `tcpdump -r 0001.pcap`
* Capture Packets from **Specific Interface**: `tcpdump -i eth0`; `tcpdump -i any`
* Capture **Only N** Number of **Packets**: `tcpdump -c 5 -i eth0`
* **Print** Captured Packets **in ASCII**: `tcpdump -A -i eth0`
* **Print** Captured Packets **in Hex**: `tcpdump -X icmp -i eth0`
* **Capture the data of each packet**, including its link level header in HEX and ASCII format: 
`tcpdump -XX -i eth0`
* Capture **IP Address** Packets: `tcpdump -n -i eth0`
* Capture traffic of **one protocol**: `tcpdump icmp`
* Capture **only TCP** Packets: `tcpdump -i eth0 tcp`; `icmp`;`http`
* Capture Packet **from Specific Port**: `tcpdump -i eth0 port 22
* Capture Packets **from source IP**: `tcpdump -i eth0 src 192.168.0.2`
* Capture Packets **from destination IP**: `tcpdump -i eth0 dst 50.116.66.139`
* Capture traffic that’s **going to or from 1.1.1.1**: `tcpdump host 1.1.1.1`
* Capture packets going to or from a **particular network or subnet**: `tcpdump net 1.2.3.0/24`
* Capture **all IP6 traffic** using the protocol option: `tcpdump ip6`
* Use a **range of ports**: `tcpdump portrange 21-23`
* If you’re looking for **packets of a particular size** you can use these options: `tcpdump less 32`; `tcpdump greater 64`; `tcpdump <= 128`
* To print the TCP **packets with flags RST and ACK both set**. (i.e. select only the RST and ACK flags in the flags field, and if the result is "RST and ACK both set", match) `tcpdump 'tcp[tcpflags] & (tcp-rst|tcp-ack) == (tcp-rst|tcp-ack)'`
* To print **all IPv4 HTTP packets to and from port 80**, i.e. print only packets that contain data, **not, for example, SYN and FIN packets and ACK-only packets**. (IPv6 is left as an exercise for the reader.): `tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'`
* To print **IP packets longer than 576 bytes** sent through gateway *snup*: `tcpdump 'gateway snup and ip[2:2] > 576'`
* To print **IP broadcast or multicast packets that were not sent via Ethernet broadcast or multicast**: `tcpdump 'ether[0] & 1 = 0 and ip[16] >= 224'`
* To print all **ICMP packets that are not echo requests/replies (i.e., not ping packets)**: `tcpdump 'icmp[icmptype] != icmp-echo and icmp[icmptype] != icmp-echoreply'`

### Advanced

#### Options
Here are ways to tweak how you call tcpdump.
* -n: Don't convert addresses (i.e., host addresses, port numbers, etc.) to names.
* -A: Print each packet (minus its link level header) in ASCII. Handy for capturing web pages.
* -x, -xx: as -X and -XX but only in hex
* -X : Show the packet’s contents in both hex and ASCII.
* -XX : Same as -X, but also shows the ethernet header.
* -D : Show the list of available interfaces
* -l : Line-readable output (for viewing as you save, or sending to other commands)
* -q : Be less verbose (more quiet) with your output.
* -t : Give human-readable timestamp output (don't print a timestamp on each dump line.).
* -tt: Print the timestamp, as seconds since January 1, 1970, 00:00:00, UTC, and fractions of a second since that time, on each dump line.
* -ttt: Print a delta (microsecond or nanosecond resolution depending on the --time-stamp-precision option) between current and previous line on each dump line. The default is microsecond resolution.
* -tttt: Give maximally human-readable timestamp output (Print a timestamp, as hours, minutes, seconds, and fractions of a second since midnight, preceded by the date, on each dump line.)
* -ttttt: Print a delta (microsecond or nanosecond resolution depending on the --time-stamp-precision option) between current and first line on each dump line. The default is microsecond resolution.
* -i eth0 : Listen on the eth0 interface.
* -vv : Verbose output (more v’s gives more output).
* -c : Only get x number of packets and then stop.
* -s : Define the snaplength (size) of the capture in bytes. Use -s0 to get everything, unless you are intentionally capturing less.
* -S : Print absolute sequence numbers.
* -e : Get the ethernet header as well.
* -q : Show less protocol information.
* -E : Decrypt IPSEC traffic by providing an encryption key.
* -C file_size: split the output file (by adding a suffix of k/K, m/M or g/G to the value, the unit can be changed to 1,024 (KiB), 1,048,576 (MiB), or 1,073,741,824 (GiB) respectively.)
* -W filecount: Used in conjunction with the -C option, this will limit the number of files created to the specified number, and begin overwriting files from the beginning, thus creating a 'rotating' buffer.
* -F file: Use file as input for the filter expression. An additional expression given on the command line is ignored.
* -G rotate_seconds: rotates the dump file specified with the -w option every rotate_seconds seconds.
* -z postrotate-command: For example, specifying -z gzip or -z bzip2 will compress each savefile using gzip or bzip2. For example, specifying -z gzip or -z bzip2 will compress each savefile using gzip or bzip2.
* -r file: Read packets from file
* -w file: This output will be buffered.  Use the -U flag to cause packets to be written as soon as they are received.

#### More samples
* Use this combination to see verbose output, with no resolution of hostnames or port numbers, using absolute sequence numbers, and showing human-readable timestamps: `tcpdump -ttnnvvS`
* Let’s find all traffic from 10.5.2.3 going to any host on port 3389: `tcpdump -nnvvS src 10.5.2.3 and dst port 3389`
* Let’s look for all traffic coming from 192.168.x.x and going to the 10.x or 172.16.x.x networks, and we’re showing hex output with no hostname resolution and one level of extra verbosity: `tcpdump -nvX src net 192.168.0.0/16 and dst net 10.0.0.0/8 or 172.16.0.0/16`
* Non ICMP Traffic Going to a Specific IP: `tcpdump dst 192.168.0.2 and src net and not icmp`
* This will show us all traffic from a host that isn’t SSH traffic (assuming default port usage): `tcpdump -vv src mars and not dst port 22`
* Keep in mind that when you’re building complex queries you might have to group your options using single quotes. Single quotes are used in order to tell tcpdump to ignore certain special characters—in this case below the “( )” brackets: `tcpdump 'src 10.0.2.4 and (dst port 3389 or 22)'`

#### Isolate TCP Flags
* Isolate TCP RST flags: `tcpdump 'tcp[13] & 4!=0'`; `tcpdump 'tcp[tcpflags] == tcp-rst'`
* Isolate TCP SYN flags: `tcpdump 'tcp[13] & 2!=0'`; `tcpdump 'tcp[tcpflags] == tcp-syn'`
* Isolate packets that have both the SYN and ACK flags set: `tcpdump 'tcp[13]=18'`
* Isolate TCP URG flags: `tcpdump 'tcp[13] & 32!=0'`; `tcpdump 'tcp[tcpflags] == tcp-urg'`
* Isolate TCP ACK flags: `tcpdump 'tcp[13] & 16!=0'`; `tcpdump 'tcp[tcpflags] == tcp-ack'`
* Isolate TCP PSH flags: `tcpdump 'tcp[13] & 8!=0'`; `tcpdump 'tcp[tcpflags] == tcp-push'`
* Isolate TCP FIN flags: `tcpdump 'tcp[13] & 1!=0'`; `tcpdump 'tcp[tcpflags] == tcp-fin'`

### Everyday Recipe Examples
* Basic command that will get us HTTPS traffic: `tcpdump -nnSX port 443`
* Both SYN and RST Set: `tcpdump 'tcp[13] = 6'`
* Find HTTP User Agents: `tcpdump -vvAls0 | grep 'User-Agent:'`
* Cleartext GET Requests: `tcpdump -vvAls0 | grep 'GET'`
* Find HTTP Host Headers: `tcpdump -vvAls0 | grep 'Host:'`
* Find HTTP Cookies: `tcpdump -vvAls0 | grep 'Set-Cookie|Host:|Cookie:'`
* Find SSH Connections (This one works regardless of what port the connection comes in on, because it’s getting the banner response.): `tcpdump 'tcp[(tcp[12]>>2):4] = 0x5353482D'`
* Find DNS Traffic: `tcpdump -vvAs0 port 53`
* Find FTP Traffic: `tcpdump -vvAs0 port ftp or ftp-data`
* Find NTP Traffic: `tcpdump -vvAs0 port 123`
* Find Cleartext Passwords: `tcpdump port http or port ftp or port smtp or port imap or port pop3 or port telnet -lA | egrep -i -B5 'pass=|pwd=|log=|login=|user=|username=|pw=|passw=|passwd= |password=|pass:|user:|username:|password:|login:|pass |user '`
* Find Traffic With Evil Bit (There’s a bit in the IP header that never gets set by legitimate applications, which we call the “Evil Bit”. Here’s a fun filter to find packets where it’s been toggled): `tcpdump 'ip[6] & 128 != 0'`
