---
layout: default
title: 机器人关节序列
nav_order: 3.3
parent: 参考指南
has_toc: true
---

# 机器人关节序列

* TOC
{:toc}

在获取关节信息或是发送控制指令时，需要整体发送所有关节的信息。因此需要按照机器人的关节序列进行操作。

Fourier-GRX SDK 提供的关节序列与机器人 URDF 模型文件中的关节序列一致。用户在使用时需要注意关节序列的顺序。

Fourier-GRX SDK 中对于关节序列的定义如下。

## GR2

关节数量：

- 总数：6 + 6 + 1 + 2 + 7 + 7 = 29
- 左腿：6
- 右腿：6
- 腰部：1
- 头部：2
- 左臂：7
- 右臂：7

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
