# SOFAR's ASSIGNMENT
The goal of this assignment is to build a simulation with ROS2 in which a robot follows a moving target at a fixed distance.

For doing that, first of all I installed the Nav2 project [1], which can be used in ROS2 whenever you have to deal with applications concerning the navigation of robots in the environment.
In particular, the simulation that I had to implement is commonly called "following dynamic point" maintaining a fixed distance, and I found a tutorial about it [2], which I adapted and analyzed appropriately. 

The simulation is based on the behavior tree `follow_point.xml`, which sets the behavior of the robot (`turtlebot3_waffle`) in the environment (`turtlebot3_world`), and whose scheme is shown below.

![alt text](https://github.com/MartinaGermani/SOFAR_Assignment/blob/main/follow_point.png?raw=true)

So, as you can see in the scheme:
- for following a moving target, it reprograms, with a rate of 1 Hz, a new path to pose and it sends it to the controller,
- for updating the dynamic target that the robot is following, the `GoalUpdater` node takes as input the current goal and it subscribes it to the topic /goal_update,
- for maintaining the robot at a desired distance from the goal, the action node `TruncatePath` allows to stay at a certain distance from the target, 
- the control node `KeepRunningUntilFailure` allows to control if there are failures in the path since in that case it stops to move.

Instead, for sending the update goals to Nav2, I used the `clicked point` button, which allows to publish the coordinates of the goal in the /clicked_point topic, that are sended to the behavior tree to the cpp file `clicked_point_to_pose`.

As launch file, which contains the configuration of the system (program to run, environments, arguments etc), I used `tb3_simulation_launch.py` in the `nav2_bringup` package of Nav2.

## Development environment requirements
This simulation requires the full installation of ROS2 galactic, that you can install by following the steps here https://docs.ros.org/en/galactic/Installation/Ubuntu-Install-Debians.html).

Install also the Nav2 workspace at the following link https://navigation.ros.org/build_instructions/index.html (due to size reasons I was unable to upload it here).

## Installation and use instructions
If you haven't already, first of all you have to install the Nav2 workspace at the following link https://navigation.ros.org/build_instructions/index.html. 
Then:
- replace the `nav2_bringup` and `nav2_bt_navigator` packages with the ones I modified inside the navigation2 package,
- clone the `nav2_test_utilis` package inside the Nav2 workspace. 

Before to build the environment with the `colcon build` ROS2 command, remember to modify the path for the parameter `default_nav_to_pose_bt_xml` inside `nav2_params.yaml`, that you can find in nav2_ws -> src -> navigation2 -> nav2_bringup -> bringup -> params. 

To execute the project, you must execute the following commands on two different terminals:


`ros2 run nav2_test_utils clicked_point_to_pose`


`ros2 launch nav2_bringup tb3_simulation_launch.py`

## Status of the project
In the current state of the implementation, the robot will follow the moving target as long as the distance between the two is greater than 1 meter. Therefore a future development could be to have the robot and the target move simultaneously, so that the distance between them is constant.
In fact, a limit of this implementation is precisely that the robot tries to remain at a fixed distance from the target, but the target itself can totally change position in the environment at any time with only one click. 

Another limit of this implementation is that, since the new position of the target is determined with only one click, the robot and the moving target don't follow exactly the same path; so, a future development could make sure that between one click and the next the user, with the movement of the mouse, creates a path, which will then be followed by the robot.

## Reference
[1] https://navigation.ros.org/

[2] https://navigation.ros.org/tutorials/docs/navigation2_dynamic_point_following.html#tutorial-steps

[3] S. Macenski, F. Martín, R. White, J. Clavero. The Marathon 2: A Navigation System. IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 2020.
