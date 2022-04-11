## Disable Firewall
### CentOS
Starting with CentOS 7, FirewallD replaces iptables as the default firewall management tool.

#### View the current status of the FirewallD service:
```
sudo firewall-cmd --state
```

#### Disable Firewall
You can temporarily stop the FirewallD service with the following command:
```
sudo systemctl stop firewalld
```

To permanently disable the firewall:
```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

Mask the FirewallD service which will prevent the firewall from being started by other services:
```
sudo systemctl mask --now firewalld
```
