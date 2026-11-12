# F1TENTH workspace

This root repository intentionally tracks only:

- f1tenth_ws.repos
- This README

The repositories under src/ are independent Git repositories. They are not
root-repository submodules.

## Repository index

- f1tenth_system — vehicle hardware bringup, VESC, LiDAR, joystick, odometry, and command multiplexing.
- f1tenth_bringup — platform-specific launch files, mapping, raceline orchestration, and stack-level configuration.
- f1tenth_controllers — MPC and pure-pursuit controllers.
- particle_filter — LiDAR-based Monte Carlo localization.
- raceline_generation — lane-bound generation and raceline optimization.
- transport_drivers — reusable serial, UDP, and asynchronous transport drivers.
- udp_msgs — ROS 2 UDP message interfaces.
- f1tenth_system nested repositories — VESC, teleoperation tools, and Ackermann multiplexer components.

## Configuration ownership policy

Platform-specific launch files and configuration changes must be kept in
src/f1tenth_bringup. Do not place platform-specific launch or YAML changes in
the root workspace repository or in the upstream-oriented driver repositories.

Examples include:

- LiDAR and sensor launch selection
- SLAM and mapping launch files
- controller launch files
- raceline and map paths
- platform-specific joystick mappings
- platform-specific topic remappings
- vehicle calibration overrides used only by one platform

Generic driver code and reusable package defaults belong in their owning package.
Generated maps, lanes, racelines, and other platform data also belong under
f1tenth_bringup.

Platform-specific changes must be committed and pushed on a platform branch,
such as platform1 or platform2. The root manifest should reference that branch
when reproducing the platform workspace.

## Recreate the workspace

Install the repository importer:

~~~bash
sudo apt update
sudo apt install python3-vcstool
~~~

Import the configured repositories from the workspace root:

~~~bash
cd ~/f1tenth_ws
vcs import < f1tenth_ws.repos
~~~

Initialize nested submodules used by f1tenth_system:

~~~bash
git -C src/f1tenth_system submodule update --init --recursive
~~~

Install ROS dependencies and build:

~~~bash
cd ~/f1tenth_ws
rosdep install --from-paths src --ignore-src -r -y
source /opt/ros/humble/setup.bash
colcon build --symlink-install
source install/setup.bash
~~~

The manifest records the repository URL and branch for each package. To update
a package, edit its entry in f1tenth_ws.repos and re-run the appropriate
vcs import or vcs pull command.
