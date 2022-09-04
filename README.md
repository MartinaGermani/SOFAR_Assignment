# SOFAR's ASSIGNMENT
The goal of this assignment is to build a simulation with ROS2 in which a robot follows a moving target at a fixed distance.

For doing that, first of all I installed the Nav2 project [1], which can be used in ROS2 whenever you have to deal with applications concerning the navigation of robots in the environment.
In particular, the simulation that I had to implement is commonly called "following dynamic point" maintaining a fixed distance, and I found a tutorial about it [2], which I adapted and analyzed appropriately. 

The simulation is based on the behavior tree `follow_point.xml`, which sets the behavior of the robot (`turtlebot3_waffle`) in the environment (`turtlebot3_world`), and whose scheme is shown below.
![alt text](https://github.com/MartinaGermani/SOFAR_Assignment/blob/main/follow_point.png?raw=true)


## Development environment requirements:
This simulation requires the installation of ROS2 galactic.

## Installation and use instructions:
To install the project, you can directly use the following command in your terminal:

`git clone https://github.com/MartinaGermani/SOFAR_Assignment.git`


To execute the project, you must execute the following commands on two different terminals:


`ros2 run nav2_test_utils clicked_point_to_pose`


`ros2 launch nav2_bringup tb3_simulation_launch.py`

## Reference
[1] https://navigation.ros.org/

[2] https://navigation.ros.org/tutorials/docs/navigation2_dynamic_point_following.html#tutorial-steps
