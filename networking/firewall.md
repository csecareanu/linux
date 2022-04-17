## Disable Firewall
* [CentOS](#centos)
* [Ubuntu](#ubuntu)

### CentOS  <a name="centos"/>
Starting with CentOS 7, FirewallD replaces iptables as the default firewall management tool.

#### View the current status of the FirewallD service:
```
sudo firewall-cmd --state
# or
systemctl status firewalld
```

#### Disable Firewall
You can temporarily stop the FirewallD service with the following command:
```bash
sudo systemctl stop firewalld
```

To permanently disable the firewall:
```bash
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

Mask the FirewallD service which will prevent the firewall from being started by other services:
```bash
# Check if the service is already masked
systemctl list-unit-files | grep masked
# or
systemctl status firewalld

# Mask the service
sudo systemctl mask --now firewalld
```

To unmask the service:
```bash
systemctl unmask firewalld
```

### Ubuntu  <a name="ubuntu"/>

 Stop/Sart the service, but it will be restarted next tine on os restart
```bash
sudo ufw disable
```

Disable the service that starts ufw during the boot process of the computer
```bash
sudo systemctl disable ufw.service
```

Completely remove UFW from your system: that will remove the systemd service and the program
```bash
sudo apt purge ufw
```

Verify UFW Status:
```bash
sudo ufw status
```

Configure the firewall (UFW Essentials: Common Firewall Rules and Commands): https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands
