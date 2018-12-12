### Topology 

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/trunked_vlan_lab_1/images/c1.png)

### Router Configureation 

~~~
Below is a very basic configuration example that will provide a NAT gateway for a device with two interfaces.
~~~


### Enter configuration mode:

~~~
vyos@vyos$ configure
~~~

### Configure network interfaces:

~~~
set interfaces ethernet eth0 address 'dhcp'
set interfaces ethernet eth0 description 'OUTSIDE'
set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INSIDE'
~~~


~~~
vyos@vyos:~$ show interfaces 
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface        IP Address                        S/L  Description
---------        ----------                        ---  -----------
eth0             192.168.124.162/24                u/u  OUTSIDE 
eth1             10.10.10.1/24                     u/u  INSIDE 
lo               127.0.0.1/8                       u/u  
                 ::1/128
vyos@vyos:~$ 
~~~


### Enable SSH for remote management:

~~~
set service ssh port '22'
~~~

### Ethernet Speed Configuration 

~~~
set interfaces ethernet eth1 duplex 'auto'
~~~

### Configure Source NAT for our “Inside” network.

~~~
set nat source rule 10 outbound-interface 'eth0'
set nat source rule 10 protocol 'all'
set nat source rule 10 source address '10.10.10.0/24'
set nat source rule 10 translation address 'masquerade'
~~~

### DNS forwarder: 

~~~
set service dns forwarding cache-size '0'
set service dns forwarding listen-on 'eth1'
set service dns forwarding name-server '192.168.124.1'
set service dns forwarding name-server '8.8.8.8'
~~~

### IP Config details 

~~~
vyos@vyos:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:01:00 brd ff:ff:ff:ff:ff:ff
    inet 192.168.124.162/24 brd 192.168.124.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:100/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:01:01 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.1/24 brd 10.10.10.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:101/64 scope link 
       valid_lft forever preferred_lft forever
vyos@vyos:~$ 
~~~


### Able to ping to Gateway and Google. 

~~~
vyos@vyos:~$ ping 192.168.124.1 
PING 192.168.124.1 (192.168.124.1) 56(84) bytes of data.
64 bytes from 192.168.124.1: icmp_req=1 ttl=64 time=0.310 ms
64 bytes from 192.168.124.1: icmp_req=2 ttl=64 time=3.41 ms
64 bytes from 192.168.124.1: icmp_req=3 ttl=64 time=0.825 ms
~~~


~~~
vyos@vyos:~$ ping google.com
PING google.com (216.58.200.142) 56(84) bytes of data.
64 bytes from maa05s10-in-f14.1e100.net (216.58.200.142): icmp_req=1 ttl=54 time=46.6 ms
64 bytes from maa05s10-in-f14.1e100.net (216.58.200.142): icmp_req=2 ttl=54 time=52.5 ms
64 bytes from maa05s10-in-f14.1e100.net (216.58.200.142): icmp_req=3 ttl=54 time=70.9 ms
~~~


## Switch Side Configuration : Cumulus-VX ###

~~~
net add interface swp1-15 link autoneg on
~~~

### Adding Bridge to make switch port communicate

~~~
net add bridge bridge port swp1-15
~~~

~~~
net add bridge bridge pvid 1
~~~

### Trunk Configuration 

~~~ 
net add interface swp6 bridge trunk vlans 10
net add interface swp11 bridge trunk vlans 10
~~~

~~~
net show interface swp6
net show interface swp11
~~~


### Check COnfiguration 

~~~
net show interface
~~~

~~~
net show bridge macs
~~~

~~~
cat /etc/network/interfaces
~~~


## Server Side Configuration 

### Check Hostname `` ctrl.example.com ``

~~~
[root@ctrl ~]# hostname
ctrl.example.com
[root@ctrl ~]# 
~~~

### Check IP 

~~~
[root@ctrl ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:03:00 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.10/24 brd 10.10.10.255 scope global noprefixroute eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:300/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:03:01 brd ff:ff:ff:ff:ff:ff
4: eth1.10@eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 00:50:00:00:03:01 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.2/24 brd 10.1.1.255 scope global noprefixroute eth1.10
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:301/64 scope link 
       valid_lft forever preferred_lft forever
[root@ctrl ~]# 
~~~

### Network Configuration  ``ctrl.example.com``

~~~
[root@ctrl ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth1.10 
TYPE=vlan  
BOOTPROTO=none
NAME=eth1.10
DEVICE=eth1.10
ONBOOT=yes
VLAN=yes
IPADDR=10.1.1.2
PREFIX=24
NETWORK=10.1.1.0
[root@ctrl ~]# 
~~~


### Check Hostname  ``cmpt.example.com``

~~~
[root@cmpt network-scripts]# hostname
cmpt.example.com
[root@cmpt network-scripts]# 
~~~

### Check IP 

~~~
[root@cmpt network-scripts]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:04:00 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.20/24 brd 10.10.10.255 scope global noprefixroute eth0
       valid_lft forever preferred_lft forever
   inet6 fe80::250:ff:fe00:400/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:00:00:04:01 brd ff:ff:ff:ff:ff:ff
4: eth1.10@eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 00:50:00:00:04:01 brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.4/24 brd 10.1.1.255 scope global noprefixroute eth1.10
       valid_lft forever preferred_lft forever
    inet6 fe80::250:ff:fe00:401/64 scope link 
       valid_lft forever preferred_lft forever
[root@cmpt network-scripts]# 
~~~

### Network Configuration ``cmpt.example.com``

~~~
[root@cmpt network-scripts]# cat ifcfg-eth1.10 
TYPE=vlan  
BOOTPROTO=none
NAME=eth1.10
DEVICE=eth1.10
ONBOOT=yes
VLAN=yes
IPADDR=10.1.1.4
PREFIX=24
NETWORK=10.1.1.0
[root@cmpt network-scripts]# 
~~~


## Ping from `` ctrl.example.com `` TO  `` cmpt.example.com `` 

~~~
[root@ctrl ~]# ping 10.10.10.20 -c 1
PING 10.10.10.20 (10.10.10.20) 56(84) bytes of data.
64 bytes from 10.10.10.20: icmp_seq=1 ttl=64 time=1.96 ms

--- 10.10.10.20 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.966/1.966/1.966/0.000 ms
[root@ctrl ~]# 
~~~

~~~
[root@cmpt ~]# ping 10.10.10.10 -c 1
PING 10.10.10.10 (10.10.10.10) 56(84) bytes of data.
64 bytes from 10.10.10.10: icmp_seq=1 ttl=64 time=1.31 ms

--- 10.10.10.10 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.315/1.315/1.315/0.000 ms
[root@cmpt ~]# 
~~~


