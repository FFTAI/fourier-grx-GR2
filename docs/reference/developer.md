---
layout: default
title: Developer 接口
nav_order: 3.2
parent: 参考指南
has_toc: true
---

# 参考指南 Developer 接口

* TOC
  {:toc}

developer 接口是针对开发者底层开发提供的二次开发接口。需要在 python 环境中安装 fourier_core 和 fourier_grx 库后，直接使用库函数进行开发调用。

## 状态字典（state dict）

Fourier-GRX developer 接口使用状态字典（state dict）返回机器人当前的状态信息，状态字典的 key 和 value 如下：

| key                    | 说明               | 数据类型                           | 具体描述             |
|------------------------|------------------|--------------------------------|------------------|
| `imu_quat`             | 机器人 IMU 的四元数姿态信息 | array(float,float,float,float) | x, y, z, w       |
| `imu_euler_angle`      | 机器人 IMU 的欧拉角姿态信息 | array(float, float, float)     | roll, pitch, yaw |
| `imu_angular_velocity` | 机器人 IMU 的角速度信息   | array(float, float, float)     | roll, pitch, yaw |
| `imu_acceleration`     | 机器人 IMU 的线加速度信息  | array(float, float, float)     | x, y, z          |
| `joint_position`       | 机器人关节的位置信息       | array(float * num_of_joints)   | 参考机器人关节次序        |
| `joint_velocity`       | 机器人关节的速度信息       | array(float * num_of_joints)   | 参考机器人关节次序        |
| `joint_kinetic`        | 机器人关节的力矩信息       | array(float * num_of_joints)   | 参考机器人关节次序        |

## 控制字典（control dict）

Fourier-GRX developer 接口使用控制字典（control dict）发送机器人的控制指令，控制字典的 key 和 value 如下：

| key                   | 说明                | 数据类型                         | 具体描述                                             |
|-----------------------|-------------------|------------------------------|--------------------------------------------------|
| `control_mode`        | 机器人的控制模式          | int (float * num_of_joints)  | 0: 无控制，1: 电流控制，2: 力矩控制，3: 速度控制，4: 位置控制, 6: PD 控制 |
| `position`            | 机器人关节的位置指令        | array(float * num_of_joints) | 单位: deg                                          |
| `velocity`            | 机器人关节的速度指令        | array(float * num_of_joints) | 单位: deg/s                                        |
| `effort`              | 机器人关节的力矩指令        | array(float * num_of_joints) | 单位: Nm                                           |
| `current`             | 机器人关节的电流指令        | array(float * num_of_joints) | 单位: A                                            |
| `position_control_kp` | 机器人关节位置控制的 P 系数   | array(float * num_of_joints) | 参考机器人关节次序                                        |
| `velocity_control_kp` | 机器人关节速度控制的 P 系数   | array(float * num_of_joints) | 参考机器人关节次序                                        |
| `velocity_control_ki` | 机器人关节速度控制的 I 系数   | array(float * num_of_joints) | 参考机器人关节次序                                        |
| `pd_control_kp`       | 机器人关节 PD 控制的 P 系数 | array(float * num_of_joints) | 参考机器人关节次序                                        |
| `pd_control_kd`       | 机器人关节 PD 控制的 D 系数 | array(float * num_of_joints) | 参考机器人关节次序                                        |

## 机器人关节序列

在获取关节信息或是发送控制指令时，需要整体发送所有关节的信息。因此需要按照机器人的关节序列进行操作。

Fourier-GRX SDK 中对于关节序列的定义如下。

#### GRMini1

