# VESC_ROS2

This is a ROS2 Humble implementation for the Vedder VESC controller. The code is based on the original F1TENTH Foundation's code for the ROS2 infrastructure and includes the following modifications:
- Removed some of the files and nodes that were not relevant for our implementation
- Added a simple Differential Drive example for a robot using a Sony Gamepad
- Added a Lifecycle Node (Managed Node) implementation for the VESC driver, making it more suitable for state based software architectures.

The said modifications were done under the AURA Project and Maynooth University, Ireland. The code is open source and free to use and modify. The author provides no guarantee of bug fixes, updates, or continued support.

# How to set up

```bash
cd ~
git clone https://github.com/PetriJF/VESC_ROS2.git
cd VESC_ROS2
source /opt/ros/humble/setup.bash
colcon build
```

# How to use (Lifecycle node code)

Note: Under the assumption that the VESC is connected to your machine and it has power.

If you haven't done so already, build and source your code:
```bash
source /opt/ros/humble/setup.bash
cd ~/VESC_ROS2
colcon build
source install/setup.bash
```

Run the launch file for the lifecycle node:
```bash
ros2 launch vesc_driver vesc_driver_lc_node.launch.py
```

This will launch the lifecycle node for one vesc with the correct configuration file outlined in the config directory. The node is now in the unconfigured state so you will need to open another terminal to switch to another one (e.g. configured)
```bash
ros2 lifecycle set /vesc_driver_lc_node configure
```

The VESC won't actually run the motors until you move to the active state (the same command as above but with activate at the end instead of configure). To switch around the states use activate, deactivate, configure, unconfigure, shutdown. You can read the lifecycle (managed) node ros2 wiki page for more information on transitions and states.

Finally if you want to test the VESC, once you are in the active state, run this in a third terminal (replace the speed with a value appropriate for your motor):
```bash
ros2 topic pub /commands/motor/speed std_msgs/msg/Float64 "{data: 1000.0}"
```

This entire process can be automated using a lifecycle manager node.