# BATMAN_setup
/etc/config/wireless

config wifi-iface 'wmesh'
        option device 'radio0'
        option ifname 'adhoc0'
        option network 'batnet'
        option mode 'adhoc'
        option ssid 'mesh'
        option 'mcast_rate' '18000'
        option bssid '02:CA:FE:CA:CA:40'
        
/etc/config/network
config interface 'batnet'
        option mtu '1532'
        option proto 'batadv'
        option mesh 'bat0'
        
/etc/init.d/mesh        
#!/bin/sh /etc/rc.common
START=99
start(){
batctl if add eth0
ip link set up dev eth0
ip link set mtu 1532 dev wlan0
iwconfig wlan0 mode ad-hoc essid batman_default ap 02:12:34:56:78:9A channel 1
batctl if add wlan0
ip link set up dev wlan0
ip link set up dev bat0
ip addr add 192.168.123.5/24 dev bat0
}
stop(){}

/etc/rc.d/
ln -s /etc/init.d/mesh /etc/rc.d/S99mesh