| 序号 | 关节名称                       | 具体描述        |
|----|----------------------------|-------------|
| 0  | left_hip_pitch_joint       | 左髋 pitch 关节 |
| 1  | left_hip_roll_joint        | 左髋 roll 关节  |
| 2  | left_hip_yaw_joint         | 左髋 yaw 关节   |
| 3  | left_knee_pitch_joint      | 左膝 pitch 关节 |
| 4  | left_ankle_roll_joint      | 左踝 roll 关节  |
| 5  | left_ankle_pitch_joint     | 左踝 pitch 关节 |
| 6  | right_hip_pitch_joint      | 右髋 pitch 关节 |
| 7  | right_hip_roll_joint       | 右髋 roll 关节  |
| 8  | right_hip_yaw_joint        | 右髋 yaw 关节   |
| 9  | right_knee_pitch_joint     | 右膝 pitch 关节 |
| 10 | right_ankle_roll_joint     | 右踝 roll 关节  |
| 11 | right_ankle_pitch_joint    | 右踝 pitch 关节 |
| 12 | waist_yaw_joint            | 腰部 yaw 关节   |
| 13 | left_shoulder_pitch_joint  | 左肩 pitch 关节 |
| 14 | left_shoulder_roll_joint   | 左肩 roll 关节  |
| 15 | left_shoulder_yaw_joint    | 左肩 yaw 关节   |
| 16 | left_elbow_pitch_joint     | 左肘 pitch 关节 |
| 17 | left_wrist_yaw_joint       | 左腕 yaw 关节   |
| 18 | right_shoulder_pitch_joint | 右肩 pitch 关节 |
| 19 | right_shoulder_roll_joint  | 右肩 roll 关节  |
| 20 | right_shoulder_yaw_joint   | 右肩 yaw 关节   |
| 21 | right_elbow_pitch_joint    | 右肘 pitch 关节 |
| 22 | right_wrist_yaw_joint      | 右腕 yaw 关节   |

#### GR2

| 序号 | 关节名称                       | 具体描述        |
|----|----------------------------|-------------|
| 0  | left_hip_pitch_joint       | 左髋 pitch 关节 |
| 1  | left_hip_roll_joint        | 左髋 roll 关节  |
| 2  | left_hip_yaw_joint         | 左髋 yaw 关节   |
| 3  | left_knee_pitch_joint      | 左膝 pitch 关节 |
| 4  | left_ankle_roll_joint      | 左踝 roll 关节  |
| 5  | left_ankle_pitch_joint     | 左踝 pitch 关节 |
| 6  | right_hip_pitch_joint      | 右髋 pitch 关节 |
| 7  | right_hip_roll_joint       | 右髋 roll 关节  |
| 8  | right_hip_yaw_joint        | 右髋 yaw 关节   |
| 9  | right_knee_pitch_joint     | 右膝 pitch 关节 |
| 10 | right_ankle_roll_joint     | 右踝 roll 关节  |
| 11 | right_ankle_pitch_joint    | 右踝 pitch 关节 |
| 12 | waist_yaw_joint            | 腰部 yaw 关节   |
| 13 | head_yaw_joint             | 头部 yaw 关节   |
| 14 | head_pitch_joint           | 头部 pitch 关节 |
| 15 | left_shoulder_pitch_joint  | 左肩 pitch 关节 |
| 16 | left_shoulder_roll_joint   | 左肩 roll 关节  |
| 17 | left_shoulder_yaw_joint    | 左肩 yaw 关节   |
| 18 | left_elbow_pitch_joint     | 左肘 pitch 关节 |
| 19 | left_wrist_yaw_joint       | 左腕 yaw 关节   |
| 20 | left_wrist_pitch_joint     | 左腕 pitch 关节 |
| 21 | left_wrist_roll_joint      | 左腕 roll 关节  |
| 22 | right_shoulder_pitch_joint | 右肩 pitch 关节 |
| 23 | right_shoulder_roll_joint  | 右肩 roll 关节  |
| 24 | right_shoulder_yaw_joint   | 右肩 yaw 关节   |
| 25 | right_elbow_pitch_joint    | 右肘 pitch 关节 |
| 26 | right_wrist_yaw_joint      | 右腕 yaw 关节   |
| 27 | right_wrist_pitch_joint    | 右腕 pitch 关节 |
| 28 | right_wrist_roll_joint     | 右腕 roll 关节  |


