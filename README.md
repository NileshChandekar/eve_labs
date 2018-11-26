### Router Configureation 

~~~
vyos@vyos:~$ sh configuration commands 
~~~

~~~
set interfaces ethernet eth0 address 'dhcp'
set interfaces ethernet eth0 description 'OUTSIDE'
set interfaces ethernet eth0 hw-id '00:50:00:00:01:00'
set interfaces ethernet eth1 address '10.10.10.1/24'
set interfaces ethernet eth1 description 'INSIDE'
set interfaces ethernet eth1 hw-id '00:50:00:00:01:01'
set interfaces loopback 'lo'
set nat source rule 100 outbound-interface 'eth0'
set nat source rule 100 source address '10.10.10.0/24'
set nat source rule 100 translation address 'masquerade'
set protocols static table 100 route 0.0.0.0/0 next-hop '192.168.124.1'
set service ssh port '22'
set system config-management commit-revisions '20'
set system console device ttyS0 speed '9600'
set system login user vyos authentication encrypted-password '$1$iiT3lq06$8imUS23xLM45kglIbhrPo/'
set system login user vyos authentication plaintext-password ''
set system login user vyos level 'admin'
set system ntp server '0.pool.ntp.org'
set system ntp server '1.pool.ntp.org'
set system ntp server '2.pool.ntp.org'
set system package repository community components 'main'
set system package repository community distribution 'helium'
set system package repository community url 'http://packages.vyos.net/vyos'
set system syslog global facility all level 'notice'
set system syslog global facility protocols level 'debug'
vyos@vyos:~$ 
~~~

