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


### GuiInterface - GUI 交互接口

#### 自定义面板接口

```python
# Python 示例   鼠标事件
from PySide2.QtGui import QMouseEvent

def afterViewMousePressEvent(self, event: QMouseEvent) -> None:
    # 获取鼠标坐标
    mouse_pos = event.pos()
    # 转为仿真场景坐标
    scene_pos = self.netiface.graphicsView().mapToScene(mouse_pos)
    x = scene_pos.x()
    y = scene_pos.y()
```



### MyNet - 路网建模扩展模版

```python
# Python 示例
from PySide2.QtCore import QPointF

from tessng import PyCustomerNet, tessngIFace, m2p, p2m, NetItemType, GraphicsItemPropName


# 用户插件子类，代表用户自定义与路网相关的实现逻辑，继承自MyCustomerNet
class MyNet(PyCustomerNet):
    def __init__(self):
        super().__init__()
        # 代表TESS NG 的接口
        self.iface = tessngIFace()
        # 代表TESS NG 的路网子接口
        self.netiface = self.iface.netInterface()
        # 代表TESS NG 的仿真子接口
        self.simuiface = self.iface.simuInterface()

    # 自定义方法：创建路网上的路段和连接段
    def create_network(self) -> None:
        # 创建第一条路段
        start_point = QPointF(m2p(-300), 0)
        end_point = QPointF(m2p(300), 0)
        link_points = [start_point, end_point]
        link1 = self.netiface.createLink(link_points, 7, "曹安公路")
        if link1 is not None:
            # 车道列表
            lanes = link1.lanes()
            # 打印该路段所有车道ID列表
            print("曹安公路车道ID列表：", [lane.id() for lane in lanes])
            # 在当前路段创建发车点
            dp = self.netiface.createDispatchPoint(link1)
            if dp is not None:
                # 设置发车间隔，含车型组成、时间间隔、发车数
                dp.addDispatchInterval(1, 2, 28)

        # 创建第二条路段
        start_point = QPointF(m2p(-300), m2p(-25))
        end_point = QPointF(m2p(300), m2p(-25))
        link_points = [start_point, end_point]
        link2 = self.netiface.createLink(link_points, 7, "次干道")
        if link2 is not None:
            # 在当前路段创建发车点
            dp = self.netiface.createDispatchPoint(link2)
            if dp is not None:
                # 设置发车间隔，含车型组成、时间间隔、发车数
                dp.addDispatchInterval(1, 3600, 3600)
            # 将外侧车道设为公交专用道
            lanes = link2.lanes()
            lane = lanes[0]
            lane.setLaneType("公交专用道")

        # 创建第三条路段
        start_point = QPointF(m2p(-300), m2p(25))
        end_point = QPointF(m2p(-150), m2p(25))
        link_points = [start_point, end_point]
        link3 = self.netiface.createLink(link_points, 3)
        if link3 is not None:
            # 在当前路段创建发车点
            dp = self.netiface.createDispatchPoint(link3)
            if dp is not None:
                # 设置发车间隔，含车型组成、时间间隔、发车数
                dp.addDispatchInterval(1, 3600, 3600)

        # 创建第四条路段
        start_point = QPointF(m2p(-50), m2p(25))
        end_point = QPointF(m2p(50), m2p(25))
        link_points = [start_point, end_point]
        link4 = self.netiface.createLink(link_points, 3)

        # 创建第五条路段
        start_point = QPointF(m2p(150), m2p(25))
        end_point = QPointF(m2p(300), m2p(25))
        link_points = [start_point, end_point]
        link5 = self.netiface.createLink(link_points, 3, "自定义限速路段")
        if link5 is not None:
            # 设置路段限速 km/h
            link5.setLimitSpeed(30)

        # 创建第六条路段
        start_point = QPointF(m2p(-300), m2p(50))
        end_point = QPointF(m2p(300), m2p(50))
        link_points = [start_point, end_point]
        link6 = self.netiface.createLink(link_points, 3, "动态发车路段")
        if link6 is not None:
            # 设置路段限速 km/h
            link6.setLimitSpeed(80)

        # 创建第七条路段
        start_point = QPointF(m2p(-300), m2p(75))
        end_point = QPointF(m2p(-250), m2p(75))
        link_points = [start_point, end_point]
        link7 = self.netiface.createLink(link_points, 3)
        if link7 is not None:
            # 设置路段限速 km/h
            link7.setLimitSpeed(80)

        # 创建第八条路段
        start_point = QPointF(m2p(-50), m2p(75))
        end_point = QPointF(m2p(300), m2p(75))
        link_points = [start_point, end_point]
        link8 = self.netiface.createLink(link_points, 3)
        if link8 is not None:
            # 设置路段限速 km/h
            link8.setLimitSpeed(80)

        # 创建第一条连接段
        if link3 is not None and link4 is not None:
            from_lane_numbers = [1, 2, 3]
            to_lane_numbers = [1, 2, 3]
            connector1 = self.netiface.createConnector(link3.id(), link4.id(), from_lane_numbers, to_lane_numbers, "连接段1", True)

        # 创建第二条连接段
        if link4 is not None and link5 is not None:
            from_lane_numbers = [1, 2, 3]
            to_lane_numbers = [1, 2, 3]
            connector2 = self.netiface.createConnector(link4.id(), link5.id(), from_lane_numbers, to_lane_numbers, "连接段2", True)

        # 创建第三条连接段
        if link7 is not None and link8 is not None:
            from_lane_numbers = [1, 2, 3]
            to_lane_numbers = [1, 2, 3]
            connector3 = self.netiface.createConnector(link7.id(), link8.id(), from_lane_numbers, to_lane_numbers, "动态发车连接段", True)

    # 重写父类方法：当加载路网后TESS NG 调用此方法
    def afterLoadNet(self) -> None:
        # 获取路段数
        count = self.netiface.linkCount()
        # 如果路网上没有路段，则调用自定义的创建路网的方法
        if count == 0:
            self.create_network()

        if self.netiface.linkCount() > 0:
            # 所有路段对象列表
            links = self.netiface.links()
            # ID等于1的路段
            link = self.netiface.findLink(1)
            if link is not None:
                # 路段中心线断点集
                link_points = link.centerBreakPoints()
                # print("一条路段中心线断点：", [(p.x(), p.y()) for p in link_points])
                # 路段的车道对象列表
                lanes = link.lanes()
                if lanes is not None and len(lanes) > 0:
                    # 第一条车道中心线断点
                    lane_points = lanes[0].centerBreakPoints()
                    # print("一条车道中心线断点：", [(p.x(), p.y()) for p in lane_points])
            # 所有连接段对象列表
            connectors = self.netiface.connectors()
            if len(connectors) > 0:
                # 第一条连接段的所有“车道连接”
                lane_connectors = connectors[0].laneConnectors()
                # 其中第一条“车道连接”
                lane_connector = lane_connectors[0]
                # "车道连接“断点集
                lane_connector_points = lane_connector.centerBreakPoints()
                # print("一条'车道连接'中心线断点：", [(p.x(), p.y()) for p in lane_connector_points])

    # 重写父类方法：是否允许用户对路网元素的绘制进行干预，如选择路段标签类型、确定绘制颜色等，本方法目的在于减少不必要的对python方法调用频次
    def isPermitForCustDraw(self) -> bool:
        """
        :return: 返回值，True表示允许用户对路网元素的绘制进行干预，False表示不允许用户对路网元素的绘制进行干预
        """
        # 获取当前路网文件名
        net_file_name = self.netiface.netFilePath()
        if "Temp" in net_file_name:
            return True
        else:
            return False

    # 重写父类方法：在绘制路网元素时被调用，确定用ID或名称，以及字体大小绘制标签，也可确定不绘制标签
    def ref_labelNameAndFont(self, itemType, itemId, ref_outPropName, ref_outFontSize) -> None:
        """
        :param itemType: NetItemType常量，代表不同类型路网元素
        :param itemId: 路网元素的ID
        :param ref_outPropName: GraphicsItemPropName枚举类型，None_表示不绘制，ID表示绘制ID，NAME表示绘制名称，影响路段和连接段的标签是否被绘制
        :param ref_outFontSize: 标签大小，单位：米。假设车道宽度是3.5米，如果ref_outFontSize.value等于7，绘制的标签大小占两个车道宽度
        """
        # 如果仿真正在进行，设置ref_outPropName.value等于GraphicsItemPropName.None_，路段和车道都不绘制标签
        if self.simuiface.isRunning():
            ref_outPropName.value = GraphicsItemPropName.None_
            return
        # 默认绘制ID
        ref_outPropName.value = GraphicsItemPropName.Id
        # 标签大小为6米
        ref_outFontSize.value = 6
        # 如果是连接段一律绘制名称
        if itemType == NetItemType.GConnectorType:
            ref_outPropName.value = GraphicsItemPropName.Name
        # 如果是路段，则根据ID判断是否绘制名称
        elif itemType == NetItemType.GLinkType:
            if itemId == 1 or itemId == 5 or itemId == 6:
                ref_outPropName.value = GraphicsItemPropName.Name

    # 重写父类方法：是否绘制车道中心线
    def isDrawLaneCenterLine(self, lane_id: int) -> bool:
        """
        :param lane_id: 车道ID
        :return: 是否绘制车道中心线，True表示绘制，False表示不绘制
        """
        return True

    # 重写父类方法：是否绘制路段中心线
    def isDrawLinkCenterLine(self, link_id: int) -> bool:
        """
        :param link_id: 路段ID
        :return: 是否绘制路段中心线，True表示绘制，False表示不绘制
        """
        return link_id != 1

```



