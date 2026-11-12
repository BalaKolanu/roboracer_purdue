# F1TENTH workspace

This root repository intentionally tracks only:

- `f1tenth_ws.repos`
- This README

The repositories under `src/` are independent Git repositories. They are not root-repository submodules.

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

Initialize nested submodules used by `f1tenth_system`:

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

The manifest records the repository URL and branch for each package. To update a package, edit its entry in `f1tenth_ws.repos` and re-run the appropriate `vcs import` or `vcs pull` command.
