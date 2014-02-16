A brief summary of the last meeting:


We helped each other get the software installed and running. The major problems that were encountered were:

* Only Ubuntu 13.04 actually works, so everyone please move to that. Not using Ubuntu 13.04 results in various compilation errors.
* Several problems were caused by people having done something other than the ROS "Desktop-Full Install." If you're unsure of whether you did that, run `sudo apt-get install ros-hydro-desktop-full` to make sure that everything you need is installed.
* People were having trouble setting up their .bashrc file. After step 3 of the getting started tutorial is completed, you should have added _two_ "source" lines to the end of your .bashrc file. If not configured correctly, this problem reveals itself by `roscd` taking you to "/opt/ros/hydro" instead of "~/catkin_ws/devel".
* The "socat" dependency was missing from the SubjuGator and PropaGator repository READMEs; it's since been added. Without socat installed, you see lots of errors when you try to roslaunch the sub's software.


Then, I demonstrated:

Starting a roscore:

    roscore &

Starting the simulator:

    rosrun subsim sim

Starting the sub's software stack: (Do this while the simulator is running. Press Ctrl-c to stop.)

    roslaunch subsim run.launch

Sending waypoints to make the sub move:

    alias wp="rosrun sub_launch send_waypoint"
    wp depth 1
    wp left 1
    wp forward 10

Running rqt_graph to look at how everything is connected:

    rqt_graph

(Using rqt_graph's default configuration (all checkboxes checked), but changing "Nodes only" to one of the "Nodes/Topics" options results in the clearest display of what's going on.)

Looking at the sub's forward camera's view:

    rosrun image_view image_view image:=forward_camera/image_rect_color

Recording video into a rosbag: (Press Ctrl-c to stop.)

    rosbag record -O sim_video /forward_camera/image_rect_color

Playing the rosbag back:

    rosbag play sim_video.bag
