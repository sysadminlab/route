system/clock/print
system/clock set time-zone-name=Europe/Samara

ip address/ add address=192.168.1.1 interface=ether4 network=192.168.1.0

ip service/disable ftp,telnet,www,api
ip service/ set winbox,ssh,www-ssl,api-ssl  address=192.168.1.1

interface/list/add name=LAN
interface/list/member/add list=LAN interface=ether4
ip neighbor/discovery-settings/set discover-interface-list=LAN

interface/list/add name=WAN
interface/list/member/add list=WAN interface=ether1
interface/list/member/add list=WAN interface=ether2
interface/list/member/add list=WAN interface=ether3