### MyPlugin - 插件扩展模版

```python
# Python 示例
import os
from PySide2.QtWidgets import QApplication

from tessng import TessPlugin, TessngFactory
from MyGUI import MyGUI
from MyNet import MyNet
from MySimulator import MySimulator

class MyPlugin(TessPlugin):
    def __init__(self, config: dict):
        super().__init__()
        # 配置
        self.config: dict = config
        # 自定义界面
        self.my_gui = None
        # 自定义路网控制逻辑
        self.my_net = None
        # 自定义仿真控制逻辑
        self.my_simulator = None

    # 启动TESS NG 窗体
    def build(self) -> None:
        # 创建工作空间文件夹
        default_workspace: str = os.path.join(os.getcwd(), "WorkSpace")
        workspace: str = self.config.get("__workspace", default_workspace)
        os.makedirs(workspace, exist_ok=True)

        # 创建TESS NG 对象
        app = QApplication()
        factory = TessngFactory()
        tessng = factory.build(self, self.config)
        if tessng is not None:
            app.exec_()

    # 重写父类方法：在TESS NG 工厂类创建TESS NG 对象时调用
    def init(self) -> None:
        # 自定义界面
        self.my_gui = MyGUI()
        # 自定义路网控制逻辑
        self.my_net = MyNet()
        # 自定义仿真控制逻辑
        self.my_simulator = MySimulator()

        # 关联信号和槽函数
        self.my_simulator.showRunInfo.connect(self.my_gui.show_run_info)

    # 重写父类方法：返回插件路网子接口，此方法由TESS NG 调用
    def customerNet(self):
        return self.my_net

    # 重写父类方法：返回插件仿真子接口，此方法由TESS NG 调用
    def customerSimulator(self):
        return self.my_simulator
```

