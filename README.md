# rtab-map_3d
use tutrlebot-waffle. use 3d liadr

# install
https://github.com/introlab/rtabmap_ros

backup waffle!!
```
cd /opt/ros/humble/share/turtlebot3_gazebo/models/turtlebot3_waffle
sudo cp model.sdf model.sdf.original_backup
sudo gedit model.sdf
```
waffle change to 
```
<sensor name="camera" type="depth">
        <always_on>true</always_on>
        <visualize>true</visualize>
        <update_rate>30</update_rate>
        <camera>
          <horizontal_fov>1.02974</horizontal_fov>
          <image>
            <width>640</width>
            <height>480</height>
            <format>R8G8B8</format>
          </image>
          <clip>
            <near>0.1</near>
            <far>10.0</far>
          </clip>
        </camera>
          <plugin name="camera_controllor" filename="libgazebo_ros_camera.so">
            <ros>
              <namespace></namespace> <remapping>image_raw:=image_raw</remapping>
              <remapping>camera_info:=camera_info</remapping>
              <remapping>depth/image_raw:=depth/image_raw</remapping>
            </ros>
            <min_depth>0.1</min_depth>
            <max_depth>10.0</max_depth>
          </plugin>
      </sensor>
    </link>
```

change 2d to 3d !! 
find base_scan
```
        <link name="base_scan">
          <inertial>
            <pose>-0.052 0 0.111 0 0 0</pose>
            <inertia>
              <ixx>0.001</ixx>
              <ixy>0.000</ixy>
              <ixz>0.000</ixz>
              <iyy>0.001</iyy>
              <iyz>0.000</iyz>
              <izz>0.001</izz>
            </inertia>
            <mass>0.114</mass>
          </inertial>
    
          <collision name="lidar_sensor_collision">
            <pose>-0.052 0 0.111 0 0 0</pose>
            <geometry>
              <cylinder>
                <radius>0.0508</radius>
                <length>0.055</length>
              </cylinder>
            </geometry>
          </collision>
    
          <visual name="lidar_sensor_visual">
            <pose>-0.064 0 0.121 0 0 0</pose>
            <geometry>
              <cylinder>
                <radius>0.05</radius>
                <length>0.07</length>
              </cylinder>
            </geometry>
            <material>
              <ambient>0.5 0.5 0.5 1.0</ambient> <diffuse>0.5 0.5 0.5 1.0</diffuse>
            </material>
          </visual>
    
          <sensor name="velodyne-VLP16" type="ray">
            <visualize>true</visualize>
            <update_rate>10</update_rate>
            <ray>
              <scan>
                <horizontal>
                  <samples>440</samples>
                  <resolution>1</resolution>
                  <min_angle>-3.141592653589793</min_angle>
                  <max_angle>3.141592653589793</max_angle>
                </horizontal>
                <vertical>
                  <samples>16</samples>
                  <resolution>1</resolution>
                  <min_angle>-0.261799</min_angle>
                  <max_angle>0.261799</max_angle>
                </vertical>
              </scan>
              <range>
                <min>0.3</min>
                <max>100.0</max>
                <resolution>0.001</resolution>
              </range>
            </ray>
            <plugin name="gazebo_ros_laser_controller" filename="libgazebo_ros_velodyne_laser.so">
              <ros>
                <namespace>/velodyne</namespace>
                <remapping>~/out:=/points_raw</remapping>
              </ros>
              <frame_name>velodyne_frame</frame_name>
              <min_range>0.9</min_range>
              <max_range>130.0</max_range>
              <gaussian_noise>0.008</gaussian_noise>
            </plugin>
          </sensor>
        </link>
```

# how to use
T1
```
export TURTLEBOT3_MODEL=waffle
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
T2
```
ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 0.12 --yaw 0 --pitch 0 --roll 0 --frame-id base_link --child-frame-id velodyne_frame --ros-args -p use_sim_time:=true
```
T3
```
ros2 launch rtabmap_launch rtabmap.launch.py     rtabmap_viz:=false     use_sim_time:=true     args:="-d --DeleteDbOnStart --Reg/Strategy 1 --Reg/Force3DoF true \
           --Grid/MaxObstacleHeight 1.0 \
           --Grid/MinGroundHeight 0.1 \
           --Grid/RangeMax 10.0 \
           --Grid/RayTracing true"     visual_odometry:=false     icp_odometry:=false     odom_topic:=/odom     subscribe_depth:=true     subscribe_rgb:=true     subscribe_scan:=false     subscribe_scan_cloud:=true     scan_cloud_topic:=/points_raw     rgb_topic:=/camera/image_raw     depth_topic:=/camera/depth/image_raw     camera_info_topic:=/camera/camera_info     frame_id:=base_footprint     approx_sync:=true     wait_imu_to_init:=false     qos:=2
```
T3
```
export TURTLEBOT3_MODEL=waffle
ros2 run turtlebot3_teleop teleop_keyboard
```
# demo 
<img width="1210" height="718" alt="螢幕擷取畫面 2026-01-07 162934" src="https://github.com/user-attachments/assets/1abda1d6-9ead-4bd7-a260-8095defbc84d" />





