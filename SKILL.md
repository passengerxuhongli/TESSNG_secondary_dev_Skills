---
name: tessng-secondary-dev
description: TESS NG V4.1 二次开发技能 - 支持 Python/Java/C++ 三种语言的交通仿真二次开发
doc_version: V4.1
---

# TESS NG 二次开发技能

## 技能概述

本技能提供微观交通仿真软件 **TESS NG V4.1** 的二次开发能力，支持 **Python**、**Java**、**C++** 三种编程语言。TESS NG 是由上海集达交通科技有限公司自主研发的微观交通仿真软件，具有完全独立的知识产权。

### 核心能力

- **仿真流程控制**：启动、暂停、恢复、关闭仿真，单步仿真执行
- **路网管理**：路网文件的加载、编辑、CRUD 操作
- **信号控制**：交通信号灯、信号组、多周期信号控制方案
- **车辆管理**：车辆行为控制、速度引导、路径诱导、编队控制
- **特殊区域**：事故区域、施工区域、限速区域、收费站、停车场
- **行人仿真**：行人区域、行人路径、人行横道信号
- **数据输出**：检测器、采集器、车辆轨迹、结构化路网数据

### 支持的编程语言

| 语言 | 版本要求 | 开发环境 |
|------|----------|----------|
| Python | 3.6 / 3.8 | PyCharm |
| Java | JDK 8+ | IntelliJ IDEA |
| C++ | MSVC 2017+ | Visual Studio / Qt Creator |

---

## 快速开始

### Python 开发环境配置

1. **安装 Python 3.6 或 3.8**
   ```bash
   # 下载地址：https://www.python.org/ftp/python/
   # 安装时勾选"Add Python to PATH"
   ```

2. **安装 tessng 包**
   ```bash
   pip install tessng
   # 或使用阿里云镜像加速
   pip install tessng -i https://mirrors.aliyun.com/pypi/simple/
   ```

3. **安装依赖库**
   ```bash
   pip install PySide2
   ```

4. **运行示例**
   ```python
   from tessng import TessngFactory
   from MyPlugin import MyPlugin
   
   my_plugin = MyPlugin()
   config = {
       "__netfilepath": "path/to/netfile",
       "__workspace": "D:/TESSNG/V4.1",
       "__simuafterload": True,
   }
   factory = TessngFactory()
   factory.build(my_plugin, config)
   ```

### Java 开发环境配置

1. **安装 JDK 8 或更高版本**
   ```bash
   # 下载地址：https://www.oracle.com/java/technologies/downloads/
   ```

2. **配置项目结构**
   ```
   TESS_JavaAPI_EXAMPLE/
   ├── lib/           # TESSNG.jar 等依赖库
   ├── src/main/java/ # 源代码
   │   ├── Main.java
   │   ├── MyPlugin.java
   │   ├── MyNet.java
   │   └── MySimulator.java
   └── pom.xml        # Maven 配置
   ```

3. **加载本地库**
   ```java
   static {
       try {
           System.loadLibrary("TESS_WIN_Java");
       } catch (UnsatisfiedLinkError e) {
           System.err.println("Native code library failed to load");
           System.exit(1);
       }
   }
   ```

### C++ 开发环境配置

1. **安装 Qt 5.12.x**
   ```bash
   # 下载地址：https://download.qt.io/archive/qt/5.12/
   # 确保勾选"MSVC 2017 64-bit"组件
   ```

2. **安装 Visual Studio 2017 或更高版本**
   - 选择"使用 C++ 的桌面开发"工作负载

3. **配置环境变量**
   ```bash
   # 添加 Qt bin 目录到 Path
   # 例如：D:/Qt/5.12.2/msvc2019_64/bin
   ```

---

## 核心 API 接口

### 顶层接口：TessInterface

所有二次开发功能的入口接口，通过它可获取三个子接口：

| 子接口 | 功能描述 |
|--------|----------|
| `SimuInterface` | 仿真控制接口 |
| `NetInterface` | 路网操作接口 |
| `GuiInterface` | GUI 交互接口 |

### SimuInterface - 仿真控制接口

#### 仿真流程控制

```python
# Python 示例
tess_interface.startSimulation()      # 开始仿真
tess_interface.pauseSimulation()      # 暂停仿真
tess_interface.resumeSimulation()     # 恢复仿真
tess_interface.closeSimulation()      # 关闭仿真
tess_interface.runOneStep()           # 单步仿真
```

```java
// Java 示例
tessInterface.startSimulation();
tessInterface.pauseSimulation();
tessInterface.resumeSimulation();
tessInterface.closeSimulation();
tessInterface.runOneStep();
```

