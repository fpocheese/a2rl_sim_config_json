# Autoverse 模拟器配置布尔开关变量总览

> 文件路径均相对于 `Saved/` 目录

---

## 一、`simconfig.json` — 全局仿真配置

| 变量 | 当前值 | 含义 |
|---|---|---|
| `interfaces.bEnableRos2` | `true` | 启用 ROS2 接口 |
| `interfaces.bEnableCan` | `true` | 启用虚拟 CAN 总线接口 |
| `interfaces.hmi.bEnableAxisDebugPrints` | `false` | 启用方向盘/踏板等轴输入的调试打印输出 |
| `jedi.bEnableHyperdrive` | `false` | 启用超速仿真加速模式（仿真跑得比实时快） |
| `jedi.bIsLockStepEnabled` | `false` | 启用锁步模式（仿真每步等待外部确认后再继续，用于离线评估） |
| `jedi.bEnablePhysicsRatePublisher` | `true` | 发布 `/sim_physics_rate` 话题（物理帧率监控） |
| `bEnableSplitscreen` | `true` | 启用分屏显示（多车同屏视角） |
| `bDisableAllHuds` | `false` | 禁用所有 HUD 界面覆盖显示 |
| `bHideAutonomaBranding` | `true` | 隐藏 Autonoma 品牌标识 |
| `bEnableV2VCollision` | `true` | 启用车对车（V2V）碰撞检测 |

---

## 二、`Objects/Eav24_default.json` — 车辆对象配置

### GroundTruthSensor（真值传感器）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnabled` | `true` | 启用真值传感器，发布 `/flyeagle/ground_truth` 和 `/flyeagle/v2v_ground_truth` |
| `bUseOpponentRelativeRotation` | `false` | 对手真值的旋转坐标系：`false`=世界坐标，`true`=相对本车坐标 |

### WorldTextComponent（车辆头顶文字标签）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnabled` | `true` | 启用车顶 3D 文字显示 |
| `bUseObjectName` | `true` | 显示对象名称（"flyeagle"）；`false` 则显示 `defaultString` 的内容 |

### WheeledVehicleReplayIpd（轨迹录制 / 回放）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnabled` | `true` | 启用录制/回放系统 |
| `bEnableRecording` | `true` | 启用行驶轨迹录制 |
| `bEnableReplay` | `false` | 启用轨迹回放（让车自动沿录制路径行驶） |
| `bApplyInitialConfig` | `false` | 启动时自动应用 `initialConfig` 中的回放参数 |
| `bRecordSingleLap` | `true` | 只录制单圈，`false` 则持续录制 |
| `bUseRandomProfile` | `false` | 随机选择回放轨迹文件 |
| `bStartReplayAtRandomTime` | `false` | 从轨迹的随机时间点开始回放 |

### WheeledVehicleAutopilot（自动巡航）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnabled` | `false` | 启用自动巡航（沿预设路径点 `CiRoute` 自动驾驶，无需外部控制指令） |

### LwiIpd（轻量级外部控制接口）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnableControl` | `false` | 允许通过此 UDP 接口控制车辆（端口 30602/30603） |

### VehicleLaneMonitorIpd（赛道监控）⭐

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bPublishLaps` | `true` | **发布圈数/圈速信息**（含分段计时 Sector） |
| `bPublishLateralLanePercent` | `true` | **发布车辆在赛道横向位置百分比**（0=左边界，1=右边界） |
| `bTickWithPhysics` | `true` | 与物理引擎同步更新；`false` 则与渲染帧同步 |
| `bDetectBounds` | `true` | **检测赛道边界越界**（出界事件） |

### Eav24Initializer（车辆核心初始化）⭐

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnablePrimaryCan` | `true` | 启用主 CAN 总线（`flyeaglevcan0`） |
| `bEnableSecondaryCan` | `true` | 启用副 CAN 总线（`flyeaglevcan1`） |
| `bHudEnabled` | `true` | 启用车辆仪表 HUD 显示 |
| `bEnableSound` | `true` | 启用引擎声音效果 |
| `bRenderHudInWorld` | `false` | 将 HUD 渲染为世界中的 3D 对象；`false` 为屏幕叠加层 |
| `bLidarEnabled` | `true` | 启用三个激光雷达（front/left/right），发布 `/flyeagle/lidar_*/points` |
| `bCameraEnabled` | `false` | **启用 7 个相机**（front_left/right, lateral_left/right, rear_left/right, back），发布 20Hz 图像 |
| `bRadarEnabled` | `false` | **启用 4 个毫米波雷达**（front/left/right/rear），发布 `/flyeagle/radar_*/points` @16.67Hz |
| `bPublishInputs` | `false` | **发布车辆控制输入话题**（油门/刹车/转向当前指令值） |
| `bPublishGroundTruth` | `true` | 发布车辆真值话题（`/flyeagle/ground_truth`） |

