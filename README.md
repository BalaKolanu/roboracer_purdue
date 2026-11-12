# spring26_ws Repository Summary

Updated: March 2, 2026 (local workspace state)

## Hardware setup
- Jetson Orin Nano
- Standard F1tenth Lidar
- Traxxas Slash Velineon motor
- VESC mk5
- Seeed NRF IMU(to be replaced by BNO085 soon)
- Intel RealSense Depth Camera 435i

### Hardware performance
- Jetson Orin Nano: GPU utilisez 100% by particle filter, would benifit from an upgrade.
- Lidar: Adequate
- Motor: Adequate
- VESC: Odometry very unreliable, either needs a better ODOM source or an active odometry tuning workflow.
- IMU: waiting for BNO085

## Branches (Current)

### `f1tenth_planning`
- Local branches: `main`
- Remote tracking highlights: `origin/main` plus long-lived dev/release branches (`dev*`, `v1.0.0`, etc.)

### `f1tenth_system`
- Local branches: `main`
- Remote tracking highlights: `origin/main`, `origin/foxy-devel`
- Notes: snapshot tags kept for old branch tips: `AMCL`, `amcl-localization-stable`, `foxy-devel`, `mapping-scan_matching`

### `raceline_optimization`
- Local branches: `main`
- Remote tracking highlights: `origin/main`
- Notes: `closed_loop` was merged into `main` and removed locally

### `particle_filter`
- Local branches: `particle_filter-GPU`, `foxy-devel`
- Remote tracking highlights: `origin/particle_filter-GPU`, `origin/foxy-devel`

### `transport_drivers`
- Local branches: `main`
- Remote tracking highlights: `origin/main` plus distro branches (`dashing`, `foxy`, `galactic`, `humble`)

### `udp_msgs`
- Local branches: `main`
- Remote tracking highlights: `origin/main`

## Repo-by-Repo Summary

### 1) `f1tenth_planning`
- Main functionality:
- Python planning/control library for F1TENTH
- Controllers include `dynamic_mpc`, `kinematic_mpc`, `lqr`, `pure_pursuit`, `stanley`
- Planners include `fgm`, `graph_planner`, `lane_switcher`, `lattice_planner`, `wall_follow`
- Launch files:
- None (library/examples repo)

### 2) `f1tenth_system`
- Main functionality:
- Core onboard F1TENTH stack and hardware bringup
- Mapping and localization bringup with scan-matching, AMCL, and GPU particle filter launch integration
- IMU-wheel odometry fusion and odom calibration utilities used by mapping/localization pipelines
- Launch files:
- `src/f1tenth_system/f1tenth_stack/launch/bringup_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/localization_amcl_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/localization_scan_matching_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/mapping_scan_matching_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/mapping_odom_only_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/odom_calibration_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/scanmatching_slam_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/odom_only_slam_launch.py`
- `src/f1tenth_system/f1tenth_stack/launch/particle_filter_gpu_launch.py`
- `src/f1tenth_system/ackermann_mux/launch/ackermann_mux_launch.py`
- `src/f1tenth_system/vesc/vesc_driver/launch/vesc_driver_node.launch.py`
- `src/f1tenth_system/vesc/vesc_ackermann/launch/ackermann_to_vesc_node.launch.xml`
- `src/f1tenth_system/vesc/vesc_ackermann/launch/vesc_to_odom_node.launch.xml`
- `src/f1tenth_system/teleop_tools/joy_teleop/launch/example.launch.py`
- `src/f1tenth_system/teleop_tools/mouse_teleop/launch/mouse_teleop.launch.py`

### 3) `raceline_optimization`
- Main functionality:
- Autonomy orchestration and path-following test package
- Includes `astar_global_planner`, `pure_pursuit_controller`, and `raceline_path_publisher`
- Contains integrated trajectory/raceline generation stack under `src/raceline_optimization/trajectory_optimization`
- Launch files:
- `src/raceline_optimization/launch/autonomy_pipeline.launch.py`
- `src/raceline_optimization/trajectory_optimization/launch/raceline_generator.launch.py`

### 4) `particle_filter`
- Main functionality:
- Monte Carlo localization with RangeLibc
- GPU-focused localization config support (`rmgpu`, optional GPU resample/motion path)
- Launch files:
- `src/particle_filter/launch/localize_launch.py`
- Legacy ROS1 launch files under `src/particle_filter/deprecated/`

### 5) `transport_drivers`
- Main functionality:
- ROS2 transport-layer libraries/drivers for serial and UDP
- Launch files:
- `src/transport_drivers/serial_driver/launch/serial_driver_bridge_node.launch.py`
- UDP is typically started with `ros2 run udp_driver ... --params-file ...`

### 6) `udp_msgs`
- Main functionality:
- Interface-only package for UDP message/service definitions
- Includes `msg/UdpPacket.msg`, `srv/UdpSend.srv`, `srv/UdpSocket.srv`
- Launch files:
- None