#### 仿真参数设置

```python
# Python 示例
tess_interface.setSimulationAccuracy(0.001)  # 设置仿真精度
tess_interface.setRandomSeed(12345)          # 设置随机种子
tess_interface.setAcceleration(9.8)          # 设置加速度
```

#### 数据输出控制

```python
# Python 示例
tess_interface.isRecordSignalLampStatus()           # 读取信号灯状态输出配置
tessInterface.setIsRecordSignalLampStatus(True)     # 设置是否输出信号灯数据
```

### NetInterface - 路网操作接口

#### 基础路网元素 CRUD

```python
# Python 示例
# 创建路段
road = net_interface.createRoad(road_id, start_node, end_node, lane_count)

# 读取路段信息
road_info = net_interface.getRoadInfo(road_id)

# 更新路段
net_interface.updateRoad(road_id, new_params)

# 删除路段
net_interface.deleteRoad(road_id)
```

#### 信号控制

```python
# Python 示例 - 创建信号控制方案
signal_plan = net_interface.createSignalPlan(plan_id, plan_name)
signal_plan.addPhase(phase_id, duration, green_time)

# 创建信号灯
signal_lamp = net_interface.createSignalLamp(lamp_id, controller_id)

# 创建人行横道信号灯
crosswalk_signal = net_interface.createCrosswalkSignalLamp(lamp_id, controller_id)
```

#### 特殊区域管理

```python
# Python 示例
# 事故区域
accident_zone = net_interface.createAccIDentZone(zone_id)
accident_zone.addInterval(start_time, end_time, blocked_lanes)

# 施工区域
road_work = net_interface.createRoadWorkZone(zone_id)

# 限速区域
speed_limit = net_interface.createReduceSpeedArea(area_id)
speed_limit.addInterval(start_time, end_time, speed_limit_value)
speed_limit.addVehicleType(vehicle_type, speed_limit)

# 收费站
toll_lane = net_interface.createTollLane(lane_id)
toll_point = net_interface.createTollDecisionPoint(point_id)
toll_routing = net_interface.createTollRouting(routing_id)
toll_stopping = net_interface.createTollPoint(point_id)

# 停车场
parking_stall = net_interface.createParkingStall(stall_id)
parking_region = net_interface.createParkingRegion(region_id)
parking_decision = net_interface.createParkingDecisionPoint(point_id)
parking_routing = net_interface.createParkingRouting(routing_id)
```

#### 行人区域

```python
# Python 示例
# 基础行人区域
pedestrian = net_interface.createPedestrian(ped_id)

# 各种形状的区域
obstacle = net_interface.createObstacleRegion(region_id)
passenger = net_interface.createPassengerRegion(region_id)
ellipse = net_interface.createPedestrianEllipseRegion(region_id)
fan = net_interface.createPedestrianFanShapeRegion(region_id)
polygon = net_interface.createPedestrianPolygonRegion(region_id)
rect = net_interface.createPedestrianRectRegion(region_id)
triangle = net_interface.createPedestrianTriangleRegion(region_id)
sidewalk = net_interface.createPedestrianSideWalkRegion(region_id)
crosswalk = net_interface.createPedestrianCrossWalkRegion(region_id)
stair = net_interface.createPedestrianStairRegion(region_id)

# 行人路径
ped_path = net_interface.createPedestrianPath(path_id)
ped_path_point = net_interface.createPedestrianPathPoint(point_id)
```

#### 节点与重构

```python
# Python 示例
# 节点建设
junction = net_interface.createJunction(junction_id)

# 重构区域
reconstruction = net_interface.createIReconstruction(area_id)

# 受限区域
limited = net_interface.createILimitedZone(zone_id)
```

### MyPlugin - 插件基类

```python
# Python 示例
from tessng import MyPluginBase

class MyPlugin(MyPluginBase):
    def afterLoadNet(self):
        """路网加载完成后调用"""
        pass
    
    def beforeSimulation(self):
        """仿真开始前调用"""
        pass
    
    def afterStart(self):
        """仿真启动后调用"""
        pass
    
    def afterOneStep(self, simulation_time):
        """每个仿真步长后调用"""
        pass
    
    def afterStop(self):
        """仿真停止后调用"""
        pass
```

### MySimulator - 仿真器扩展

```python
# Python 示例
from tessng import MySimulatorBase

class MySimulator(MySimulatorBase):
    def vehicleFollowModel(self, vehicle_id, lead_vehicle_id):
        """跟驰模型"""
        pass
    
    def laneChangeModel(self, vehicle_id):
        """换道模型"""
        pass
```

