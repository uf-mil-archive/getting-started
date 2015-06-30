To use the actuator board on the bench:

1. Supply +32 volt power to it
2. Connect it via USB to a computer
3. Run `rosrun kill_handling kill_master` to start the kill master
4. Run `k` and then Ctrl-C it in order to clear the initial kill
5. Run `rosrun actuator_driver actuator_driver _port:=/dev/ttyUSB0` to start the actuator driver
6. Now, you can do things like:

<!-- end the list -->

    rosservice call /actuator_driver/pulse_valve "valve: 0
    duration:
      secs: 1
      nsecs: 0"

or

    rosservice call /actuator_driver/set_valve "valve: 1
    opened: true"

Valve numbers are 0 to 5, inclusive.
