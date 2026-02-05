快速调试

基础操作步骤

1. 开机与连接

1. 按下底盘后部电源按钮

2. 连接机器人：

ssh yuanyou@192.168.31.230 #密码：a

2. 建立地图

1. 启动雷达

cd yuanyou_ws
source ./devel/setup.bash
roslaunch rslidar_sdk start.launch #启动雷达数据
roslaunch nav_pkg 3dmapping_rslidar.launch #对雷达数据进行处理后使用

2. 地图建立

cd fast_lio
source devel/setup.bash
roslaunch fast_lio mapping_robosense.launch

地图建完之后，pcd文件存放在/home/yuanyou/fast_lio/src/PCD

3. 记录点位

3. 导航与重定位

1. 启动雷达

cd yuanyou_ws
source ./devel/setup.bash
roslaunch rslidar_sdk start.launch

2. 启动底盘

cd yuanyou_ws
source ./devel/setup.bash
roslaunch nav_pkg wheel_odom_to_odom.launch

3. 启动导航

cd yuanyou_ws
source ./devel/setup.bash
roslaunch nav_pkg final_navigation.launch

4. 启动重定位

cd yuanyou_ws
source ./devel/setup.bash
roslaunch hdl_localization hdl_localization.launch

4. 升降机

1. 启动升降机

cd yuanyou_ws
source ./devel/setup.bash
roslaunch uart bringup.launch

2. 上升

roslaunch uart up.launch

3. 下降

roslaunch uart down.launch

基础控制

1. ⚠️ 使用前请确保：

- 了解急停按钮位置

- 如遇异常立即按下红色急停按钮

- 控制机器人移动时请注意速度

2. 遥控器控制教程

- 开机：

  1. 开关A,B,C,D必须都处于上拨状态

  2. 同时按下遥控器上两边的电源开关开启遥控器

- 关机

  1. 同时按下遥控器背后的按键1和按键2

  2. 同时按下遥控器上两边的电源开关关闭遥控器

- 控制模式

 1. 将开关BD拨到最下档进入控制模式

  2. 遥控器的左摇杆的上下拨动控制车体的前进和后退，左右拨动控制车体的左右转向（控制时需要缓慢推动)

  3. 遥控器的右摇杆的左右拨动控制车体的逆时针和顺时针原地转动

- 驻车模式

  1. 将开关C拨到最下档进入X驻车模式

配置文件说明

1. 导航控制参数配置文件说明

1. 局部路径规划配置文件路径：

/home/yuanyou/yuanyou2_ws/src/eband_local_planner/config/eband_local_planner_params.yaml

主要参数及说明

# 机器人的最大线速度 (m/s)
max_vel_lin: 0.75
# 机器人的最大角速度 (rad/s)
max_vel_th: 1.0

# 机器人的最小线速度 (m/s)
min_vel_lin: -0.1
# 机器人的最小角速度 (rad/s)
min_vel_th: 0.0

# 原地旋转时的最小角速度 (rad/s)
min_in_place_vel_th: 0.2
# 原地旋转时的线速度阈值 (通常为0)
in_place_trans_vel: 0.0

# 最大线加速度 (m/s^2)
max_translational_acceleration: 0.4
# 最大角加速度 (rad/s^2)
max_rotational_acceleration: 0.6

# 到达目标点的 XY 距离容差 (米)
xy_goal_tolerance: 0.15
# 到达目标点的角度容差 (弧度)
yaw_goal_tolerance: 0.15

2. 局部代价地图配置文件路径：

/home/yuanyou/yuanyou2_ws/src/nav_pkg/config/local_costmap_params.yaml

主要参数说明

#更新频率
update_frequency: 15.0
#发布频率
publish_frequency: 10.0

#机器人大小
footprint: [[0.30, 0.25], [-0.30, 0.25], [-0.30, -0.25], [0.30, -0.25]]

#障碍物检测高度范围
max_obstacle_height: 1.0
min_obstacle_height: -0.3

#传感器检测障碍物的最大有效距离
obstacle_range: 3.5
#清理已通过区域的"幽灵"障碍物的范围
raytrace_range: 4.0

# 膨胀半径（米）
inflation_radius: 0.38 
# 代价缩放因子
cost_scaling_factor: 3.5

3. 全局代价地图配置文件路径：

/home/yuanyou/yuanyou2_ws/src/nav_pkg/config/global_costmap_params.yaml

主要参数说明

# 全局地图更新频率（Hz）
update_frequency: 1
# 地图可视化发布频率（Hz）           
publish_frequency: 1       

#机器人大小
footprint: [[0.30, 0.25], [-0.30, 0.25], [-0.30, -0.25], [0.30, -0.25]]