### MySimulator - 仿真器扩展模版

```python
# Python 示例
import random
from datetime import datetime
from PySide2.QtCore import QObject, Qt, Signal

from tessng import PyCustomerSimulator, tessngIFace, Online, m2p, p2m


class MySimulator(PyCustomerSimulator, QObject):
    # 停止仿真的信号
    forStopSimu = Signal()
    # 启动仿真的信号
    forReStartSimu = Signal()
    # 在自定义面板的信息窗上显示信息的信号
    showRunInfo = Signal(str)

    def __init__(self):
        PyCustomerSimulator.__init__(self)
        QObject.__init__(self)
        # 代表TESS NG的接口
        self.iface = tessngIFace()
        # 代表TESS NG 的路网子接口
        self.netiface = self.iface.netInterface()
        # 代表TESS NG 的仿真子接口
        self.simuiface = self.iface.simuInterface()
        # 代表TESS NG 的界面子接口
        self.guiiface = self.iface.guiInterface()
        # 主面板
        self.main_window = self.guiiface.mainWindow()

        # 关联信号和槽函数
        # 将信号关联到主窗体的槽函数doStopSimu，可以安全地停止仿真
        self.forStopSimu.connect(self.main_window.doStopSimu, Qt.QueuedConnection)
        # 将信号关联到主窗体的槽函数doStartSimu，可以安全地启动仿真
        self.forReStartSimu.connect(self.main_window.doStartSimu, Qt.QueuedConnection)

        # 车辆方阵的车辆数
        self.square_vehi_count: int = 28
        # 飞机速度，飞机后面的车辆速度会被设定为此数据
        self.plane_speed: float = 0
        # 当前正在仿真计算的路网名称
        self.current_net_name: str = ""
        # 相同路网连续仿真次数
        self.max_simu_count: int = 0

    # 重写父类方法：TESS NG 在仿真开启前调用此方法
    def beforeStart(self, ref_keep_on: bool) -> None:
        # 获取当前路网名称
        current_net_name: str = self.netiface.netFilePath()
        if current_net_name != self.current_net_name:
            self.current_net_name = current_net_name
            self.max_simu_count = 1
        else:
            self.max_simu_count += 1

    # 重写父类方法：TESS NG 在仿真开启后调用此方法
    def afterStart(self) -> None:
        pass

    # 重写父类方法：TESS NG 在仿真结束后调用此方法
    def afterStop(self) -> None:
        # 最多连续仿真2次
        if self.max_simu_count >= 2:
            return
        # 通知主窗体开启仿真
        self.forReStartSimu.emit()

    # 重写父类方法：TESS NG 在每个计算周期结束后调用此方法，大量用户逻辑在此实现，注意耗时大的计算要尽可能优化，否则影响运行效率
    def afterOneStep(self) -> None:
        # = == == == == == = 以下是获取一些仿真过程数据的方法 == == == == == ==
        # 仿真精度
        simu_accuracy: int = self.simuiface.simuAccuracy()
        # 仿真倍速
        simu_multiples: int = self.simuiface.acceMultiples()
        # 当前仿真计算批次
        batch_number = self.simuiface.batchNumber()
        # 当前已仿真时间，单位：毫秒
        simu_time = self.simuiface.simuTimeIntervalWithAcceMutiples()
        # 开始仿真的现实时间，单位：毫秒
        start_realtime = self.simuiface.startMSecsSinceEpoch()
        # 如果仿真时间大于等于600秒，通知主窗体停止仿真
        if simu_time >= 600 * 1000:
            self.forStopSimu.emit()

        # 当前正在运行车辆列表
        all_vehicles = self.simuiface.allVehiStarted()
        # 打印当前在运行车辆ID列表
        # print([item.id() for item in all_vehicles])
        # 当前在ID为1的路段上车辆
        vehicles = self.simuiface.vehisInLink(1)

        # 在信息窗显示信息
        if batch_number % simu_accuracy == 0:
            # 路段数量
            link_count: int = self.netiface.linkCount()
            # 车辆数
            vehicle_count: int = len(all_vehicles)
            run_info: str = f"路段数：{link_count}\n运行车辆数：{vehicle_count}\n仿真时间：{simu_time}(毫秒)"
            self.showRunInfo.emit(run_info)

        # 动态发车，不通过发车点发送，直接在路段和连接段中间某位置创建并发送，每50个计算批次发送一次
        if batch_number % 50 == 1:
            r = hex(256 + random.randint(0,256))[3:].upper()
            g = hex(256 + random.randint(0,256))[3:].upper()
            b = hex(256 + random.randint(0,256))[3:].upper()
            color = f"#{r}{g}{b}"
            # 路段上发车
            dvp = Online.DynaVehiParam()
            dvp.vehiTypeCode = random.randint(0, 4) + 1
            dvp.roadId = 6
            dvp.laneNumber = random.randint(0, 3)
            dvp.dist = 50
            dvp.speed = 20
            dvp.color = color
            vehicle1 = self.simuiface.createGVehicle(dvp)
            if vehicle1 is not None:
                pass

            # 连接段上发车
            dvp2 = Online.DynaVehiParam()
            dvp2.vehiTypeCode = random.randint(0, 4) + 1
            dvp2.roadId = 3
            dvp2.laneNumber = random.randint(0, 3)
            dvp2.toLaneNumber = dvp2.laneNumber # 默认为 - 1，如果大于等于0, 在连接段上发车
            dvp2.dist = 50
            dvp2.speed = 20
            dvp2.color = color
            vehicle2 = self.simuiface.createGVehicle(dvp2)
            if vehicle2 is not None:
                pass

        # 信号灯组相位颜色
        lPhoneColor = self.simuiface.getSignalPhasesColor()
        #print("信号灯组相位颜色", [(pcolor.signalGroupId, pcolor.phaseNumber, pcolor.color, pcolor.mrIntervalSetted, pcolor.mrIntervalByNow) for pcolor in lPhoneColor])
        # 获取当前仿真时间完成穿越采集器的所有车辆信息
        lVehiInfo = self.simuiface.getVehisInfoCollected()
        #if len(lVehiInfo) > 0:
        #    print("车辆信息采集器采集信息：", [(vinfo.collectorId, vinfo.vehiId) for vinfo in lVehiInfo])
        # 获取最近集计时间段内采集器采集的所有车辆集计信息
        lVehisInfoAggr = self.simuiface.getVehisInfoAggregated()
        #if len(lVehisInfoAggr) > 0:
        #    print("车辆信息采集集计数据：", [(vinfo.collectorId, vinfo.vehiCount) for vinfo in lVehisInfoAggr])
        # 获取当前仿真时间排队计数器计数的车辆排队信息
        lVehiQueue = self.simuiface.getVehisQueueCounted()
        #if len(lVehiQueue) > 0:
        #    print("车辆排队计数器计数：", [(vq.counterId, vq.queueLength) for vq in lVehiQueue])
        # 获取最近集计时间段内排队计数器集计数据
        lVehiQueueAggr = self.simuiface.getVehisQueueAggregated()
        #if len(lVehiQueueAggr) > 0:
        #    print("车辆排队集计数据：", [(vqAggr.counterId, vqAggr.avgQueueLength) for vqAggr in lVehiQueueAggr])
        # 获取当前仿真时间行程时间检测器完成的行程时间检测信息
        lVehiTravel = self.simuiface.getVehisTravelDetected()
        #if len(lVehiTravel) > 0:
        #    print("车辆行程时间检测信息：", [(vtrav.detectedId, vtrav.travelDistance) for vtrav in lVehiTravel])
        # 获取最近集计时间段内行程时间检测器集计数据
        lVehiTravAggr = self.simuiface.getVehisTravelAggregated()
        #if len(lVehiTravAggr) > 0:
        #    print("车辆行程时间集计数据：", [(vTravAggr.detectedId, vTravAggr.vehiCount, vTravAggr.avgTravelDistance) for vTravAggr in lVehiTravAggr])

    # 重写父类方法：在车辆启动上路时被TESS NG调用一次
    def initVehicle(self, vehicle) -> None:
        # 设置当前车辆及其驾驶行为过载方法被TESSNG调用频次，即多少个计算周调用一次指定方法。如果对运行效率有极高要求，可以精确控制具体车辆或车辆类型及具体场景相关参数
        self.set_steps_per_call(vehicle)

        # 车辆ID，不含首位数，首位数与车辆来源有关，如发车点、公交线路
        tmp_vehicle_id = vehicle.id() % 100000
        # 车辆所在路段名或连接段名
        road_name = vehicle.roadName()
        # 车辆所在路段ID或连接段ID
        road_id = vehicle.roadId()
        if road_name == '曹安公路':
            # 飞机
            if tmp_vehicle_id == 1:
                vehicle.setVehiType(12)
                vehicle.initLane(3, m2p(105), 0)
            # 工程车
            elif tmp_vehicle_id >= 2 and tmp_vehicle_id <= 8:
                vehicle.setVehiType(8)
                vehicle.initLane((tmp_vehicle_id - 2) % 7, m2p(80), 0)
            # 消防车
            elif tmp_vehicle_id >= 9 and tmp_vehicle_id <= 15:
                vehicle.setVehiType(9)
                vehicle.initLane((tmp_vehicle_id - 2) % 7, m2p(65), 0)
            # 消防车
            elif tmp_vehicle_id >= 16 and tmp_vehicle_id <= 22:
                vehicle.setVehiType(10)
                vehicle.initLane((tmp_vehicle_id - 2) % 7, m2p(50), 0)
            # 最后两队列小车
            elif tmp_vehicle_id == 23:
                vehicle.setVehiType(1)
                vehicle.initLane(1, m2p(35), 0)
            elif tmp_vehicle_id == 24:
                vehicle.setVehiType(1)
                vehicle.initLane(5, m2p(35), 0)
            elif tmp_vehicle_id == 25:
                vehicle.setVehiType(1)
                vehicle.initLane(1, m2p(20), 0)
            elif tmp_vehicle_id == 26:
                vehicle.setVehiType(1)
                vehicle.initLane(5, m2p(20), 0)
            elif tmp_vehicle_id == 27:
                vehicle.setVehiType(1)
                vehicle.initLane(1, m2p(5), 0)
            elif tmp_vehicle_id == 28:
                vehicle.setVehiType(1)
                vehicle.initLane(5, m2p(5), 0)
            # 最后两列小车的长度设为一样长，这个很重要，如果车长不一样长，加上导致的前车距就不一样，会使它们变道轨迹长度不一样，就会乱掉
            if tmp_vehicle_id >= 23 and tmp_vehicle_id <= 28:
                vehicle.setLength(m2p(4.5), True)

    # 自定义方法：设置本类实现的过载方法被调用频次，即多少个计算周期调用一次，过多的不必要调用会影响运行效率
    def set_steps_per_call(self, vehicle) -> None:
        # 设置当前车辆及其驾驶行为过载方法被TESSNG调用频次，即多少个计算周调用一次指定方法。如果对运行效率有极高要求，可以精确控制具体车辆或车辆类型及具体场景相关参数
        net_file_name = self.netiface.netFilePath()
        # 范例打开临时路段会会创建车辆方阵，需要进行一些仿真过程控制
        if "Temp" in net_file_name:
            # 允许对车辆重绘方法的调用
            vehicle.setIsPermitForVehicleDraw(True)
            # 计算限制车道方法每10个计算周期被调用一次
            vehicle.setSteps_calcLimitedLaneNumber(10)
            # 计算安全变道距离方法每10个计算周期被调用一次
            vehicle.setSteps_calcChangeLaneSafeDist(10)
            # 重新计算车辆期望速度方法每一个计算周期被调用一次
            vehicle.setSteps_reCalcdesirSpeed(1)
            # 重新设置车速方法每一个计算周期被调用一次
            vehicle.setSteps_reSetSpeed(1)
        else:
            # 仿真精度，即每秒计算次数
            steps = self.simuface.simuAccuracy()
            # ======设置本类过载方法被TESSNG调用频次，以下是默认设置，可以修改======
            # ======车辆相关方法调用频次======
            # 是否允许对车辆重绘方法的调用
            vehicle.setIsPermitForVehicleDraw(False)
            # 计算下一位置前处理方法被调用频次
            vehicle.setSteps_beforeNextPoint(steps * 300)
            # 计算下一位置方法方法被调用频次
            vehicle.setSteps_nextPoint(steps * 300)
            # 计算下一位置完成后处理方法被调用频次
            vehicle.setSteps_afterStep(steps * 300)
            # 确定是否停止车辆运行便移出路网方法调用频次
            vehicle.setSteps_isStopDriving(steps * 300)
            # ======驾驶行为相关方法调用频次======
            # 重新设置期望速度方法被调用频次
            vehicle.setSteps_reCalcdesirSpeed(steps * 300)
            # 计算最大限速方法被调用频次
            vehicle.setSteps_calcMaxLimitedSpeed(steps * 300)
            # 计算限制车道方法被调用频次
            vehicle.setSteps_calcLimitedLaneNumber(steps)
            # 计算车道限速方法被调用频次
            vehicle.setSteps_calcSpeedLimitByLane(steps)
            # 计算安全变道方法被调用频次
            vehicle.setSteps_calcChangeLaneSafeDist(steps)
            # 重新计算是否可以左强制变道方法被调用频次
            vehicle.setSteps_reCalcToLeftLane(steps)
            # 重新计算是否可以右强制变道方法被调用频次
            vehicle.setSteps_reCalcToRightLane(steps)
            # 重新计算是否可以左自由变道方法被调用频次
            vehicle.setSteps_reCalcToLeftFreely(steps)
            # 重新计算是否可以右自由变道方法被调用频次
            vehicle.setSteps_reCalcToRightFreely(steps)
            # 计算跟驰类型后处理方法被调用频次
            vehicle.setSteps_afterCalcTracingType(steps * 300)
            # 连接段上汇入到车道前处理方法被调用频次
            vehicle.setSteps_beforeMergingToLane(steps * 300)
            # 重新跟驰状态参数方法被调用频次
            vehicle.setSteps_reSetFollowingType(steps * 300)
            # 计算加速度方法被调用频次
            vehicle.setSteps_calcAcce(steps * 300)
            # 重新计算加速度方法被调用频次
            vehicle.setSteps_reSetAcce(steps * 300)
            # 重置车速方法被调用频次
            vehicle.setSteps_reSetSpeed(steps * 300)
            # 重新计算角度方法被调用频次
            vehicle.setSteps_reCalcAngle(steps * 300)
            vehicle.setSteps_recentTimeOfSpeedAndPos(steps * 300)
            vehicle.setSteps_travelOnChangingTrace(steps * 300)
            vehicle.setSteps_leaveOffChangingTrace(steps * 300)
            # 计算后续道路前处理方法被调用频次
            vehicle.setSteps_beforeNextRoad(steps * 300)

    # 重写父类方法：停止指定车辆运行，退出路网，但不会从内存删除，会参数各种统计
    def isStopDriving(self, vehicle) -> bool:
        # 范例车辆进入ID等于2的路段或连接段，路离终点小于100米，则驰出路网
        if vehicle.roadId() == 2:
            # 车头到当前路段或连接段终点距离
            dist = vehicle.vehicleDriving().distToEndpoint(True)
            # 如果距终点距离小于100米，车辆停止运行退出路网
            if dist < m2p(100):
                return True
        return False

    # 重写父类方法：在车辆被移除时被TESS NG调用一次
    def afterStopVehicle(self, vehicle) -> None:
        pass

    # 重写父类方法，重新计算车辆的加速度，即设置车辆的期望速度，只对当前帧生效
    def ref_reSetAcce(self, vehicle, inOutAcce) -> bool:
        """
        :param vehicle: 车辆对象
        :param inOutAcce: 车辆加速度，inOutAcce.value是TESS NG已计算的车辆加速度，此方法可以改变它
        :return: True：接受此次修改，False：忽略此次修改
        """
        roadName = vehicle.roadName()
        if roadName == "连接段1":
            if vehicle.currSpeed() > m2p(20 / 3.6):
                inOutAcce.value = m2p(-5)
                return True
            elif vehicle.currSpeed() > m2p(20 / 3.6):
                inOutAcce.value = m2p(-1)
                return True
        return False

    # 重写父类方法：重新计算车辆的期望速度，即设置车辆的期望速度，只对当前帧生效
    def ref_reCalcdesirSpeed(self, vehicle, ref_desirSpeed) -> bool:
        """
        :param vehicle: 车辆对象
        :param ref_desirSpeed: 期望速度，ref_desirSpeed.value，是已计算好的车辆期望速度，此方法可以改变它
        :return: True：接受此次修改，False：忽略此次修改
        """
        tmp_vehicle_id = vehicle.id() % 100000
        road_name = vehicle.roadName()
        if road_name == '曹安公路':
            if tmp_vehicle_id <= self.square_vehi_count:
                simu_time = self.simuiface.simuTimeIntervalWithAcceMutiples()
                if simu_time < 5 * 1000:
                    ref_desirSpeed.value = 0
                elif simu_time < 10 * 1000:
                    ref_desirSpeed.value = m2p(20 / 3.6)
                else:
                    ref_desirSpeed.value = m2p(40 / 3.6)
            return True
        return False

    # 重写父类方法：重新计算车辆的当前速度，即直接修改车辆的当前速度，只对当前帧生效
    def ref_reSetSpeed(self, vehicle, ref_inOutSpeed) -> bool:
        """
        :param vehicle: 车辆对象
        :param ref_inOutSpeed: 速度，ref_inOutSpeed.value，是已计算好的车辆速度，此方法可以改变它
        :return: True：接受此次修改，False：忽略此次修改
        """
        tmp_vehicle_id = vehicle.id() % 100000
        road_name = vehicle.roadName()
        if road_name == "曹安公路":
            if tmp_vehicle_id == 1:
                self.plane_speed = vehicle.currSpeed()
            elif 2 <= tmp_vehicle_id <= self.square_vehi_count:
                ref_inOutSpeed.value = self.plane_speed
            return True
        return False

    # 重写父类方法：重新计算车辆跟驰参数，安全时距及安全距离，只对当前帧生效
    def ref_reSetFollowingParam(self, vehicle, ref_inOutSi, ref_inOutSd) -> bool:
        """
        :param vehicle: 车辆对象
        :param ref_inOutSi: 安全时距，ref_inOutSi.value是TESS NG已计算好的值，此方法可以改变它
        :param ref_inOutSd: 安全距离，ref_inOutSd.value是TESS NG已计算好的值，此方法可以改变它
        :return: True：接受此次修改，False：忽略此次修改
        """
        road_name = vehicle.roadName()
        if road_name == "连接段2":
            ref_inOutSd.value = m2p(30)
            return True
        return False

    # 重写父类方法：计算车辆是否要左自由变道
    def reCalcToLeftFreely(self, vehicle) -> bool:
        """
        :param vehicle: 车辆对象
        :return: True：车辆向左自由变道，False：TESS NG 自行判断是否要向左自由变道
        """
        # 车辆到路段终点距离小于20米不变道
        if vehicle.vehicleDriving().distToEndpoint() - vehicle.length() / 2 < m2p(20):
            return False
        tmp_vehicle_id = vehicle.id() % 100000
        road_name = vehicle.roadName()
        if road_name == "曹安公路":
            if 23 <= tmp_vehicle_id <= 28:
                lane_number = vehicle.vehicleDriving().laneNumber()
                if lane_number in [1, 4]:
                    return True
        return False

    # 重写父类方法：计算车辆是否要右自由变道
    def reCalcToRightFreely(self, vehicle) -> bool:
        """
        :param vehicle: 车辆对象
        :return: True：车辆向右自由变道，False：TESS NG 自行判断是否要向右自由变道
        """
        tmp_vehicle_id = vehicle.id() % 100000
        # 车辆到路段终点距离小于20米不变道
        if vehicle.vehicleDriving().distToEndpoint() - vehicle.length() / 2 < m2p(20):
            return False
        road_name = vehicle.roadName()
        if road_name == "曹安公路":
            if 23 <= tmp_vehicle_id <= 28:
                lane_number = vehicle.vehicleDriving().laneNumber()
                if lane_number in [2, 5]:
                    return True
        return False

    # 重写父类方法：计算车辆当前禁行车道的序号列表，只对当前帧生效
    def calcLimitedLaneNumber(self, vehicle) -> list:
        """
        :param vehicle: 车辆对象
        :return: 禁行车道的序号的列表
        """
        # 如果当前车辆在路段上，且路段ID等于2，则小车走内侧，大车走外侧
        if vehicle.roadIsLink():
            link = vehicle.lane().link()
            if link is not None and link.id() == 2:
                lane_count = link.laneCount()
                # 小车走内侧，大车走外侧，设长度小于8米为小车
                if vehicle.length() < m2p(8):
                    return [num for num in range(lane_count // 2 - 1)]
                else:
                    return [num for num in range(lane_count // 2 - 1, lane_count)]
        return []

    # 重写父类方法：设置信号灯的灯色，需要每帧都调用，否则被TESS NG 计算的灯色覆盖
    def calcLampColor(self, signal_lamp) -> bool:
        """
        :param signal_lamp: 信号灯头对象
        :return: True：接受此次修改，False：忽略此次修改
        """
        if signal_lamp.id() == 5:
            signal_lamp.setLampColor("红")
            return True
        return False

    # 重写父类方法：车道限速，只对当前帧生效
    def ref_calcSpeedLimitByLane(self, link, lane_number: int, ref_outSpeed) -> bool:
        """
        :param link: 路段对象
        :param lane_number: 车道序号，从0开始，从右向左
        :param ref_outSpeed: 可以改变ref_outSpeed.value为设定的限速值，单位：km/h
        :return: True：接受此次修改，False：忽略此次修改
        """
        # ID为2路段，车道序号为0，速度不大于30千米/小时
        if link.id() == 2 and lane_number <= 1:
            ref_outSpeed.value = 30
            return True
        return False

    # 重写父类方法：对发车点一增加发车时间段
    #   此范例展示了以飞机打头的方阵全部驰出路段后为这条路段的发车点增加发车间隔
    def calcDynaDispatchParameters(self) -> list:
        # 当前仿真时间
        current_simu_time = self.simuiface.simuTimeIntervalWithAcceMutiples()
        # ID等于1的路段上的车辆
        vehicles = self.simuiface.vehisInLink(1)
        if current_simu_time < 1000 * 10 or len(vehicles) > 0:
            return []
        now = datetime.now()
        # 当前时间秒
        current_second = now.hour * 3600 + now.minute * 60 + now.second
        # 仿真10秒后且ID等于1的路段上车辆数为0，则为ID等于1的发车点增加发车间隔
        di = Online.DispatchInterval()
        di.dispatchId = 1
        di.fromTime = current_second
        di.toTime = di.fromTime + 300 - 1
        di.vehiCount = 300
        di.mlVehicleConsDetail = [
            Online.VehiComposition(1, 60),
            Online.VehiComposition(2, 40)
        ]
        return [di]

    # 重写父类方法：动态修改决策点不同路径流量比
    def calcDynaFlowRatioParameters(self) -> list:
        # 当前仿真计算批次
        batch_number = self.simuiface.batchNumber()
        # 在计算第20批次时修改某决策点各路径流量比
        if batch_number == 20:
            # 一个决策点某个时段各路径车辆分配比
            dfi = Online.DecipointFlowRatioByInterval()
            # 决策点编号
            dfi.deciPointID = 5
            # 起始时间 单位：秒
            dfi.startDateTime = 1
            # 结束时间 单位：秒
            dfi.endDateTime = 999999
            # 路径流量比
            rfr1 = Online.RoutingFlowRatio(10, 3)
            rfr2 = Online.RoutingFlowRatio(11, 4)
            rfr3 = Online.RoutingFlowRatio(12, 3)
            dfi.mlRoutingFlowRatio = [rfr1, rfr2, rfr3]
            return [dfi]
        return []

```

