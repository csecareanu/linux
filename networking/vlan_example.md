
**Monitorize kernel logs for all operations**
```bash
dmesg -w
```

**Watch for netlink messages**
```bash
ip monitor all
```

**Delete interface if exists**
```bash
ip -br addr show ens34.10
# or
ip -br link show ens34.10
# or
ifconfig ens34.10

sudo ip link del ens34.10
```

**Load the 8021q module into the kernel**
```bash
# check if the module is available in system and get info about it
modinfo 8021q

# check if the module is already loaded
lsmod | grep 8021q

# add the module
sudo modprobe 8021q

# If you want to make it permanent (load it at boot time)
sudo su -c 'echo "8021q" >> /etc/modules'
```

**Create a virtual interface**
```bash
# check if the interface already exists using one of the two options
ip -br addr
ip -br link


sudo vconfig add ens34 10
# or
sudo ip link add link ens34 name ens34.10 type vlan id 10
```

**Assign an address to the new interface**
```bash
ip addr flush dev ens34.10
sudo ip addr add 10.0.0.1/24 brd 10.0.0.255 dev ens34.10
```

**Start the new interface**
```bash
sudo ip link set up ens34.10
```

**To make it permanent load 8021q at boot time and add the following lines to /etc/network/interfaces (Ubuntu/Debian/Linux Mint)**\
```
auto ens34.10
iface ens34.10 inet static
    address 10.0.0.1
    netmask 255.255.255.0
    vlan-raw-device ens34
```
> For RHEL/CentOS/Fedora the settings are different and are stored in /etc/sysconfig/network-scripts/ifcfg-eth0
