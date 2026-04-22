
Sometimes the virtual network adapter in UTM gets "stuck" after a VPN disconnect

sudo ip link set eth0 down 
sudo ip link set eth0 up

sudo nmcli device reapply eth0

or hard reset for interface

sudo nmcli networking off && sudo nmcli networking on