### MyGUI - 仿真GUI可视化扩展模版

```python
# Python 示例
import os
from pathlib import Path
from PySide2.QtWidgets import QMainWindow, QPushButton, QGroupBox, QTextBrowser, QVBoxLayout
from PySide2.QtWidgets import QWidget, QDockWidget, QMenu, QMessageBox, QFileDialog
from PySide2.QtCore import Qt

from tessng import tessngIFace


class MyGUI(QMainWindow):
    def __init__(self):
        super().__init__()
        # 代表TESS NG 的接口
        self.iface = tessngIFace()
        # 代表TESS NG 的路网子接口
        self.netiface = self.iface.netInterface()
        # 代表TESS NG 的仿真子接口
        self.simuiface = self.iface.simuInterface()
        # 代表TESS NG 的界面子接口
        self.guiiface = self.iface.guiInterface()
        # 主窗体
        self.main_window = self.guiiface.mainWindow()

        # 创建自定义面板
        self._create_gui()
        # 关联按钮与槽函数
        self._create_connect()
        # 添加到主窗体
        self._add_to_main_window()

    def _create_gui(self) -> None:
        """创建自定义面板"""
        # 创建按钮
        self.btn_open_net = QPushButton("打开路网")
        self.btn_start_simu = QPushButton("开启仿真")
        self.btn_pause_simu = QPushButton("暂停仿真")
        self.btn_stop_simu = QPushButton("停止仿真")

        # 创建信息窗
        self.group_box = QGroupBox("信息窗")
        self.txt_message = QTextBrowser(self.group_box)

        # 创建纵向布局
        self.vertical_layout_2 = QVBoxLayout(self.group_box)
        self.vertical_layout_2.setSpacing(6)
        self.vertical_layout_2.setContentsMargins(11, 11, 11, 11)
        self.vertical_layout_2.setContentsMargins(1, -1, 1, -1)
        self.vertical_layout_2.addWidget(self.txt_message)

        # 创建纵向布局
        self.vertical_layout = QVBoxLayout()
        self.vertical_layout.setSpacing(6)
        self.vertical_layout.setContentsMargins(11, 11, 11, 11)
        self.vertical_layout.addWidget(self.btn_open_net)
        self.vertical_layout.addWidget(self.btn_start_simu)
        self.vertical_layout.addWidget(self.btn_pause_simu)
        self.vertical_layout.addWidget(self.btn_stop_simu)
        self.vertical_layout.addWidget(self.group_box)

        # 设置布局
        self.central_widget = QWidget(self)
        self.central_widget.setLayout(self.vertical_layout)

    def _create_connect(self) -> None:
        """关联按钮与槽函数"""
        self.btn_open_net.clicked.connect(self._open_net)
        self.btn_start_simu.clicked.connect(self._start_simu)
        self.btn_pause_simu.clicked.connect(self._pause_simu)
        self.btn_stop_simu.clicked.connect(self._stop_simu)

    def _add_to_main_window(self) -> None:
        """添加自定义面板到主窗体"""
        dock_widget = QDockWidget("自定义与TESS NG交互界面", self.main_window)
        dock_widget.setObjectName("mainDockWidget")
        dock_widget.setFeatures(QDockWidget.NoDockWidgetFeatures)
        dock_widget.setAllowedAreas(Qt.LeftDockWidgetArea)
        dock_widget.setWidget(self.central_widget)
        self.guiiface.addDockWidgetToMainWindow(Qt.DockWidgetArea(1), dock_widget)

        # 增加菜单及菜单项
        menu_bar = self.guiiface.menuBar()
        menu = QMenu(menu_bar)
        menu_bar.addAction(menu.menuAction())
        menu.setTitle("范例菜单")
        action_ok = menu.addAction("范例菜单项")
        action_ok.setCheckable(True)
        action_ok.triggered.connect(self._print_ok)

    def _open_net(self) -> None:
        """打开路网"""
        if self.simuiface.isRunning():
            QMessageBox.warning(None, "提示信息", "请先停止仿真，再打开路网")
            return
        cust_suffix = "TESSNG Files (*.tess);;TESSNG Files (*.backup)"
        db_dir = os.fspath(Path(__file__).resolve().parent)
        selected_filter = "TESSNG Files (*.tess)"
        options = QFileDialog.Options(0)
        net_file_path, filtr = QFileDialog.getOpenFileName(self, "打开文件", db_dir, cust_suffix, selected_filter, options)
        if net_file_path:
            self.netiface.openNetFle(net_file_path)

    def _start_simu(self) -> None:
        """开启仿真"""
        if not self.simuiface.isRunning() or self.simuiface.isPausing():
            self.simuiface.startSimu()

    def _pause_simu(self) -> None:
        """暂停仿真"""
        if self.simuiface.isRunning():
            self.simuiface.pauseSimu()

    def _stop_simu(self) -> None:
        """停止仿真"""
        if self.simuiface.isRunning():
            self.simuiface.stopSimu()

    def _print_ok(self) -> None:
        """自定义按钮关联的槽函数"""
        QMessageBox.information(None, "提示信息", "is ok!")

    def show_run_info(self, run_info: str) -> None:
        """在自定义面板的信息窗上显示信息"""
        self.txt_message.clear()
        self.txt_message.setText(run_info)


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

- [济达交通官网](https://jidatraffic.com/#/home)
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
