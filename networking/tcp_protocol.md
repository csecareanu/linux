## Content
* [The three-way handshake](#three-way_handshake)
* [TCP keepalive](#keepalive)
  *  [Checking for dead peers](#checking_for_dead_peers)
  *  [Preventing disconnection due to network inactivity](#prev_disconn_due_to_innactivity)
  *  [Configuring the kernel for TCP keepalive parameters](#conf_kernel_keepalive)
  *  [Making changes persistent to reboot](#make_changes_persistent_to_reboot)

## The three-way handshake <a name="three-way_handshake"/>
The initial three-way handshake, with one SYN segment from A to B, the SYN/ACK back from B to A, and the final ACK from A to B. At this time, we're in a stable status: connection is established.
```
    _____                                                     _____
   |     |                                                   |     |
   |  A  |                                                   |  B  |
   |_____|                                                   |_____|
      ^                                                         ^
      |--->--->--->-------------- SYN -------------->--->--->---|
      |---<---<---<------------ SYN/ACK ------------<---<---<---|
      |--->--->--->-------------- ACK -------------->--->--->---|
      |                                                         |
```

## TCP keepalive <a name="keepalive"/>
Determine whether the connection is still up and running or if it has broken.

When the keepalive timer reaches zero, you send your peer a keepalive probe packet with no data in it and the ACK flag turned on.\
On the other hand, you will receive a reply from the remote host (which doesn't need to support keepalive at all, just TCP/IP), with no data and the ACK set.\
TCP permits you to handle a stream, not packets, and so a zero-length data packet is not dangerous for the user program.
  

### Checking for dead peers <a name="checking_for_dead_peers"/>
Keepalive can be used to advise you when your peer dies before it is able to notify you. 
This could happen for several reasons, like:
  * kernel panic or a brutal termination of the process handling that peer
  * the peer is still alive but the network channel between it and you has gone down.
  
Withoug keepalive mechanism, the connection is detected as broken only when one peer tries to send data to the ohter peer.\
For example lets assume that the peer B has reset suddenly and the peer A was not notified.\
If the connection is broken and if A tries to send data to B over the dead connection, B will reply with an RST packet, causing A to close the connection.
```
    _____                                                     _____
   |     |                                                   |     |
   |  A  |                                                   |  B  |
   |_____|                                                   |_____|
      ^                                                         ^
      |--->--->--->-------------- SYN -------------->--->--->---|
      |---<---<---<------------ SYN/ACK ------------<---<---<---|
      |--->--->--->-------------- ACK -------------->--->--->---|
      |                                                         |
      |                                       system crash ---> X
      |
      |                                     system restart ---> ^
      |                                                         |
      |--->--->--->-------------- PSH -------------->--->--->---|
      |---<---<---<-------------- RST --------------<---<---<---|
      |                                                         |
```

### Preventing disconnection due to network inactivity <a name="prev_disconn_due_to_innactivity"/>
The other useful goal of keepalive is to prevent inactivity from disconnecting the channel. It's a very common issue, when you are behind a NAT proxy or a firewall, to be disconnected without a reason (proxies and firewalls keep track of all connections that pass through them; because of the physical limits of these machines, they can only keep a finite number of connections in their memory)

```
    _____           _____                                     _____
   |     |         |     |                                   |     |
   |  A  |         | NAT |                                   |  B  |
   |_____|         |_____|                                   |_____|
      ^               ^                                         ^
      |--->--->--->---|----------- SYN ------------->--->--->---|
      |---<---<---<---|--------- SYN/ACK -----------<---<---<---|
      |--->--->--->---|----------- ACK ------------->--->--->---|
      |               |                                         |
      |               | <--- connection deleted from table      |
      |               |                                         |
      |--->- PSH ->---| <--- invalid connection                 |
      |               |                                         |

```

### Configuring the kernel for TCP keepalive parameters (accessing kernel variables) <a name="conf_kernel_keepalive"/>
There are two ways to configure keepalive parameters inside the kernel via userspace commands:
* *procfs* interface
  * This interface requires both *sysctl* and *procfs* to be built into the kernel, and procfs mounted somewhere in the filesystem (usually on /proc)
  ```
  # cat /proc/sys/net/ipv4/tcp_keepalive_time
  7200
  # cat /proc/sys/net/ipv4/tcp_keepalive_intvl
  75
  # cat /proc/sys/net/ipv4/tcp_keepalive_probes
  9
  ```
  * The first two parameters are expressed in seconds, and the last is the pure number. This means that the keepalive routines wait for two hours (7200 secs) before sending the first keepalive probe, and then resend it every 75 seconds. If no ACK response is received for nine consecutive times, the connection is marked as broken.
  * To modify a value: `# echo 600 > /proc/sys/net/ipv4/tcp_keepalive_time`
  * You can access the interface through the sysctl(8) **tool**, specifying what you want to read or write (Note that sysctl names are very close to procfs paths).
  ```
  # sysctl \
  > net.ipv4.tcp_keepalive_time \
  > net.ipv4.tcp_keepalive_intvl \
  > net.ipv4.tcp_keepalive_probes
  net.ipv4.tcp_keepalive_time = 7200
  net.ipv4.tcp_keepalive_intvl = 75
  net.ipv4.tcp_keepalive_probes = 9
  ```
  * Write is performed using the -w switch of sysctl (8):
  ```
  # sysctl -w \
  > net.ipv4.tcp_keepalive_time=600 \
  > net.ipv4.tcp_keepalive_intvl=60 \
  > net.ipv4.tcp_keepalive_probes=20
  net.ipv4.tcp_keepalive_time = 600
  net.ipv4.tcp_keepalive_intvl = 60
  net.ipv4.tcp_keepalive_probes = 20
  ```
* *sysctl* interface
  * There is another way to access kernel variables: sysctl(2 ) **syscall**. It can be useful when you don't have *procfs* available because the communication with the kernel is performed directly via syscall and not through the procfs subtree.
 
### Making changes persistent to reboot <a name="make_changes_persistent_to_reboot"/>
Detailed info about configuring settings at reboot: [linux_configuration,startup_scripts(kali,ubuntu...).md](./linux_configuration,startup_scripts(kali,ubuntu...).md)

Every Linux distribution has its own set of init scripts called by init (8). The most common configurations include the **/etc/rc.d/** directory, or the alternative, **/etc/init.d/**. In any case, you can set the parameters in any of the startup scripts, because keepalive rereads the values every time its procedures need them. So if you change the value of _tcp_keepalive_intvl_ when the connection is still up, the kernel will use the new value going forward.

There are three spots where the initialization commands should logically be placed: 
* the first is where your network is configured
* the second is the **rc.local** script, usually included in all distributions (/etc/rc.local on ubuntu), which is known as the place where user configuration setups are done. 
* the third place may already exist in your system. Referring back to the sysctl (8) tool, you can see that the -p switch loads settings from the /etc/sysctl.conf configuration file. In many cases your init script already performs the sysctl -p (you can "grep" it in the configuration directory for confirmation), and so you just have to add the lines in _/etc/sysctl.conf_ to make them load at every boot. For more information about the syntax of _sysctl.conf(5)_, refer to the manpage.
