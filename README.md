# depth_camera

## Table of Contents
  - [Setup ROS2 \& OpenNI SDK (Windows 11) *No longer using*](#setup-ros2--openni-sdk-windows-11-no-longer-using)
  - [Setup ROS \& OpenNI SDK (WIN 11) *Preferred*](#setup-ros--openni-sdk-win-11-preferred)
    - [IMPORTANT NOTES](#important-notes)
  - [How to take picture](#how-to-take-picture)
  - [Integration of ROS depth camera \& OpenCV](#integration-of-ros-depth-camera--opencv)
  - [Docker](#docker)
    - [Installation](#installation)
    - [QuickStart](#quickstart)
    - [Extra commands](#extra-commands)
  - [Issues](#issues)
  - [Future Plans](#future-plans)
  - [Author](#author)


## Setup ROS2 & OpenNI SDK (Windows 11) *No longer using*
1. Download a VM of your choice (Oracle VirtualBox is used)
2. Download Ubuntu 22.04 LTS Jammy Jellyfish iso image (so its compatible with ROS2 Humble)
3. Setup the Ubuntu VM (at least 4GB RAM but 8GB is better, minimum 25GB storage, 4CPU, USB of depth cam need to be added)
4. Once in Ubuntu, download ROS2 Humble following [here](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html) (all commands are typed in terminal can press window key and search for it)
5. Open firefox and go to [here](https://www.orbbec.com/developers/openni-sdk/) & scroll to the bottom under `Wrappers for OpenNI SDK` to download OpenNI SDK for ROS2
6. After download the tar zip file, extract it & open the README.md. Follow the instructions in README.md. (`IMPORTANT`: Change every `galactic` word in the file to `humble`)
7. Halfway through you might run into an error with a command starting with `colcon` because its not installed. Follow the steps from [here](https://colcon.readthedocs.io/en/released/user/installation.html#:~:text=In%20the%20context%20of%20the%20ROS%20project) to download it.
8. At the end, you should see a program with black and white image

## Setup ROS & OpenNI SDK (WIN 11) *Preferred*
1. Download VM
2. Download Ubuntu 20
3. Setup VM (Remember to add usb to vm)
4. Follow steps from here to setup ROS [here](https://wiki.ros.org/noetic/Installation/Ubuntu)
5. Follow steps from here to setup OpenNI [here](https://github.com/orbbec/ros_astra_camera)

### IMPORTANT NOTES
1. Depth camera works on zheng laptop left usb port(1)
2. USB controller of VM is set to usb3.0 (has smoother video stream)
3. This might need to be rerun everytime for it to detect the camera
     ```shell
        roscd astra_camera
        ./scripts/create_udev_rules
        sudo udevadm control --reload && sudo udevadm trigger
     ```

## How to take picture
1. Use below command
```shell
   rosservice call /camera/save_images "{}"
```
2. Images are saved in `~/.ros/image` and are only available when the sensor is on.
3. xdg-open <filename>
   - Replace <filename> with filename

## Integration of ROS depth camera & OpenCV
1. Installation not needed since all necessary dependencies are downloaded in `ros_astra_camera` package
2. Get into `Scripts` folder in `ros_astra_camera`
```shell
   cd ~/ros_ws/src/ros_astra_camera/scripts
```
3. Create a python file
```shell
   touch detect.py
```
4. Paste code from [detect.py](https://github.com/ShadowofSkull/depth_camera/blob/main/detect.py) file into your file in VM
```shell
   gedit detect.py
```
5. Save the file and give it execution permission
```shell
   chmod +x detect.py
```
6. Be sure to source these files on all the terminal before running Step 7-8
```shell
   cd ~/ros_ws
   source /opt/ros/noetic/setup.bash
   source ~/ros_ws/devel/setup.bash
```
   - To not do this for all terminal
      ```shell
         gedit ~/.bashrc
      ```
   - Copy below into the very bottom of the file and save 
      ```shell
      source /opt/ros/noetic/setup.bash
      source ~/ros_ws/devel/setup.bash
      ```   
7. Run the depth camera
```shell
roslaunch ros_astra_camera astra.launch
``` 
8. Open a terminal and run
```shell
   roscore
```
9. Open another terminal and run
```shell
   rosrun ros_astra_camera detect.py
```

## Docker
### Installation
1. Download docker desktop from the [site](https://www.docker.com/products/docker-desktop/)
2. Follow instructions here
   1. Mac: https://docs.docker.com/desktop/install/mac-install/
   2. Windows: https://docs.docker.com/desktop/install/windows-install/
3. vcxsrv X server is required to run gui through x11 forwarding in ssh, [install](https://sourceforge.net/projects/vcxsrv/)
4. Putty (optional)

### QuickStart
**Replace anything in <>**
1. Pull the docker image from dockerhub
```shell
docker pull shadowofskull/depth_cam:latest
```
2. Run a container from the image
```shell
docker run -it -p 22:22 --name ros-x11-ex shadowofskull/depth_cam:latest
```

### Extra commands
- To check container id
```shell
docker ps
```
- To add extra terminal
```shell
docker exec -it <container id> bash
```
- To rerun already created container use
```shell
docker start <container id> 
```

### Dependencies (known)
- ros-noetic-perception=1.5.0-1*
- ros-noetic-ros-core=1.5.0-1*
- openssh-server 
- sudo
- git
- vim 
- gedit 
- cmake 
- make
- ros-noetic-rviz


## Issues
1. Q:Color image not showing
   1. A: Issue lies in ros2 and openni sdk for ros2, use back ros instead

## Future Plans
1. Find out how to integrate object detection with the depth camera/rviz

  
## Author
[Adam](https://github.com/Jung028) and [Zheng](https://github.com/ShadowofSkull/) 
