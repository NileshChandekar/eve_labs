# OVS_LACP_TRUNK

### Agenda 
*  The aim of this lab is to create network using `` Openvswitch `` that include ``OVS BOND`` , ``OBS BRIDGE`` and Trunk Port, which will be land from Switch. 

### Router Configuration 

![Image ](/home/cNilesh/Redhat/githubprojects/eve_labs/OVS_LACP_TRUNK/images/q1.png)

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


