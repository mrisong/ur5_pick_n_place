# ur5_pick_n_place

from https://github.com/robotvisionlabs/autonomous-manipulation/blob/main/docs/UR5Real.md


3. Extract the calibration information from the robot - this shall provide the current parameters of the robot in a 'yaml' file[[5]](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/tree/master#prepare-the-ros-pc):
    ```bash
    roslaunch ur_calibration calibration_correction.launch robot_ip:=138.253.86.49 target_filename:="${HOME}/my_robot_calibration.yaml"
    ```

4. In a new terminal, start the robot driver using the existing launch file and pass the calibration information along with it:
    ```bash
    roslaunch ur_robot_driver ur5e_bringup.launch robot_ip:=138.253.86.49 kinematics_config:="${HOME}/my_robot_calibration.yaml"
    ```

5. On the teach pendant, press the :arrow_forward: icon on the last used display screen from step 2.v.
![20230809_123137Playmarked](https://github.com/robotvisionlabs/autonomous-manipulation/assets/17614773/fe9a6d7f-6f40-4868-8b12-9a28dfde95dd)


5. In a new terminal, use [MoveIt!](http://wiki.ros.org/action/show/moveit?action=show&redirect=MoveIt) to control the robot and allow motion planning[[2]](http://wiki.ros.org/universal_robot/Tutorials/Getting%20Started%20with%20a%20Universal%20Robot%20and%20ROS-Industrial):
    ```bash
    roslaunch ur5e_moveit_config moveit_planning_execution.launch
    ```
    The screen should now show - 'You can start planning now!'

6. In a new terminal, start [RViz](http://wiki.ros.org/rviz), including the MoveIt! motion planning plugin:
    ```bash
    roslaunch ur5e_moveit_config moveit_rviz.launch
    ```


Connect the camera in the same RViz, this is used only if we need to callibrate:
 ```roslaunch realsense2_camera rs_camera.launch```

Launch the calibration file:
 ```bash
 roslaunch image2position camera_pose.launch
 ```

Connect the gripper
from http://wiki.ros.org/robotiq/Tutorials/Control%20of%20a%202-Finger%20Gripper%20using%20the%20Modbus%20RTU%20protocol%20%28ros%20kinetic%20and%20newer%20releases%29
```bash
sudo usermod -a -G dialout mridulsongara
```
```bash
sudo dmesg | grep tty
```
```bash
rosrun robotiq_2f_gripper_control Robotiq2FGripperRtuNode.py /dev/ttyUSB0
```


Run the vision file(it will do the vision part of ROS - segmentation and publish image on the required topic, which shall be later used by ur_copntoller.py file):
 ```bash
rosrun image2position vision_realsense.py -m real -t bottle
 ```
 ```bash
rosrun image2position ur5_commander.py -m real
 ```
