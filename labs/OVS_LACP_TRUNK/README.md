# OVS_LACP_TRUNK


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q8.png)


### Agenda
*  The aim of this lab is to create network using `` Openvswitch `` that include ``OVS BOND`` , ``OBS BRIDGE`` and Trunk Port, which will be land from Switch.

### Router Configuration

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q1.png)

* Interface Configuration :-

~~~
set interfaces ethernet eth0 address 'dhcp'
set interfaces ethernet eth0 description 'OUTSIDE'
set interfaces ethernet eth1 address '192.168.100.1/24'
set interfaces ethernet eth1 description 'INSIDE'
~~~

* SSH Configure:

~~~
set service ssh port '22'
~~~

* Ethernet Speed Configuration

~~~
set interfaces ethernet eth1 duplex 'auto'
~~~

* Configure Source NAT for our "Inside" network.

~~~
set nat source rule 10 outbound-interface 'eth0'
set nat source rule 10 protocol 'all'
set nat source rule 10 source address '192.168.100.0/24'
set nat source rule 10 translation address 'masquerade'
~~~

* DNS forwarder:

~~~
set service dns forwarding cache-size '0'
set service dns forwarding listen-on 'eth1'
set service dns forwarding name-server '192.168.124.1'
set service dns forwarding name-server '8.8.8.8'
~~~

### Switch Configuration

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q2.png)

*  We have ``11`` ports in this switch including ``loopback`` and ``mgmnt``

*  Activate interface

~~~
net add interface swp1-3
~~~

*  Adding Slaves (swp6-7 and swp8-9) to bond

~~~
net add bond bond0 bond slaves swp6-7
net add bond bond1 bond slaves swp8-9
~~~


* Configure LACP Protocol

~~~
net add bond bond0 vlan-protocol 802.1ad
net add bond bond1 vlan-protocol 802.1ad
~~~


* Add interfaces and bond in Bridge to make switch port communication


~~~
net add bridge bridge port swp1-3
net add bridge bridge port bond0
net add bridge bridge port bond1
net add bridge bridge pvid 1
~~~


* Trunked Vlan Configuration

~~~
net add bond bond0 bridge trunk vlans 201-204
net add bond bond1 bridge trunk vlans 201-204
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q3.png)


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q4.png)

### Server Configuration


~~~
# cat ifcfg-eth0
~~~

~~~
DEVICE="eth0"
BOOTPROTO="static"
ONBOOT="yes"
TYPE="Ethernet"
IPADDR=192.168.100.10
PREFIX=24
GATEWAY=192.168.100.1
DNS1=192.168.124.1
DNS2=8.8.8.8
~~~

~~~
# cat ifcfg-eth1
~~~

~~~
DEVICE="eth1"
BOOTPROTO="static"
ONBOOT="yes"
TYPE="Ethernet"
~~~

~~~
# cat ifcfg-eth2
~~~

~~~
DEVICE="eth2"
BOOTPROTO="static"
ONBOOT="yes"
TYPE="Ethernet"
~~~


~~~
# cat ifcfg-ovsbr0
~~~

~~~
DEVICE="ovsbr0"
ONBOOT="yes"
DEVICETYPE="ovs"
TYPE="OVSBridge"
BOOTPROTO="none"
HOTPLUG="no"
~~~

~~~
# cat ifcfg-bond0
~~~


~~~
DEVICE="bond0"
ONBOOT="yes"
DEVICETYPE="ovs"
TYPE="OVSBond"
OVS_BRIDGE="ovsbr0"
BOOTPROTO="none"
BOND_IFACES="eth1 eth2"
OVS_OPTIONS="bond_mode=balance-tcp lacp=active"
HOTPLUG="no
~~~

~~~
# cat ifcfg-vlan201
~~~

~~~
DEVICE=vlan201
ONBOOT=yes
HOTPLUG=no
NM_CONTROLLED=no
PEERDNS=no
DEVICETYPE=ovs
TYPE=OVSIntPort
OVS_BRIDGE=ovsbr0
OVS_OPTIONS="tag=201"
BOOTPROTO=static
IPADDR=172.17.1.16
NETMASK=255.255.255.0
~~~


~~~
ovs-vsctl add-br ovsbr0

ovs-vsctl add-bond ovsbr0 bond0 eth1 eth2

ovs-vsctl set port bond0 lacp=active

ovs-appctl bond/show <bond name>

ovs-appctl lacp/show <bond name>

ovs-vsctl set port bond0 trunks=201-204
~~~

~~~
# ovs-vsctl list port bond0
~~~

~~~
_uuid               : b9e69295-8e1b-4aaa-9d34-baa5d4b7fcf7
bond_active_slave   : "00:50:00:00:03:01"
bond_downdelay      : 0
bond_fake_iface     : false
bond_mode           : balance-tcp
bond_updelay        : 0
cvlans              : []
external_ids        : {}
fake_bridge         : false
interfaces          : [0b403e49-696e-4fe8-aad4-fc25b3e46211, ba0759f3-d3df-46c4-a12a-d2c68d37ccf6]
lacp                : active
mac                 : []
name                : "bond0"
other_config        : {}
protected           : false
qos                 : []
rstp_statistics     : {}
rstp_status         : {}
statistics          : {}
status              : {}
tag                 : []
trunks              : [201, 202, 203, 204]
vlan_mode           : []
~~~

~~~
# ovs-vsctl show
~~~

~~~
8bd9d537-4a7f-404e-9131-7ff380d471dc
    Bridge "ovsbr0"
        Port "vlan201"
            tag: 201
            Interface "vlan201"
                type: internal
        Port "ovsbr0"
            Interface "ovsbr0"
                type: internal
        Port "bond0"
            trunks: [201, 202, 203, 204]
            Interface "eth2"
            Interface "eth1"
    ovs_version: "2.9.0"
~~~


### Testing

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q5.png)


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q6.png)


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q7.png)

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q9.png)


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q10.png)


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/OVS_LACP_TRUNK/images/q11.png)
