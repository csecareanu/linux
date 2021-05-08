

__Viewing the configuration of all interfaces__

* `ifconfig -a` (displays also the inactive interfaces)


__Viewing the configuration of a specific interface__

* `ifconfig eth0`

__Enabling and disabling an interface__

When a network interface is active, it can send and receive data; when it is inactive, it is not able to transmit or receive.
* sudo ifconfig eth1 up
* sudo ifconfig wlan0 down

__Configuring an interface__

ifconfig can only assign a static IP address to a network interface. If you want to assign a dynamic IP address using DHCP, use the `dhclient` command.

* `sudo ifconfig wlan0 69.72.169.1`
* `sudo ifconfig eth1 netmask 255.255.255.0`
* `sudo ifconfig wlan1 broadcast 172.16.25.98`
* In one line: `sudo ifconfig eth0 192.168.2.5 netmask 255.255.255.0 broadcast 192.168.2.7`

* check inteface exists
* 
  ```
  int sock = socket(AF_INET, SOCK_STREAM, 0);
  int res = ioctl(sockfd, SIOCGIFINDEX, interf_name);
  close(sock)
  ```
* retrieveMac
  * `ioctl( sockfd, SIOCGIFHWADDR, &ifr )`
* retrieveIPAddress
  * `ioctl(sockfd, SIOCGIFADDR, &ifr)`
* retrieveNetmask
  * `ioctl(sockfd, SIOCGIFNETMASK, &ifr)`
* retrieveBroadcastAddress
  * `ioctl(sockfd, SIOCGIFBRDADDR, &ifr)`
* retrieveMTU
  * `ioctl(sockfd, SIOCGIFMTU, &ifr)`
 
