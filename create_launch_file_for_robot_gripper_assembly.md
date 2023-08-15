## To create a launch file of the assembly of ur5e robot and robotiq_2f_85 gripper

# Create a new ROS package
In `ur5_ws/src` folder

```bash
catkin_create_pkg my_robot std_msgs rospy roscpp
```

```bash
cd ~/ur5_ws
catkin_make
```


```bash
source ~/ur5_ws/devel/setup.bash
```

# Copy the required files in the new package
1. Create a folder 'URDF' in 'my_robot'
2. Copy the ur5e_macro.xacro and robotiq_arg2f_85_macro.xacro files into the my_robot/urdf folder.
3. Create a new xacro file called ur5e_robotiq.xacro in the urdf folder.

# Create URDF assembly
Inside the new xacro file, ur5e_robotiq.xacro, copy the following code:
```bash
<?xml version="1.0"?>
<robot name="ur5e_robotiq85" xmlns:xacro="http://www.ros.org/wiki/xacro">

  <xacro:include filename="ur5e_macro.xacro"/>

  <xacro:include filename="robotiq_arg2f_85_macro.xacro"/>

  <!-- Instantiate the ur5e robot -->
  <xacro:ur5e_robot prefix="ur5e_"/> 
  
  <!-- Instantiate the robotiq 85 gripper -->
 <xacro:robotiq_arg2f_85 prefix="gripper_"/>
 
   <!-- Add a fixed joint connecting the gripper to the robot wrist -->
 <joint name="gripper_joint" type="fixed">
  <parent link="ur5e_tool0"/>
  <child link="gripper_base_link"/>
</joint>
 
</robot>
```
<!--
1. Include the UR5e and Robotiq macro xacro files:
```bash
<robot name="ur5e_robotiq" xmlns:xacro="http://www.ros.org/wiki/xacro">

  <xacro:include filename="ur5e_macro.xacro"/>

  <xacro:include filename="robotiq_arg2f_85_macro.xacro"/>

</robot>
```
2. Instantiate the UR5e robot macro with appropriate prefix:
```bash
<xacro:ur5e_robot prefix="ur5e_"/>
```
3. Instantiate the Robotiq gripper macro with appropriate prefix:
```bash
<xacro:robotiq_arg2f_85 prefix="gripper_"/>
```
4. Add a fixed joint connecting the gripper to the robot's wrist:
```bash
<joint name="gripper_joint" type="fixed">
  <parent link="ur5e_tool0"/>
  <child link="gripper_base_link"/>
</joint>
```
-->
- Run the xacro file to generate the URDF model:
```bash
rosrun xacro xacro ur5e_robotiq85.xacro > ur5e_robotiq85.urdf
```

# Create launch file for assembly
To visualize the combined UR5 and Robotiq gripper xacro model in RViz and control it with MoveIt:
<!--1. Launch RViz and set the 'Global Options' > 'Fixed Frame' to 'base'.
-->
<!--3. In the 'Displays' panel, set the 'RobotModel' display and choose the generated URDF file as the robot description.
-->
1. Launch MoveIt:

```bash
roslaunch moveit_setup_assistant setup_assistant.launch
```

2. Go through the MoveIt setup assistant and configure the UR5e robotiq robot model.
Following link is helpful while going through the setup assistant:

https://roboticscasual.com/ros-tutorial-how-to-create-a-moveit-config-for-the-ur5-and-a-gripper/[https://roboticscasual.com/ros-tutorial-how-to-create-a-moveit-config-for-the-ur5-and-a-gripper/]


3. Generate the MoveIt configuration files from the setup assistant. Save it in a folder 'moveit_config' inside 'my_robot' created earlier.

4. Launch MoveIt with the configured URDF:

```bash
roslaunch moveit_config move_group.launch
```



*Completed with help of https://claude.ai*
