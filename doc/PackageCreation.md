# Roskeycar_msgs

```
cd catkin_ws/src
catkin_create_pkg roskeycar_msgs rospy geometry_msgs std_msgs sensor_msgs
```

# Roskeycar

```
cd catkin_ws/src
catkin_create_pkg roskeycar rospy geometry_msgs std_msgs sensor_msgs roskeycar_msgs
```

# Using Rsync

## Pulling

```
rsync -av ubuntu@10.42.0.1:/home/ubuntu/catkin_ws/src/roskeycar .
rsync -av ubuntu@10.42.0.1:/home/ubuntu/catkin_ws/src/roskeycar_msgs .
```

## Pushing

```
rsync -av roskeycar ubuntu@10.42.0.1:/home/ubuntu/catkin_ws/src/
rsync -av ros-i2cpwmboard ubuntu@10.42.0.1:/home/ubuntu/catkin_ws/src/
rsync -av roshello ubuntu@10.42.0.1:/home/ubuntu/catkin_ws/src/
rsync -av roskeycar_msgs ubuntu@10.42.0.1:/home/ubuntu/catkin_ws/src
```