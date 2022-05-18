## Content
* [The three-way handshake](#three-way_handshake)
* [TCP keepalive](#keepalive)
  *  [Checking for dead peers](#checking_for_dead_peers)
  *  [Preventing disconnection due to network inactivity](#prev_disconn_due_to_innactivity)

## The three-way handshake <a name="three-way_handshake">
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

## TCP keepalive <a name="keepalive">
Determine whether the connection is still up and running or if it has broken.

When the keepalive timer reaches zero, you send your peer a keepalive probe packet with no data in it and the ACK flag turned on.\
On the other hand, you will receive a reply from the remote host (which doesn't need to support keepalive at all, just TCP/IP), with no data and the ACK set.\
TCP permits you to handle a stream, not packets, and so a zero-length data packet is not dangerous for the user program.
  

### Checking for dead peers <a name="checking_for_dead_peers">
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
  
### Preventing disconnection due to network inactivity <a name="prev_disconn_due_to_innactivity">
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