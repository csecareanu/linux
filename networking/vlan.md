# VLAN Support in Linux

The purpose of VLAN (Virtual LAN) is to build LANs from individual ports instead of entire switches. 

When you want to extend this concept across multiple switches, things become more complicated. The switches need a standard way to cooperate and keep track of which traffic belongs to which VLAN.  The purpose is still the same—build LANs from individual ports instead of entire switches, even if the ports are spread across multiple switches and even multiple geographic locations.

The standard in use today is 802.1Q.  802.1Q provides a simple way for multiple VLAN switches to cooperate by attaching VLAN-specific data directly to the headers of individual Ethernet packets.\
Linux VLANs are based on 802.1Q, and almost any switch that advertises “VLAN” will support this standard.

A switch won't send packets to ports where it knows the destination MAC can't be found. A VLAN switch adds another condition on top of this. It won't send packets to ports (the “egress port” or sink) that aren't “members” of the VLAN to which the packet belongs. This is based on the VLAN ID (VID) of the packet, which is a number between 1 and 4096.

_The ingress port is the incoming port. The egress port is the exiting port._

If a packet doesn't already have a VID, it is assigned one based on the port on which it arrived (the “ingress port” or source). This is the Primary VID (PVID) of the port. Each switch port can be a member of multiple VLANs, one of which must be configured as its PVID.

The VID is stored in an extra 4-byte header that is added to the packet called the Tag.\
Only VLAN devices know what to do with Tagged packets; normal Ethernet devices don't expect to see them. Unless the packet is being sent to another VLAN switch, the Tag needs to be removed before it is sent. This Untagging is done after the switch determines the egress port.

In the case of a VLAN with only a single switch, no Tagged packets should be sent or received.\
However, it's still useful to think of the Tagging and Untagging as occurring:
* Packet arrives and is Tagged according to the PVID of the ingress port.
* Egress port is determined based on the VID in the Tag.
* Packet is Untagged and sent.

All packets start out as Untagged when they enter the network, and they also should always end as Untagged when they leave the network and arrive at their destination. Along their journey, if they cross a VLAN network, they will be Tagged with a VID, switched according to this VID by one or more VLAN switches, and then finally Untagged by the last VLAN switch.

There are three things you need to configure for each port of each switch:
*	Member VLANs (list of VIDs).
*	PVID (must be one of the member VLANs).
*	Whether packets should be left Tagged or Untagged when sent (egress).

Think of each Ethernet interface in your system as a one-port switch. An Ethernet interface already performs the same basic functions as a switch—forwarding packets, maintaining an ARP cache and so on—but on a single port without the need or capability to decide to which other port(s) a packet should be sent.

Linux's bridging code elegantly plugs in to and extends the existing functionality by letting you define bridges as virtual Ethernet interfaces that bundle one or more regular Ethernet interfaces. Each interface within the bridge is a port. In operation, this is exactly like ports of a switch.
