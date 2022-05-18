## Content
* [TCP keepalive](#keepalive)

## TCP keepalive <a name="keepalive">
Determine whether the connection is still up and running or if it has broken.

When the keepalive timer reaches zero, you send your peer a keepalive probe packet with no data in it and the ACK flag turned on.\
On the other hand, you will receive a reply from the remote host (which doesn't need to support keepalive at all, just TCP/IP), with no data and the ACK set.
