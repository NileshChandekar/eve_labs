# Topology

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l3.png)

# Router Configureation

~~~
Here I will skip Router configuration and will directly jump to Switch configuration.
~~~

# Switch Side Configuration : Cumulus-VX ###

## Switch_1

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l4.png)

~~~
Right now switch is configured with default  network configuration, lets configure network first
~~~

### Activate All interface

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l5.png)

### Adding Slaves (swp2-3) to bond

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l6.png)

### Bonds statistics , Bond is configured but still its showing `` DOWN `` , coz the opposite bond is not activated yet.
![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l7.png)


## Switch_2

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l8.png)

~~~
Right now switch is configured with default  network configuration, lets configure network first
~~~

### Activate All interface

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l9.png)

### Adding Slaves (swp2-3) to bond

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l10.png)

### Bonds statistics , Bond is configured it should show `` UP `` .
![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l7.png)

### Final Result after configuring `` BOND`` on both switch

#### Switch_1

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l11.png)

#### Switch_2

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l12.png)


#### NOTE :

~~~
But still will not able to ping guest each other, we need to add interfaces into bridge to make both side communication.
~~~

#### Switch_1

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l13.png)


#### Switch_2

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l14.png)


# Guest Side Configuration

~~~
Able to ping each other guest.
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l15.png)

# Testing Switch_1

### Test_1

~~~
Lets down one of the bond interface from Switch_1 , let consider (swp2) , `` I am still able to ping ``
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l16.png)

### Test_2

~~~
Lets down other bond interface from Switch_1 , let consider (swp3) , `` ping failed `` , REMEMBER BOTH (swp2 and swp3) INTERFACES ARE IN DOWN STATE.
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l17.png)

### Test_3

~~~
lets up both interfaces. `` I am again able to ping each other ``
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l18.png)


# Testing Switch_2

### Test_1

~~~
Lets down bond0 interface from Switch_2 , `` Ping should failed ``
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l19.png)

### Test_2

~~~
Lets up bond0 interface from Switch_2 , `` It should ping  ``
~~~

![Image ](https://github.com/NileshChandekar/eve_labs/blob/master/labs/lacp_lab_vyos_cumulus/images/l20.png)
