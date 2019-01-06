# LACP with TRUNK Configuration

### Router Configuration :

#### Configure network interfaces:

~~~
set interfaces ethernet eth1 address 'dhcp'
set interfaces ethernet eth1 description 'OUTSIDE'
set interfaces ethernet eth2 address '192.168.100.1/24'
set interfaces ethernet eth2 description 'INSIDE'
~~~

#### Ethernet Speed Configuration

~~~
set interfaces ethernet eth2 duplex 'auto'
~~~

#### Configure Source NAT for our “Inside” network.

~~~
set nat source rule 10 outbound-interface 'eth1'
set nat source rule 10 protocol 'all'
set nat source rule 10 source address '192.168.100.0/24'
set nat source rule 10 translation address 'masquerade'
~~~

#### DNS forwarder:

~~~
set service dns forwarding cache-size '0'
set service dns forwarding listen-on 'eth2'
set service dns forwarding name-server '10.XX.XX.25'
set service dns forwarding name-server '8.8.8.8'
~~~


### Switch Side Configuration :

#### We have ``24 Port`` switch device.

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/LACP_WITH_TRUNK/images/s3.png)


#### Activate interface

~~~
net add interface swp1-20
~~~

#### Adding Slaves (swp21-24) to bond

~~~
net add bond bond0 bond slaves swp21-24
~~~

#### Configure LACP Protocol

~~~
net add bond bond0 vlan-protocol 802.1ad
~~~

#### Add ``interfaces`` and ``bond`` in Bridge to make switch port communication

~~~
net add bridge bridge port swp1-20
net add bridge bridge port bond0
net add bridge bridge pvid 1
~~~

#### Trunked Vlan Configuration

~~~
net add bond bond0 bridge trunk vlans 201-204
~~~

#### After doing above configuration

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/LACP_WITH_TRUNK/images/s4.png)

### Validate the Configuration:

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/LACP_WITH_TRUNK/images/s5.png)
