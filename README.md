Before you start, request to join the
[mailing list](https://groups.google.com/d/forum/uf-mil-software),
put your possible meeting times in the
[poll](http://doodle.com/ybg6sq5vyyat77hv),
and request access to the
[to-do list](https://docs.google.com/document/d/1cZwCfnEqv9jpzpVE7uA_qp4BQ_Zcb403iAPRucudExM/edit?usp=sharing).

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

1. https://github.com/uf-mil/rawgps-tools
2. https://github.com/uf-mil/software-common
3. https://github.com/uf-mil/hardware-common
4. https://github.com/uf-mil/SubjuGator
5. https://github.com/uf-mil/PropaGator

The repositories have several dependencies and provide the commands
you need to run to install them.

5. Add uf-mil repositories
--------------------------

Run:

    roscd && cd ../src && git clone git@github.com:uf-mil/uf-mil

Then, go into the uf-mil directory and type `./pull` to pull the newest commits.

Add `source ~/catkin_ws/src/uf-mil/bashrc` to the end of ~/.bashrc.

6. Run catkin_make
------------------

    cm

This command is defined in ~/catkin_ws/src/uf-mil/bashrc and
should complete without any errors if everything works.

7. Complete
-----------

At this point, you can start a roscore and try running the SubjuGator simulator with:

    roscore &
    rosrun subsim sim

The simulator's user interface is documented in the README file in
its package directory.

Look at the to-do list (linked at the top) to find something to work on.


Updating uf-mil repositories
----------------------------

Go into ~/catkin_ws/src/uf-mil and type `./pull`
