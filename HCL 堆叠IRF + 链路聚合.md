<center><h2>
    HCL 堆叠IRF + 链路聚合
    </h2></center>

#### 一、目标:掌握H3C的堆叠，配合链路聚合。最终实现较为典型的小网络高可用方案。

1. H3C的IRF如果是两台设备堆叠，那两台IRF编号决不能一样，IRF靠优先靠优先级选举当成主设备。
2. IRF设备如果当成核心，下面的汇聚各自连线到核心IRF两台设备上，做线路冗余。从汇聚设备上看，比如设备3的两条线g1/0/1和g1/0/2是一组聚合链路，反推到核心上呢，那就是IRF的G1/0/1和G2/0/1是一组聚合链路，建议IRF和汇聚的聚合链路组group保持一致。
3. IRF设备上创建两条聚合链路，分别冗余的连接汇聚的两台交换机，组成高可用。主机两条聚合链路做成trunk并放行相应的vlan

#### 二、 拓扑图和说明

1、拓扑图

![](images/堆叠IRF+链路聚合.png)

2.说明:

+ 设备1(Device1)通过配置irf优先级为10（默认为1）成为IRF主设备
+ 设备2通过配置IRF编号为2，自动成为备设备
+ 设备IRF的g1/0/1和设备2的g2/0/1配置成一组链路聚合，如group 1。（等等，为啥是设备2的g2/0/1？哪里来的2？因为该设备的IRF编号改成了2，所以设备2的所有接口编号都是2了以后）
+ 设备IRF（设备1和2的组合）的g1/0/2和g2/0/2组成一组聚合链路，如group 2。
+ 设备IRF上创建vlan 10
+ 将设备的两个聚合链路设置成trunk，并且允许相应的vlan通过。
+ 将设备3的g1/0/1和g1/0/2组成聚合链路 group 1(和IRF的聚合链路group 1对应)，然后设置成trunk，并允许相应的vlan通过，创建vlan10，设置g1/0/3为vlan 10的access口
+ 将设备4的g1/0/1和g1/0/2组成聚合链路 group 2(和IRF的聚合链路group 2对应)，然后设置成trunk，并允许相应的vlan通过，创建vlan10，设置g1/0/3为vlan 10的access口

#### 三、实验步骤

+ 查看设备1的irf编号

```cmd
# Device1
[SW1]dis irf
MemberID    Role    Priority  CPU-Mac         Description
 *+1        Master  1         7ac3-0ab1-0104  ---
```

+ 调高设备1的irf优先级，优先级数值高了更优

```cmd
# Device1
[SW1]irf member 1 priority 10
[SW1]save
The current configuration will be written to the device. Are you sure? [Y/N]:y
```

+ 调整设备2的irf编号

```cmd
# Device2
[SW2]irf member 1 renumber 2
[SW2]save
The current configuration will be written to the device. Are you sure? [Y/N]:y
[SW2]quit
<SW2>reboot
```

+ 配置设备1的IRF

```cmd
# Device1
[SW1]int range f1/0/53 to f1/0/54
[SW1-if-range]shutdown

[SW1]irf-port 1/1
[SW1-irf-port1/1]port group int f1/0/53
[SW1-irf-port1/1]port group int f1/0/54

[SW1]int range f1/0/53 to f1/0/54
[SW1-if-range]undo shutdown

[SW1]save
[SW1]irf-port-configuration active
```

+ 配置设备2的IRF

```cmd
# Device2
[SW2]int range f2/0/53 to f2/0/54
[SW2-if-range]shutdown

[SW2]irf-port 2/2
[SW2-irf-port2/2]port group int f2/0/53
[SW2-irf-port2/2]port group int f2/0/54

[SW2]int range f2/0/53 to f2/0/54
[SW2-if-range]undo shutdown
[SW2-if-range]quit

[SW2]save
[SW2]irf-port-configuration active
```

+ 查看IRF设备状态（登录设备1或设备2都行），并更改设备主机名为IRF-SW（可选）

```cmd
# Device1
[SW1]sysname IRF-SW
[IRF-SW]dis irf
MemberID    Role    Priority  CPU-Mac         Description
 *+1        Master  10        7ac3-0ab1-0104  ---
   2        Standby 1         7ac3-0d06-0204  ---
--------------------------------------------------
 * indicates the device is the master.
 + indicates the device through which the user logs in.

 The bridge MAC of the IRF is: 7ac3-0ab1-0100
 Auto upgrade                : yes
 Mac persistent              : 6 min
 Domain ID                   : 0

```

+ 配置IRF的链路聚合

```cmd
# IRF-SW
# 在IRF-SW上配置聚合链路1(group 1)，将SW1的g1/0/1与SW2的g2/0/1做聚合
[IRF-SW]interface Bridge-Aggregation 1
[IRF-SW-Bridge-Aggregation1]int g1/0/1
[IRF-SW-GigabitEthernet1/0/1]port link-aggregation group 1
[IRF-SW-GigabitEthernet1/0/1]int g2/0/1
[IRF-SW-GigabitEthernet2/0/1]port link-aggregation group 1


# 在IRF-SW上配置聚合链路2(group 2)，将SW1的g1/0/2与SW2的g/2/0/2做聚合
[IRF-SW]int Bridge-Aggregation 2
[IRF-SW-Bridge-Aggregation2]int g1/0/2
[IRF-SW-GigabitEthernet1/0/2]port link-aggregation group 2
[IRF-SW-GigabitEthernet1/0/2]int g2/0/2
[IRF-SW-GigabitEthernet2/0/2]port link-aggregation group 2
```

+ 配置sw3的链路聚合、vlan

```cmd
# Device3
[SW3]int Bridge-Aggregation 1
[SW3-Bridge-Aggregation1]int g1/0/1
[SW3-GigabitEthernet1/0/1]port link-aggregation group 1
[SW3-GigabitEthernet1/0/1]int g1/0/2
[SW3-GigabitEthernet1/0/2]port link-aggregation group 1

[SW3]vlan 10
[SW3-vlan10]port g1/0/3
[SW3-vlan10]quit
```

+ 配置sw4的链路聚合、vlan

```cmd
# Device4
[SW4]int Bridge-Aggregation 2
[SW4-Bridge-Aggregation2]int g1/0/1
[SW4-GigabitEthernet1/0/1]port link-aggregation group 2
[SW4-GigabitEthernet1/0/1]int g1/0/2
[SW4-GigabitEthernet1/0/2]port link-aggregation group 2

[SW4]vlan 10
[SW4-vlan10]port g1/0/3
[SW4-vlan10]quit
```

+ 配置IRF、sw3、sw4的聚合端口为Trunk

```cmd
# 配置IRF的trunk和vlan
[IRF-SW]vlan 10
[IRF-SW-vlan10]quit

[IRF-SW]int Bridge-Aggregation 1
[IRF-SW-Bridge-Aggregation1]port link-type trunk
[IRF-SW-Bridge-Aggregation1]port trunk permit vlan 10

[IRF-SW]int Bridge-Aggregation 2
[IRF-SW-Bridge-Aggregation2]port link-type trunk
[IRF-SW-Bridge-Aggregation2]port trunk permit vlan 10

[IRF-SW]save

# 配置sw3的trunk
```

+ 给pc配置ip，并测试两台pc是否能通。