### WheeledVehicleInitializer — steering（转向系统）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bIsPhysicalActuator` | `true` | 转向模拟物理执行器特性（带宽 20Hz，速率 1000 限制）；`false` 则指令瞬时生效 |

### WheeledVehicleInitializer — powertrain（动力总成）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bHasRearSolidAxel` | `false` | 后轴是否为刚性整体桥；`false` = 独立悬挂 |

### WheeledVehicleInitializer — 顶层

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bUseTireThermalModel` | `true` | **启用轮胎热模型**（轮胎温度影响抓地力，初始温度 21°C，映射曲线：70-100°C 最佳，超出则摩擦系数下降） |
| `bPhysicalBreakActuator` | `true` | 刹车模拟物理执行器特性（带宽 20Hz，速率 6000 限制）；`false` 则刹车指令瞬时生效 |

### VectornavIpd — 各数据流（namedDataStreams）

| 数据流 | `bEnabled` | ROS2 话题 | 频率 |
|---|---|---|---|
| `CommonGroup` | `true` | `/flyeagle/vectornav/raw/common` | 100 Hz |
| `GpsGroup` | `true` | `/flyeagle/vectornav/raw/gps` | 100 Hz |
| `Gps2Group` | `true` | `/flyeagle/vectornav/raw/gps2` | 100 Hz |
| `ImuGroup` | `true` | `/flyeagle/vectornav/raw/imu` | 250 Hz |
| `AttitudeGroup` | `true` | `/flyeagle/vectornav/raw/attitude` | 100 Hz |
| `Tii` | `true` | `/flyeagle/a2rl/vn/ins` | 100 Hz |
| `NavSatFix` | `true` | `/flyeagle/vectornav/nav_sat_fix` | 100 Hz |

### SimViewTargetIpd（观察视角相机）

| 变量 | 当前值 | 含义 |
|---|---|---|
| `bEnableCollisionEffects` | `true` | 碰撞时启用视角抖动等视觉效果 |
| `bEnableTranslationLag` | `false` | 相机跟随时启用位置延迟（更平滑/电影化） |
| `bEnableRotationLag` | `false` | 相机跟随时启用旋转延迟 |
| `bEnableShake`（Cam0） | `true` | 启用速度相关的相机抖动效果 |

---

## 三、`Payloads/wvp.json` — 车辆物理参数模板（默认基准）

> ⚠️ 此文件是模板参考值，实际以 `Eav24_default.json` 中的配置为准

| 变量 | 模板值 | 与 Eav24_default 对比 |
|---|---|---|
| `steering.bIsPhysicalActuator` | `false` | ⚠️ 不同！Eav24_default 中为 `true` |
| `bUseTireThermalModel` | `true` | 相同 |
| `bPhysicalBreakActuator` | `true` | 相同 |

---

## 四、快速参考：可开启的隐藏功能

| 文件 | 变量 | 改为 | 效果 |
|---|---|---|---|
| `Objects/Eav24_default.json` | `bCameraEnabled` | `true` | 获得 7 路相机图像话题 |
| `Objects/Eav24_default.json` | `bRadarEnabled` | `true` | 获得 4 路雷达点云话题 |
| `Objects/Eav24_default.json` | `bPublishInputs` | `true` | 获得车辆控制输入话题 |
| `Objects/Eav24_default.json` | `bPublishLaps` | `true` | 获得圈速/圈数话题 |
| `Objects/Eav24_default.json` | `bPublishLateralLanePercent` | `true` | 获得赛道横向位置话题 |
| `Objects/Eav24_default.json` | `bDetectBounds` | `true` | 获得赛道出界检测 |
| `Objects/Eav24_default.json` | `bEnableReplay` + `bApplyInitialConfig` | `true` | 启用 NPC 轨迹回放 |
| `Objects/Eav24_default.json` | `bUseTireThermalModel` | `true` | 启用轮胎热模型（已开启） |
| `simconfig.json` | `bIsLockStepEnabled` | `true` | 锁步离线评估模式 |
