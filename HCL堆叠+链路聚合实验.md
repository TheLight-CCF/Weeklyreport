<center><h2>
    HCL堆叠+链路聚合实验
    </h2></center>



![image-20241121193250429](C:\Users\DoubLeMoon_Bird\AppData\Roaming\Typora\typora-user-images\image-20241121193250429.png)

+ 实验配置及命令

```cmd
# IRFSW1
#irf 区域设置，并设置member 1的优先级。（默认编号也是1，最大是32）
[IRFSW1]irf domain 10
[IRFSW1]irf member 1 priority 10
#先关闭端口，然后创建irf-port 1/1（member号/逻辑口编号），然后将物理口加入，再开启刚关闭的端口。
[IRFSW1]int range fge1/0/53 to fge1/0/54
[IRFSW1-if-range]shutdown
[IRFSW1-if-range]quit

[IRFSW1]irf-port 1/1
[IRFSW1-irf-port1/1]port group interface fge1/0/53
[IRFSW1-irf-port1/1]port group interface fge1/0/54

[IRFSW1]int range fge1/0/53 to fge1/0/54
[IRFSW1-if-range]undo shutdown
```

```cmd
# IRFSW2
[IRFSW2]irf domain 10
[IRFSW2]irf member 1 renumber 2
Renumbering the member ID may result in configuration change or loss. Continue?[Y/N]:y

[IRFSW2]save
The current configuration will be written to the device. Are you sure? [Y/N]:y

[IRFSW2]quit
<IRFSW2>reboot
Start to check configuration with next startup configuration file, please wait.........DONE!
This command will reboot the device. Continue? [Y/N]:y

[IRFSW2]int range fge2/0/53 to fge2/0/54
[IRFSW2-if-range]undo shut
[IRFSW2-if-range]undo shutdown

[IRFSW2]irf-port-configuration active
<IRFSW2>reboot

```

