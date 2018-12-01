### Topology 

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n1.png)

### Router Configureation 

### Below is a very basic configuration example that will provide a NAT gateway for a device with two interfaces.

### Enter configuration mode:

~~~
vyos@vyos$ configure
~~~

### Configure network interfaces:

~~~
set interfaces ethernet eth0 address dhcp
set interfaces ethernet eth0 description 'OUTSIDE'
set interfaces ethernet eth1 address "10.10.10.1/24"
set interfaces ethernet eth1 description 'INSIDE'
~~~


### Enable SSH for remote management:

~~~
set service ssh port '22'
~~~

### Configure Source NAT for our “Inside” network.

~~~
set nat source rule 10 source address '10.10.10.0/24'
set nat source rule 10 outbound-interface 'eth0'
set nat source rule 10 protocol 'all'
set nat source rule 10 translation address 'masquerade'
~~~

### To create routing table 10 and add a new default gateway to be used by traffic matching :

~~~
set protocols static table 10 route 0.0.0.0/0 next-hop "192.168.124.1"
~~~


### Ensure that the router is configured to use the correct DNS server:

~~~
set service dns forwarding name-server "192.168.124.1"
set service dns forwarding name-server "8.8.8.8"
set service dns forwarding listen-on "eth0"
~~~


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n2.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n3.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n4.png)




![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n5.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n6.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/images/n10.png)
