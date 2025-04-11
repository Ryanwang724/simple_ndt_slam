Clone and running in your computer
```bash
cd ros1_ws/src
# please remember to --recurse-submodules !!!!
git clone --recurse-submodules https://github.com/Ryanwang724/simple_ndt_slam.git
```

Install some dependences (glog, gflag)
```bash
cd simple_ndt_slam
sudo chmod +x ./assets/scripts/setup_lib.sh
sudo -E ./assets/scripts/setup_lib.sh   # 加上-E 才能吃到環境變數
```

Open [lidar_localizer/config/ndt_mapping.yaml](lidar_localizer/config/ndt_mapping.yaml), modify the topic name based on your robot setting:
```yaml
lidar_topic: "/points_raw"
imu_topic: "/imu_corrcet"
```

if you are running on the bag, remember to modify the **bag path** in the launch
```html
<arg name="bag_file" default="/home/ryan/shared_dir/dayuan_dataset/a20s.bag" />
<node pkg="rosbag" type="play" name="bag_play" args="$(arg bag_file) --clock -r 1.0" required="false"/>
```

Build and run, please remember modify the config to point out correct topic name
```bash
cd ~/ros1_ws
catkin_make -DCMAKE_BUILD_TYPE=Release
source devel/setup.bash
roslaunch lidar_localizer ndt_mapping.launch
```

Running image with save map, 0-1 means filter rate, 0 means save all points, normally we will save 0.02-0.1 based how many points in the map

```bash
# open another terminal
source devel/setup.bash
rosservice call /save_map '/home/ryan/ndt_pcd/ndt_output.pcd' 0.0
rosservice call /save_map '/home/ryan/ndt_pcd/ndt_output.pcd' 0.2 # save around 20cm filter voxel
```