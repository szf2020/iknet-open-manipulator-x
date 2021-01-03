# IKNet: Inverse kinematics neural networks for Open Manipulator X

## Data collection

```shell
$ ros2 launch open_manipulator_x_controller open_manipulator_x_controller.launch.py
```

```shell
$ ros2 service call /set_actuator_state open_manipulator_msgs/srv/SetActuatorState
```

```shell
$ ros2 topic echo --csv /kinematics_pose > kinematics_pose.csv & \
  ros2 topic echo --csv /joint_states > joint_states.csv
```

```shell
$ sed -i "1s/^/sec,nanosec,frame_id,position_x,position_y,position_z,orientation_x,orientation_y,orientation_z,orientation_w,max_accelerations_scaling_factor,max_velocity_scaling_factor,tolerance\n/" kinematics_pose.csv
$ sed -i "1s/^/sec,nanosec,frame_id,name0,name1,name2,name3,name4,position0,position1,position2,position3,position4,velocity0,velocity1,velocity2,velocity3,velocity4,effort0,effort1,effort2,effort3,effort4\n/" joint_states.csv
```

### Demo

[![IKNet data collection with Open Manipulator X](https://img.youtube.com/vi/dsHGYwkQ5Ag/0.jpg)](https://www.youtube.com/watch?v=dsHGYwkQ5Ag)

## Training

```shell
$ python3 train_iknet.py --help
usage: train_iknet.py [-h] [--kinematics-pose-csv KINEMATICS_POSE_CSV]
                      [--joint-states-csv JOINT_STATES_CSV] [--train-val-ratio TRAIN_VAL_RATIO]
                      [--batch-size BATCH_SIZE] [--epochs EPOCHS] [--lr LR] [--save-model]

optional arguments:
  -h, --help            show this help message and exit
  --kinematics-pose-csv KINEMATICS_POSE_CSV
  --joint-states-csv JOINT_STATES_CSV
  --train-val-ratio TRAIN_VAL_RATIO
  --batch-size BATCH_SIZE
  --epochs EPOCHS
  --lr LR
  --save-model
```

### Demo

```shell
$ python3 train_iknet.py
epoch       iteration   train/loss  lr          val/loss
1           3           0.0168781   0.01        0.013123
2           6           0.0158434   0.01        0.012754
3           9           0.01492     0.01        0.0126269
...
64          192         0.00221506  0.01        0.0045823
65          195         0.00218555  0.01        0.00454468
66          198         0.00216918  0.01        0.00459203
```

## Test

```shell
$ python3 test_iknet.py --help
usage: test_iknet.py [-h] [--kinematics-pose-csv KINEMATICS_POSE_CSV]
                     [--joint-states-csv JOINT_STATES_CSV] [--batch-size BATCH_SIZE]

optional arguments:
  -h, --help            show this help message and exit
  --kinematics-pose-csv KINEMATICS_POSE_CSV
  --joint-states-csv JOINT_STATES_CSV
  --batch-size BATCH_SIZE
```

### Demo

```shell
$ python3 test_iknet.py
Total loss = 0.0087115159034729
```
