---
layout: default
title: User 接口
nav_order: 3.1
parent: 参考指南
---

# 参考指南 User 接口

## Fourier-GRX 接口协议 [user 接口]

Fourier-GRX user 接口使用 zenoh 进行通信，zenoh 是一个分布式系统的数据共享和协作平台 (https://zenoh.io/)。

fourier-grx 接口主要分为以下4类：

- comm：通信相关信息
- robot：机器人状态信息
- task：任务相关信息
- grx：fourier-grx 库相关信息，通用机器人相关内容（主要是人形）

对应的 zenoh 接口 topic 的 key 格式为：

- key 可以是 ["comm", "robot", "task", "grx"]
- `fourier-grx/dynalink_interface/{key}/server` ：用于发出状态信息
- `fourier-grx/dynalink_interface/{key}/client` ：用于接收指令信息

### 状态信息

#### comm/server 接口协议 (状态信息)

key 说明列表：

| key               | 说明   | 数据类型 | 具体描述      |
|-------------------|------|------|-----------|
| `flag_heart_beat` | 心跳标志 | bool | 1: 机器人已启动 |

#### robot/server 接口协议 (状态信息)

key 说明列表：

| key                             | 说明         | 数据类型                              | 具体描述                                    |
|---------------------------------|------------|-----------------------------------|-----------------------------------------|
| `sensor_usb_imus_quat_value`    | 传感器 IMU 数据 | array(float, float, float, float) | 传感器 IMU 数据，四元数表示姿态，顺序为 x, y, z, w       |
| `sensor_usb_imus_euler_value`   | 传感器 IMU 数据 | array(float, float, float)        | 传感器 IMU 数据，欧拉角表示姿态，顺序为 roll, pitch, yaw |
| `sensor_usb_imus_ang_vel_value` | 传感器 IMU 数据 | array(float, float, float)        | 传感器 IMU 数据，角速度表示姿态，顺序为 x, y, z          |
| `sensor_usb_imus_lin_acc_value` | 传感器 IMU 数据 | array(float, float, float)        | 传感器 IMU 数据，线加速度表示姿态，顺序为 x, y, z         |
| `actuator_measured_position`    | 电机位置       | array(float)                      | 电机测量位置，单位为角度                            |
| `actuator_measured_velocity`    | 电机速度       | array(float)                      | 电机测量速度，单位为角度/秒                          |
| `actuator_measured_kinetic`     | 电机力矩       | array(float)                      | 电机测量力矩，单位为牛顿米                           |
| `actuator_measured_current`     | 电机电流       | array(float)                      | 电机测量电流，单位为安培                            |
| `actuator_output_position`      | 电机位置       | array(float)                      | 电机指令位置，单位为角度                            |
| `actuator_output_velocity`      | 电机速度       | array(float)                      | 电机指令速度，单位为角度/秒                          |
| `actuator_output_kinetic`       | 电机力矩       | array(float)                      | 电机指令力矩，单位为牛顿米                           |
| `actuator_output_current`       | 电机电流       | array(float)                      | 电机指令电流，单位为安培                            |
| `joint_measured_position`       | 关节位置       | array(float)                      | 关节测量位置，单位为角度                            |
| `joint_measured_velocity`       | 关节速度       | array(float)                      | 关节测量速度，单位为角度/秒                          |
| `joint_measured_kinetic`        | 关节力矩       | array(float)                      | 关节测量力矩，单位为牛顿米                           |
| `joint_measured_current`        | 关节电流       | array(float)                      | 关节测量电流，单位为安培                            |
| `joint_output_position`         | 关节位置       | array(float)                      | 关节指���位置，单位为角度                          |
| `joint_output_velocity`         | 关节速度       | array(float)                      | 关节���令速度，单位为角度/秒                        |
| `joint_output_kinetic`          | 关节力矩       | array(float)                      | 关节指令力矩，单位为牛顿米                           |
| `joint_output_current`          | 关节电流       | array(float)                      | 关节���令电流，单位为安培                          |

#### task/server 接口协议 (状态信息)

key 说明列表：

| key                    | 说明       | 数据类型 | 具体描述                                                                                             |
|------------------------|----------|------|--------------------------------------------------------------------------------------------------|
| `flag_task_in_process` | 任务是否在进行中 | bool | 0: 任务未开始或已结束，1: 任务进行中                                                                            |
| `robot_task_state`     | 机器人任务状态  | int  | 当前机器人任务状态，如果设置了 task/client 中的 `flag_task_command_update` 为 True，会更新此值，更新值为 `robot_task_command` |
| `robot_task_substate`  | 机器人任务子状态 | int  |                                                                                                  |

#### grx/server 接口协议 (状态信息)

key 说明列表：

| key                        | 说明       | 数据类型 | 具体描述                      |
|----------------------------|----------|------|---------------------------|
| `robot_error_codes`        | 机器人错误码   | int  | 机器人错误码，0: 无错误，其他: 具体错误码   |
| `robot_battery_percentage` | 机器人电量百分比 | int  | 机器人电量百分比，0-100            |
| `robot_charging_level`     | 机器人电量等级  | int  | 1为低电量红色 2为中等电量黄色 3为较多电量绿色 |
| `robot_charging_state`     | 机器人充电状态  | int  | 0: 未充电，1: 正在充电            |

### 指令信息

#### comm/client 接口协议 (指令信息)

key 说明列表：

| key | 说明 | 数据类型 | 具体描述 |
|-----|----|------|------|
|     |    |      |      |

#### robot/client 接口协议 (指令信息)

key 说明列表：

| key | 说明 | 数据类型 | 具体描述 |
|-----|----|------|------|
|     |    |      |      |

#### task/client 接口协议 (指令信息)

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

#### grx/client 接口协议 (指令信息)

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

