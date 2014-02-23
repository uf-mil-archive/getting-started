Before you start, request to join the mailing list at 
https://groups.google.com/d/forum/uf-mil-software.

If anything is unclear or you run into a problem following this tutorial, ask 
a question on the mailing list. If you find a problem with this 
documentation, submit a bug report using the issue tracker on this page.

1. Install Ubuntu
-----------------

Only Ubuntu 13.04 is supported. The 64-bit version is strongly recommended.

Upgrade an existing install of Ubuntu or download a CD image from 
http://releases.ubuntu.com/raring/.

Using a VM is not recommended - it requires a much more powerful system 
and causes problems with 3D graphics.

2. Install ROS Hydro
--------------------

Follow all the instructions at 
http://wiki.ros.org/hydro/Installation/Ubuntu. Make sure to do the 
"Desktop-Full Install," as opposed to the "Desktop Install" or 
"ROS-Base."

3. Make workspace
-----------------

Follow all the instructions at 
http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment.

The linked tutorial omits this, but you have to add the following to your
`.bashrc`, after `source /opt/ros/hydro/setup.bash` (which was added during
the tutorial) in order to finish setting up your workspace:

    source ~/catkin_ws/devel/setup.bash

After adding those lines to your `.bashrc` file, open a new shell so the
settings take effect.

4. Install required packages and add required repositories
----------------------------------------------------------

Read the README in each of the ROS package repositories at 
https://github.com/uf-mil.

5. Add uf-mil repositories
--------------------------

    roscd && cd ../src && mkdir uf-mil && cd uf-mil && for A in rawgps-tools software-common hardware-common SubjuGator PropaGator ; do git clone https://github.com/uf-mil/$A.git ; done

6. Run catkin_make
------------------

    catkin_make -C ~/catkin_ws

This command should complete without any errors if everything works.

7. Complete
-----------

At this point, you can start a roscore and try running the SubjuGator simulator with:

    roscore &
    rosrun subsim sim

The simulator's user interface is documented in the subsim's README.md file, to get there:

    roscd subsim
    nano readme.md

Updating uf-mil repositories
----------------------------

Go in each directory within `~/catkin_ws/src/uf_mil` and type `git 
pull`. After that, recompile by running `catkin_make -C ~/catkin_ws`.
