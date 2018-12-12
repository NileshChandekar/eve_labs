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


