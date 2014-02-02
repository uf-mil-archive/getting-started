If anything is unclear or you run a problem following this tutorial, submit a bug report using the issue tracker on this page.

Request to join the mailing list at https://groups.google.com/d/forum/uf-mil-software .

1. Install Ubuntu
-----------------

Supported versions of Ubuntu: 12.04, 12.10, 13.04 (64-bit strongly recommended).

Download a CD image from http://releases.ubuntu.com/

Using a VM is not recommended - it requires a much more powerful system and causes 
problems with 3D graphics.

2. Install ROS Hydro
--------------------

Follow all the instructions at http://wiki.ros.org/hydro/Installation/Ubuntu .

3. Make workspace
-----------------

Follow all the instructions at http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment .

4. Install required packages and add required repositories
----------------------------------------------------------

Read the README in each of the ROS package repositories at https://github.com/uf-mil .

5. Add uf-mil repositories
--------------------------

    roscd && cd ../src && mkdir uf-mil && cd uf-mil && for A in rawgps-tools software-common hardware-common SubjuGator PropaGator ; do git clone https://github.com/uf-mil/$A.git ; done

6. Run catkin_make
------------------

    catkin_make -C ~/catkin_ws

This command should complete without any errors if everything works.

7. Complete
-----------

At this point, you can try running the SubjuGator simulator with:

    rosrun subsim sim
