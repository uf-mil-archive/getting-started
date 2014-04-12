ROS System Tutorial
	Made by Ryan Zavoral


---------start sub-------------
start ros main - 
    roscore &
start simulator - 
    rosrun subsim sim
start software stack - 
    roslaunch subsim run.launch

-----Start Virtual Boat--------
start rosmain - 
    roscore &
start boatsim and software stack - 
    roslaunch boatsim run.launch

---------boat startup (for 3d mouse)---------
(for actual boat...make sure you are ssh'd in. Only one person should do this at a time)
    roscore &
    roslaunch propagator_motor_driver start_motor_driver.launch //starts only motors
    rosrun thruster_mapper thruster_mapper //displays thruster values
    rosrun thruster_mapper joy_to_wrench joy:=spacenav/joy //3d mouse mapped to thrusters

--------Waypoints Boat -------
    roslaunch boat_launch navigation.launch 
--gps takes a bit to synch so leave running
    roslaunch boat_launch run.launch 
--runs software stack and motors (basically everything, runs some stuff from line 42)

From file - /sub_launch/scripts/send_waypoint (this is an executable)
'send_waypoint forward 5 --speed .2'
'send_waypoint set_orientation NORTH'
'send_waypoint yaw_left_deg 30'

-------waypoint aliases--------
send waypoints - 
    alias wp="rosrun sub_launch send_waypoint"
    alias k="rosrun kill_handling kill"
    wp (forward, raise, depth, left, right) magnitude

--------Making Missions--------
--Preliminary Setup--
    roscd && cd ../src && mkdir uf-mil && cd uf-mil && for A in rawgps-tools software-common hardware-common SubjuGator PropaGator ; do git clone https://github.com/uf-mil/$A.git ; done && git clone https://github.com/txros/txros.git
Go in each directory within ~/catkin_ws/src/uf-mil and type git pull. After that, recompile by running catkin_make -C ~/catkin_ws.
---------------------
    sudo apt-get install libfftw3-dev
    catkin_make -C ~/catkin_ws
    cd ~/catkin_ws/src/uf-mil/Subjugator/sub_launch/src/sub_launch/missions
    roscore && rosrun mission_core run_missions sub_launch.missions.square

This is a simple python script. Use it as a template to create your own script for the sub to run. For more sub commands, navigate to folder to see files:

    cd ~/catkin_ws/src/uf-mil/software_common/uf_common/src/uf_common/orientation_helpers.py

After creating your own, 
    catkin_make -C ~/catkin_ws 
    rosrun mission_core run_mission sub_launch.mission.<your mission>

There is also a path.py mission. To run do the following:

Kill the sub by pressing 1. 
Start the down cameras with:

    rosrun sub_launch camera_views down
    rosrun sub_launch legacy_debug_images down.

Use i-j-k-l keys to position the sub over the pipe. 
Press 2 to reactivate sub. 
    rosrun mission_core run_missions sub_launch.missions.path 

Redo the the path.py script with the "reef" object (a box with a blinking pinger under it). Try with different camera views.

-----------Motor-------------- 
manual edit calibration: pd_controller.py

----------Camera--------------
- run camera
  rosrun ueye2 ueye2

show forward camera - 
    rosrun image_view image_view image:=(output device)
-CHOOSE ONE OF THE FOLLOWING AT THIS POINT (More output devices exist so we need to find their names so that they may be accessed) 
output device1 = forward_camera/image_rect_color
output device2 = image_raw/

record video rosbag from front - 
    rosbag record -O #NAME_OF_VIDEO# /forward_camera/(output device)
record video rosbag from boat - 
    rosbag record -O #NAME_OF_VIDEO# /(output device)
play rosbag video - 
    rosbag play #NAME_OF_VIDEO#.bag

-----Magnetic Calibration-----
Kill the submarine (by pressing 1)
Start recording raw magnetic data (rosbag record /imu/mag_raw)
Rotate the submarine, sampling every possible rotation (look at subsim's README.md)
Stop recording (press Ctrl-c on rosbag)
Generate the calibration result (rosrun magnetic_hardsoft_compensation generate_config NAME_OF_THE_BAG_FILE_YOU_PRODUCED). Make sure that the points cover the entire sphere. If they don't, re-record the data. Close the window.
Paste the generated configuration (the last 5 lines of output after closing the window) into sub_launch/launch/common.xml, replacing the similar 5 lines that are already there.
Stop and restart the main "roslaunch subsim run.launch" process to load the new configuration.

-----Display info tests-------

    rostopic echo /gps
    rostopic echo /odom
    rostopic echo /trajectory

---Getting Used to System-----
NAVIGATE THROUGH IT - check what is executable, compare it with some of the commands in this file, try some stuff out that you think is convuluted or obtuse
rqt_graph //shows graph allows user to become acquainted with the system

---------Git------------------
How to use git:
git clone repositoryurl - CLONES a repository into your file directory at the current location    git clone https://github.com/rzavo76/getting-started.git 
git init nameofdirOrRepositoryUrl - MAKES a repository linked to the directory or URL
--I recommend using the URL to link instead of the directory when making a new repository
--So the repository should be initialized via the github site before hand
git pull - PULLS updates to the cloned or initiated repository at your current location

after forking a repository to your github pages ---    
    
    git add fileName 
    git commit -m"message"
    git push URLOfGitFork 

- pushes a specific file to your repository, add more files to the add if necessary
Then send a commit request via your github site at the forked repository
Someone should allow the request soon after.

If command below doesnt work - git stash

    git pull /home/fvoight/catkin_ws/src/uf-mil/PropaGator













-----------------------Make Models for Simulator-------------------------------------------------------------
Can choose object or edit mode based on the bottom tab
-------Controls------
hold middle mouse button - turn camera
scroll middle mouse button - zoom in/out
'b' then click and drag - select specific edges or vertices
'a' - select all and unselect all
'g' - grab
	'x' - to move in x coords only
	'y' - to move in y coords only
	'z' - to move in z coords only
'r' - rotate (x, y, z)
'x' - delete selected object

--------Panels-------
'n' - opens transform panel to move the selected object based on coord
in edit mode use subdivide on the left panel to allow more locations of movement on a line
common to delete different vertices to leave a line and then create a vertical plane afterwards using subdivice on points

------Making Model---
1. Delete default block
2. Consider a spatial method to easily deconstruct the object you are creating (ex. make a single plane and rotate or add default shapes)
3. Add an object that you can easily deconstruct
4. Set imperial units under scene on the right panel (this changes the grid)
5. Deconstruct the shape or add more objects
6. move vertices to specific locations of merit(subdivide to get more points)
7. Rotate or ascribe more dimension to the object
8. Render image to see how the object will look
9. save as a blend file
10. export as an obj file - click triangulate faces - change forward to y forward

--Uploading To World--
upload by finding the file through 
    roscd boatsim
    cd scripts
    nano sim 
Type this code and change every iteration of FILE_NAME OR NAME to your decided name or file name.
    NAME_mesh = threed.mesh_from_obj(roslib.packages.resource_file('boatsim', 'models', 'FILE_NAME.obj')) // gives access to the file
    NAME_mesh = NAME_mesh.translate((x,y,z)) //moves the object
    w.objs.append(NAME_mesh) //adds to world
    NAME_geom = ode.GeomTriMesh(NAME_mesh.ode_trimeshdata, space) // this line makes it so the object is solid


