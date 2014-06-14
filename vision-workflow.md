Vision Workflow
===============

Playing video from a bag file
-----------------------------

    rosbag play MY_FILE.bag
    rosrun image_proc image_proc __ns:=forward_camera
    rosrun image_proc image_proc __ns:=down_camera

Then, you can do

    rosrun sub_launch camera_views

as usual.


Testing vision finders
----------------------

    roslaunch legacy_vision camera:=forward_camera
    rosrun actionlib axclient.py /find2_forward_camera

(Change "forward" to "down" as appropriate.) Then, change `object_names` to
`['shooter']`, for example, and click "Send Goal".
