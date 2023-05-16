### PLC 故障检测系统

#### 问题：两条高速两个监控中心
- 用户权限来切割
- ？

#### 配置：
- `plc_association` 对应 **网络拓扑图**
- `auto_system_device_list` 对应 **接入信息**
- `alarmrecord` 对应 **报警信息**
- `plc_diagnosis` 对应 **一键巡查**

#### 主页 http://localhost:8082/#/homePage
- ##### 需要的数据：plc & 交换机当前故障率、历史故障率、实时报警
1. PLC & 交换机故障率：**WebSocket** 从 **RDB** 获取
    1. **PLC** 正常率取自 `RDB Param7.F_CV`
    1. **交换机** 故障率取自 `RDB Param8.F_CV`
1. 故障率：**Node** 接口调取 **MySQL** 获取
1. 实时报警：**WebSocket** 从 **CxView** 获取

#### 网络拓扑图 http://localhost:8082/#/topology
- ##### 需要的数据：获取 `PLC & 交换机 IP 地址` 和 `状态`
- ##### 设计：plc & 交换机分布在 `左洞 右洞 和中间（变电站）`

1. 获取底部导航栏 `index number`
2. 获取左侧导航栏 `index number`
3. 结合两项去查询 `ip 地址 & 状态 `

#### 接入信息
- 数据：根据底部`导航栏 Index Number` 和`左侧导航栏 Index Number` 来从 `MySQL` 获取`ACU挂载信息`
- 点击 `ACU` 获取更多详细信息

#### 报警信息

#### 一键巡查

#### 智能分析

#### 项目材料 || 设置

#### 设计思路
1. 点击底部导航栏获取到`index`。
    - `test`
1. 点击左侧隧道导航栏获取隧道`index number`
1. 配置文件：各个隧道的PLC数量
1. 请求**接口**，通过配置文件中的隧道PLC数量做循环来获取RDB中含有 **PLC_隧道index number**的数据。返回给Node后台。
1. 后台做处理：根据返回的自定义域中的文字判断是在左洞，右洞，配电间哪一个。并分成三个对象返回给前端。
1. 前端接收数据，插入三个不同template中。
