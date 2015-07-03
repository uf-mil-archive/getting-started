Run something like

    rosrun camera_calibration cameracalibrator.py -q 0.0635 -s 8x6 --fix-principal-point camera:=forward_camera image:=/forward_camera/image_color
