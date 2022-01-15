
**Delete interface if exists**
```bash
ip -br addr
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

# Making it permanent (load it at boot time)
sudo su -c 'echo "8021q" >> /etc/modules'
```

**Create virtual interface**
```bash
# check if the interface already exists
ip -br addr 

sudo vconfig add ens34 10
# or
sudo ip link add link ens34 name ens34.10 type vlan id 10
```

**Assign an address to the new interface**
```bash
sudo ip addr add 10.0.0.1/24 dev ens34.10
```

**Start the new interface**
```bash
sudo ip link set up ens34.10
```

**To make it permanent load 8021q at boot time and add the following lines to /etc/network/interfaces**
```
auto ens34.10
iface ens34.10 inet static
    address 10.0.0.1
    netmask 255.255.255.0
    vlan-raw-device ens34
```
