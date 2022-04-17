
## Setup
* check current time zone
* Settings -> Privacy -> Screen Lock -> disable
* Settings -> Power
* Settings -> Appearance (icon size, color theme)
* Settings -> Users -> Automatic Login
* Disable firewall

## Tools to install
* sudo apt-get install net-tools
* sudo apt install vim
* sudo apt install tmux
* sudo apt install inetutils-traceroute
* sudo apt install terminator
* sudo apt install htop
* sudo apt install iperf3
* sudo apt install sysstat
* atom
  * sudo apt update
  * sudo apt install software-properties-common apt-transport-https wget
  * wget -q https://packagecloud.io/AtomEditor/atom/gpgkey -O- | sudo apt-key add -
  * sudo add-apt-repository "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main"
  * sudo apt install atom
