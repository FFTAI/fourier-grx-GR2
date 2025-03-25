---
layout: default
title: 常见问题
nav_order: 4
toc: true          # 启用目录
toc_min_header: 2  # 最小显示标题层级（如 H2）
toc_max_header: 3  # 最大显示标题层级（如 H3）
---

# 常见问题

## 机器人 initialize 失败，提示找不到 IMU 设备

![initialize_imu_error.png](/assets/images/initialize_imu_error.png)

- 检查 IMU 连接线是否正常连接到机器人主控板。

## 机器人 self-check 失败，提示无法访问指定 ip 的执行器：

![self_check_error.png](/assets/images/self_check_error.png)

- 检查机器人的执行器是否上电，是否闪烁紫色呼吸灯。
- 检查机器人的有线网口是否连接到有线网络，并且系统已正确配置为静态 IP 模式。
- 检查机器人是否存在线路连线问题。

## 如何让机器人访问外网？

- 由于机器人的 wifi 模块已被配置为自动热点模式，因此无法使用机器人连接外部 wifi 网络。
  如果需要使用外部网络，可以将机器人的有线网口连接到外部网络，然后通过有线网口访问外网。
  访问外网前需要将机器人的有线网口设置为 **动态 IP 模式（DHCP 模式）**。
  访问完成后，需要将有线网口设置为 **静态 IP 模式**，并设置 IP 地址为 `192.168.137.200`, 子网掩码为 `255.255.255.0`。
  并重启机器人的有线网络服务，或者重启机器人主控电脑。

## 如何关闭机器人的 wifi 热点自启动？让 wifi 恢复正常的手动连接模式？

- 机器人的 wifi 热点自启动由服务 rocs-wifi 自动启动，可以通过以下命令关闭 rocs-wifi 服务：

```bash
sudo systemctl stop rocs-wifi  # 只关闭一次
sudo systemctl disable rocs-wifi  # 永久关闭
```

## 机器人的控制频率是多少？

- 使用 **user** 接口时，机器人的主程序控制频率默认为 **50Hz**。状态输出发送以及外部指令接收的频率也为 **50Hz**。
- 使用 **developer** 接口时，机器人底层的数据更新频率默认为 **400Hz** （最高支持 **500Hz**），具体算法运行频率由用户自行控制，但建议不超过
  **500Hz**。

## 机器人程序报 **Timeout** 警告 ⚠️ 怎么办？

- 检查机器人的主控电脑是否有设置 ipv6 启动，如果有，建议关闭 ipv6。
- 检查机器人所在的有线局域网是否有其他电脑接入并开启了 ipv6，如果有，建议关闭 ipv6。
- 如果仍然偶发该警报，建议检查对应丢包执行器的上下连接线是否接触良好，是否有松动。

## user 测试程序跑不通，机器人控制程序没有提示收到指令信息？

- 目前 zenoh 数据发送会优先有线端口发出。
- 如果是使用外部电脑开发机器人，同时连接了外部电脑的有线接口和机器人的无线热点信号，可能导致数据发送失败。

## ImportError: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found

- (required by
  /home/gr225ja0023/miniconda3/envs/fourier-grx/lib/python3.11/site-packages/fourier_grx.cpython-311-x86_64-linux-gnu.so)
- 安装编译工具，`sudo apt install build-essentials`
- 如果仍然报错，建议升级系统到 Ubuntu 22.04 LTS，或者使用 Ubuntu 22.04 LTS 系统的开发主机。
