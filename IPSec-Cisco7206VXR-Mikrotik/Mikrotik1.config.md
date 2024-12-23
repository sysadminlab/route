/ip ipsec profile
add dh-group=modp2048 enc-algorithm=3des hash-algorithm=sha256 name=to100_2

/ip ipsec peer
add address=10.100.0.2/32 name=toCisco profile=to100_2

/ip ipsec identity
add peer=toCisco

/ip ipsec proposal
add auth-algorithms=sha256 enc-algorithms=3des name=to100_2 pfs-group=modp2048

/ip ipsec policy
add dst-address=192.168.2.0/24 peer=toCisco proposal=to100_2 src-address=192.168.1.0/24 tunnel=yes

/ip address
add address=10.100.0.1/30 comment=Provider1 interface=ether1 network=10.100.0.0
add address=10.200.0.1/30 comment=Provider2 interface=ether2 network=10.200.0.0
add address=192.168.1.1/24 comment=Lan1 interface=ether4 network=192.168.1.0

/ip firewall filter
add action=accept chain=forward connection-state=established,related dst-address=192.168.2.0/24 src-address=192.168.1.0/24
add action=accept chain=forward connection-state=established,related dst-address=192.168.1.0/24 src-address=192.168.2.0/24

/ip firewall nat
add action=accept chain=srcnat dst-address=192.168.2.0/24 src-address=192.168.1.0/24

/ip firewall raw
add action=notrack chain=prerouting dst-address=192.168.2.0/24 src-address=192.168.1.0/24
add action=notrack chain=prerouting dst-address=192.168.1.0/24 src-address=192.168.2.0/24

/ip route
add dst-address=192.168.2.0/24 gateway=10.100.0.2
