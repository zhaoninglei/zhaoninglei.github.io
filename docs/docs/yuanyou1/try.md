快速调试

基础操作步骤

1. 开机与连接

1. 按下机器人背后电源按钮

2. 连接机器人：

ssh yuanyou@192.168.10.120 #密码：a

2. 代码仓库

1. 上位机代码仓库

2. 下位机代码仓库

http://gitlab.handx.cc:9999/handx-yuanyou/yuanyou-core.git

3. 启动前准备

1. 地图建立

2. 记录点位（充电点、停靠点、巡航点）

3. 机器人无线网络连接

机器人网络连接文档.md

基础控制

1. ⚠️ 使用前请确保：

- 了解急停按钮位置

- 如遇异常立即按下红色急停按钮

- 控制机器人移动时请注意速度

2. 启动下位机程序：

cd yuanyou-core/catkin_ws
source devel/setup.bash
roslaunch aiui aiui.launch 
roslaunch arm_manipulation spawn_all_actuator.launch

- 等待机器人语音发出"当前是XX模式，网络连接成功"

- 确保机械臂已经使能

3. 机器人app控制教程

- 参考快速开始机器人app遥控器控制教程

配置文件说明

1. 主控配置文件说明

2. 机械臂控制参数配置文件说明

3. 语音控制参数配置文件说明
