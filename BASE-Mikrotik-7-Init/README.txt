system/clock/print
system/clock set time-zone-name=Europe/Samara

ip dhcp-client/disable ether1
ip address/ add address=192.168.1.1/24 interface=ether4 network=192.168.1.0

ip service/disable ftp,telnet,www,api
ip service/ set winbox,ssh,www-ssl,api-ssl  address=192.168.1.1

interface/list/add name=LAN
interface/list/member/add list=LAN interface=ether4
ip neighbor/discovery-settings/set discover-interface-list=LAN

interface/list/add name=WAN
interface/list/member/add list=WAN interface=ether1
interface/list/member/add list=WAN interface=ether2
interface/list/member/add list=WAN interface=ether3

/ip pool add name=lan-pool ranges=192.168.0.10-192.168.0.100
/ip dhcp-server add address-pool=lan-pool interface=ether2 name=dhcp-lan

/ip dhcp-server network add address=192.168.0.0/24 dns-server=192.168.0.1 gateway=192.168.0.1 netmask=24
/ip dns set allow-remote-requests=yes servers=8.8.8.8

/ip firewall nat add action=masquerade chain=srcnat src-address=192.168.0.0/24

ip firewall/address-list/add address=10.10.0.2 list=IPLIST_BRANCH
ip firewall/address-list/add address=10.20.0.2 list=IPLIST_BRANCH
ip firewall/address-list/add address=10.30.0.2 list=IPLIST_BRANCH

ip firewall/filter/add in-interface-list=WAN chain=input protocol=ipsec-esp address-list=IPLIST_BRANCH action=accept
ip firewall/filter/add in-interface-list=WAN chain=input action=return
ip firewall/filter/add in-interface-list=WAN chain=forward action=return

