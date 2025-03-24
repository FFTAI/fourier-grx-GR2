---
layout: default
title: 参考指南
nav_order: 3
---

# 参考指南

## Fourier-GRX 接口协议 [user 接口]

Fourier-GRX user 接口使用 zenoh 进行通信，zenoh 是一个分布式系统的数据共享和协作平台 (https://zenoh.io/)。

针对机器人开发，Fourier-GRX 提供了以下接口 key：

- `fourier-grx/dynalink_interface/task/server`
    - 用于接收任务指令，主要用于获取机器人当前的任务模式
- `fourier-grx/dynalink_interface/grx/server`
    - 用于接收 GRX 机器人更细节的状态信息
- `fourier-grx/dynalink_interface/task/client`
    - 用于发送任务指令给机器人，主要用于切换不同的任务模式
- `fourier-grx/dynalink_interface/grx/client`
    - 用于进行 GRX 机器人不同任务更细节的控制参数设置。
    - 例如：设置机器人的行走指令参数等。

### task/server 接口协议 (用户接收)

key 说明列表：

| key                    | 说明       | 数据类型 | 具体描述                                                                                             |
|------------------------|----------|------|--------------------------------------------------------------------------------------------------|
| `flag_task_in_process` | 任务是否在进行中 | bool | 0: 任务未开始或已结束，1: 任务进行中                                                                            |
| `robot_task_state`     | 机器人任务状态  | int  | 当前机器人任务状态，如果设置了 task/client 中的 `flag_task_command_update` 为 True，会更新此值，更新值为 `robot_task_command` |
| `robot_task_substate`  | 机器人任务子状态 | int  |                                                                                                  |

### grx/server 接口协议 (用户接收)

key 说明列表：

| key                        | 说明       | 数据类型 | 具体描述                      |
|----------------------------|----------|------|---------------------------|
| `robot_error_codes`        | 机器人错误码   | int  | 机器人错误码，0: 无错误，其他: 具体错误码   |
| `robot_battery_percentage` | 机器人电量百分比 | int  | 机器人电量百分比，0-100            |
| `robot_charging_level`     | 机器人电量等级  | int  | 1为低电量红色 2为中等电量黄色 3为较多电量绿色 |
| `robot_charging_state`     | 机器人充电状态  | int  | 0: 未充电，1: 正在充电            |

### task/client 接口协议 (用户发送)

key 说明列表：

| key                        | 说明         | 数据类型 | 具体描述            |
|----------------------------|------------|------|-----------------|
| `flag_task_command_update` | 任务指令请求更新标志 | bool | 0: 不更新，1: 更新    |
| `robot_task_command`       | 任务指令       | int  | 机器人任务指令，具体指令见下表 |

机器人任务指令列表：

- 任务指令通过 fourier_grx.TaskCommand 枚举类定义，具体定义如下：
- 具体任务值可能会根据机器人的不同，或 `fourier-grx` 版本的不同而有所变化，具体值以实际为准。

| 任务指令             | 任务值  | 任务描述                            |
|------------------|------|---------------------------------|
| TASK_SERVO_OFF   | 36   | 机器人全关节下电失能                      |
| TASK_SERVO_ON    | 35   | 机器人全关节上电使能                      |
| TASK_CLEAR_FAULT | 34   | 清除机器人全关节报警                      |
| TASK_SET_HOME    | 3000 | 设置机器人全关节零位位置为当前位置               |
| TASK_TEST_JOINT  | 3003 | 机器人关节运动功能测试，用于检测机器人关节是否能够正常运动   |
| TASK_READY_STATE | 3011 | 机器人运行到 **准备状态**，为微曲膝关节的站立姿态     |
| TASK_RL_WALK     | 3530 | 机器人运动到 **行走状态**，可以用手柄控制机器人行走    |
| TASK_RL_WALK_RUN | 3542 | 机器人运动到 **跑步状态**，可以用手柄控制机器人行走/跑步 |

### grx/client 接口协议 (用户发送)

key 说明列表：

- 为规范用户的高层控制指令传入，对输入参数进行了封装和抽象。
  主要定义了 “虚拟摇杆 (virtual joystick)” 和 “虚拟遥操作手柄 (virtual teleoperation)” 的概念来进行机器人的控制。
- 在需要使用到对应虚拟设备时，需要在主程序启动时调用的 `config_xxx.yaml` 文件中
  设置 `peripheral/use_virtual_joystick` 或 `peripheral/use_virtual_teleoperation` 为 True。

| key                                       | 说明             | 数据类型                                               | 具体描述             |
|-------------------------------------------|----------------|----------------------------------------------------|------------------|
| `virtual_joystick_button_up`              | 虚拟手柄上按钮状态      | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_down`            | 虚拟手柄下按钮状态      | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_left`            | 虚拟手柄左按钮状态      | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_right`           | 虚拟手柄右按钮状态      | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_l1`              | 虚拟手柄 L1 按钮状态   | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_l2`              | 虚拟手柄 L2 按钮状态   | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_r1`              | 虚拟手柄 R1 按钮状态   | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_button_r2`              | 虚拟手柄 R2 按钮状态   | int                                                | 0: 未按下，1: 按下     |
| `virtual_joystick_axis_left`              | 虚拟手柄左摇杆状态      | array(int, int)                                    | 摇杆状态值范围为 [-1, 1] |
| `virtual_joystick_axis_right`             | 虚拟手柄右摇杆状态      | array(int, int)                                    | 摇杆状态值范围为 [-1, 1] |
| ==========                                | ==========     | ==========                                         | ==========       |
| `virtual_teleoperation_left_handle_pose`  | 虚拟遥操作手柄左手柄姿态   | array(float, float, float,float,float,float,float) |                  |
| `virtual_teleoperation_right_handle_pose` | 虚拟遥操作手柄右手柄姿态   | array(float, float, float,float,float,float,float) |                  |
| `virtual_teleoperation_button_left`       | 虚拟遥操作手柄左手柄按钮状态 | int                                                | 0: 未按下，1: 按下     |
| `virtual_teleoperation_button_right`      | 虚拟遥操作手柄右手柄按钮状态 | int                                                | 0: 未按下，1: 按下     |

--------------------------------------------

## Fourier-GRX 接口协议 [developer 接口]

### 状态字典（state dict）

Fourier-GRX developer 接口使用状态字典（state dict）返回机器人当前的状态信息，状态字典的 key 和 value 如下：

| key                    | 说明               | 数据类型                           |
|------------------------|------------------|--------------------------------|
| `imu_quat`             | 机器人 IMU 的四元数姿态信息 | array(float,float,float,float) |
| `imu_euler_angle`      | 机器人 IMU 的欧拉角姿态信息 | array(float, float, float)     |
| `imu_angular_velocity` | 机器人 IMU 的角速度信息   | array(float, float, float)     |
| `imu_acceleration`     | 机器人 IMU 的线加速度信息  | array(float, float, float)     |
| `joint_position`       | 机器人关节的位置信息       | array(float * num_of_joints)   |
| `joint_velocity`       | 机器人关节的速度信息       | array(float * num_of_joints)   |
| `joint_kinetic`        | 机器人关节的力矩信息       | array(float * num_of_joints)   |

### 控制字典（control dict）

Fourier-GRX developer 接口使用控制字典（control dict）发送机器人的控制指令，控制字典的 key 和 value 如下：

| key                   | 说明                | 数据类型                         | 具体描述                                             |
|-----------------------|-------------------|------------------------------|--------------------------------------------------|
| `control_mode`        | 机器人的控制模式          | int (float * num_of_joints)  | 0: 无控制，1: 电流控制，2: 力矩控制，3: 速度控制，4: 位置控制, 6: PD 控制 |
| `position`            | 机器人关节的位置指令        | array(float * num_of_joints) | 单位: deg                                          |
| `velocity`            | 机器人关节的速度指令        | array(float * num_of_joints) | 单位: deg/s                                        |
| `effort`              | 机器人关节的力矩指令        | array(float * num_of_joints) | 单位: Nm                                           |
| `current`             | 机器人关节的电流指令        | array(float * num_of_joints) | 单位: A                                            |
| `position_control_kp` | 机器人关节位置控制的 P 系数   | array(float * num_of_joints) |                                                  |
| `velocity_control_kp` | 机器人关节速度控制的 P 系数   | array(float * num_of_joints) |                                                  |
| `velocity_control_ki` | 机器人关节速度控制的 I 系数   | array(float * num_of_joints) |                                                  |
| `pd_control_kp`       | 机器人关节 PD 控制的 P 系数 | array(float * num_of_joints) |                                                  |
| `pd_control_kd`       | 机器人关节 PD 控制的 D 系数 | array(float * num_of_joints) |                                                  |
