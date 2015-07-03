1. On your computer
-------------------

Configure an ethernet connection to be "Shared to other computers."

Then, run `ifconfig eth0:1 192.168.1.200` (make sure that your IP address does not conflict with somebody else's).

2. On sub
---------

    sudo route del default dev em1
    sudo ifconfig em1:1 10.42.0.1 # should match the subnet of eth0 on your computer
    sudo route add default gw 10.42.0.1 dev em1:1
