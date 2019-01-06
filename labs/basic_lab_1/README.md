### Topology

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n1.png)

### Router Configureation

### Below is a very basic configuration example that will provide a NAT gateway for a device with two interfaces.

### Enter configuration mode:

~~~
vyos@vyos$ configure
~~~

### Configure network interfaces:

~~~
set interfaces ethernet eth1 address 'dhcp'
set interfaces ethernet eth1 description 'OUTSIDE'
set interfaces ethernet eth2 address '10.10.10.1/24'
set interfaces ethernet eth2 description 'INSIDE'
~~~

### Enable SSH for remote management:

~~~
set service ssh port '22'
~~~

### Ethernet Speed Configuration 

~~~
set interfaces ethernet eth2 duplex 'auto'
~~~

### Configure Source NAT for our “Inside” network.

~~~
set nat source rule 10 outbound-interface 'eth1'
set nat source rule 10 protocol 'all'
set nat source rule 10 source address '10.10.10.0/24'
set nat source rule 10 translation address 'masquerade'
~~~

### DNS forwarder:

~~~
set service dns forwarding cache-size '0'
set service dns forwarding listen-on 'eth2'
set service dns forwarding name-server '192.168.124.1'
set service dns forwarding name-server '8.8.8.8'
~~~


![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n2.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n3.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n4.png)




![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n5.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n6.png)



![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/basic_lab_1/images/n10.png)