---

## 应用场景速查表

| 场景分类 | 场景名称 | Python | Java | C++ |
|----------|----------|--------|------|-----|
| **信号控制** | 单点 Webster 控制 | ✅ | ✅ | ✅ |
| | 干线绿波控制 | ✅ | ✅ | ✅ |
| | 公交优先控制 | ✅ | ✅ | ✅ |
| | 匝道 ALINEA 控制 | ✅ | ✅ | ✅ |
| **车辆引导** | 路径诱导 | ✅ | ✅ | ✅ |
| | 速度引导 | ✅ | ✅ | ✅ |
| | 车辆编队 | ✅ | ✅ | ✅ |
| **特殊场景** | 事故区域仿真 | ✅ | ✅ | ✅ |
| | 施工区域仿真 | ✅ | ✅ | ✅ |
| | 收费站仿真 | ✅ | ✅ | ✅ |
| | 停车场仿真 | ✅ | ✅ | ✅ |
| **行人仿真** | 行人区域 | ✅ | ✅ | ✅ |
| | 人行横道 | ✅ | ✅ | ✅ |
| **参数标定** | 跟驰模型标定 | ✅ | ✅ | ✅ |
| | 换道模型标定 | ✅ | ✅ | ✅ |

---

## 单位说明

TESS NG V4.0 起采用公制单位：

| 物理量 | 单位 |
|--------|------|
| 长度 | 米 (m) |
| 速度 | 米/秒 (m/s) |
| 加速度 | 米/秒² (m/s²) |
| 时间 | 秒 (s) |

> **注意**：V3.x 版本默认使用像素单位，V4.x 已转换为公制单位。如需像素与米的转换，可使用 `unitOfMeasure` 参数。

---

## 插件注册机制

**关键概念**：TESS NG 通过 `build` 方法（Python/Java）或`loadPluginFromMem` 方法（C++）注册用户自定义插件。

### Python/Java 注册方式
```python
# Python
factory = TessngFactory()
factory.build(my_plugin, config)
```

```java
// Java
TessngFactory tessngFactory = new TessngFactory();
tessngFactory.build(myPlugin, workspacePath);
```

### C++ 注册方式
```cpp
// C++
auto *pPlugin = new MyPlugin();
pPlugin->init();
gpTessInterface->loadPluginFromMem(pPlugin);
```

**注册后**，TESS NG 会在仿真关键节点自动调用插件中重写的钩子方法，无需手动调用。

---

## 参考资源

### 官方链接

- [集达交通官网](https://jidatraffic.com/#/home)
- [TESS NG 二次开发官方文档](http://jidatraffic.com:82/)
- [GitHub 仓库](https://github.com/jIDa-traffic/TESSNG_SecondaryDev)
- [Gitee 仓库](https://gitee.com/jidatraffic/tessng-secondary-doc)

### 本技能包含的参考文件

- `references/documentation/other/` - 三种语言的完整 API 文档
- `references/documentation/api/` - API 示例说明
- `references/dependencies/` - 依赖关系分析
- `references/patterns/` - 设计模式分析
- `references/config_patterns/` - 配置模式分析

---

## 更新日志

### V4.1 (2025-09-30)

- [新增] 设置车辆安全时间间隔：`setSafeTimeInterval`
- [新增] 设置车辆停车距离：`setStoppingDistance`
- [新增] 读取/设置是否输出交通信号灯头数据：`isRecordSignalLampStatus`/`setIsRecordSignalLampStatus`
- [移除] 弃用的变道相关插件函数：`reSetChangeLaneFreelyParam`、`isPassbyEventZone`

### V4.0 (2025-01-21)

- [升级] TESSNG 内核从 V3.1 升级到 V4.0
- [新增] `ISignalPlan` 多周期信号控制方案接口
- [新增] `ISignalLamp` 交通信号灯接口
- [新增] `ICrosswalkSignalLamp` 人行横道信号灯接口
- [新增] `IRoadWorkZone` 施工区域接口
- [新增] `IAccIDentZone` 事故区域接口（支持多时间段）
- [新增] `ILimitedZone` 受限区域接口
- [新增] `IReconstruction` 改扩建接口
- [新增] `IReduceSpeedArea` 限速区域接口
- [新增] 收费站系列接口
- [新增] 停车场系列接口
- [新增] `IJunction` 节点建设与路径重构接口
- [新增] 行人区域系列接口
- [优化] 统一采用公制单位

---

**Generated by Skill Seeker** | TESS NG Secondary Development Skill