#传感器检测障碍物的最大有效距离
obstacle_range: 4.0
#清理已通过区域的"幽灵"障碍物的范围
raytrace_range: 5.0

# 膨胀半径（米）
inflation_radius: 1.2 
# 代价缩放因子
cost_scaling_factor: 10.0

2. 重定位参数配置文件说明

1. 重定位参数配置文件路径：

/home/yuanyou/yuanyou2_ws/src/location/src/hdl_global_localization/config/ransac_config.yaml

主要参数说明

ransac/voxel_based: True # 是否使用体素化方法进行点云降采样和预处理
ransac/max_iterations: 1000000 # RANSAC算法最大迭代次数，保证足够多的采样机会
ransac/matching_budget: 5000 # 匹配预算，限制参与匹配的点云点数，控制计算复杂度
ransac/max_correspondence_distance: 0.5 # 最大对应点距离阈值（单位：米），收紧匹配标准。原来0.5甚至1.0太宽容了，哪怕稍微歪一点它也算对。0.3要求必须严丝合缝才能算内点。
ransac/similarity_threshold: 0.7 # 相似度阈值，用于判断两个特征是否匹配
ransac/correspondence_randomness: 2 # 对应点随机性参数，控制RANSAC采样的随机性程度
ransac/inlier_fraction: 0.60 # 内点比例阈值（及格线）。重合点比例必须达到多少才算成功的配准？0.6表示至少60%的点是内点

ransac/x_min: -10000.0 # X轴最小平移量，基本无限制
ransac/x_max: 10000.0 # X轴最大平移量
ransac/y_min: -10000.0 # Y轴最小平移量
ransac/y_max: 10000.0 # Y轴最大平移量
ransac/z_min: -0.5 # Z轴最小平移量，-0.5米（通常限制垂直方向变化）
ransac/z_max: 0.5 # Z轴最大平移量，+0.5米（地面车辆通常垂直变化很小）

ransac/roll_min: -0.2 # 滚转角最小限制，约-11.46度（限制侧倾）
ransac/roll_max: 0.2 # 滚转角最大限制，约+11.46度
ransac/pitch_min: -0.2 # 俯仰角最小限制，约-11.46度（限制俯仰）
ransac/pitch_max: 0.2 # 俯仰角最大限制，约+11.46度
ransac/yaw_min: -3.15 # 偏航角最小限制，约-180.5度（基本无限制，允许大角度旋转）
ransac/yaw_max: 3.15 # 偏航角最大限制，约+180.5度

3. 导航点位参数配置文件说明

1. 点位配置文件路径：

/home/yuanyou/yuanyou2_ws/src/simple_points/config/points.yaml

参数说明：

# 导航点位数据库
# 格式: 标签名: {x: 米, y: 米, yaw: 弧度}
# yaw: 0=东, 1.57=北, 3.14=西, -1.57=南
# 通俗方向,角度 (Degrees),填入 YAML 的弧度 (Radians),说明
# 正东,       0°,   0.0,     X轴正方向
# 东北 (斜),  45°,  0.785,  π/4
# 正北,       90°,  1.57, π/2
# 西北 (斜),  135°, 2.356, 3π/4
# 正西,       180°, 3.14, π
# 西南 (斜), -135° (或 225°), -2.356, −3π/4
# 正南,      -90° (或 270°), -1.57, −π/2
# 东南 (斜), -45° (或 315°), -0.785, −π/4
# 1. 基础点位数据库 (物理坐标)
waypoints:
  boli:
    x: 11.2
    y: -1.98
    yaw: 0.0

  zuozi:
    x: 0.8
    y: -5.14
    yaw: 3.14

  menkou:
    x: 3.58
    y: -2.05
    yaw: 0.0
  
  Home:
    x: 0.8
    y: -0.03
    yaw: 0.0

  test:
    x: 1.92
    y: -5.09
    yaw: 0.0

# 2. 预定义路线 (逻辑组合)
# 格式: 路线名: [点位1, 点位2, 点位3...]
routes:  # 路线1:  (去A再去B)
  Morning_Patrol: [zuozi,boli,Home]
  test: [test]
  # 路线2: 送餐任务 (去厨房再回来)
  Food_Delivery: [Kitchen, Home]  
  # 路线3: 往返跑测试
  Test_Loop: [Point_A, Point_B, Point_A, Point_B, Home]

2. 导航点位配置路径

/home/yuanyou/yuanyou2_ws/src/simple_points/launch/patrol.launch

参数修改说明

#修改default即可修改导航任务，具体点位在points.yaml中
<arg name="task" default="Morning_Patrol" />

4. 升降机控制参数配置文件说明
