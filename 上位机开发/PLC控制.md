```python
# 连接IO
IO_A.inst = M100()
IO_A.inst.connect()
StepCBB.step_cbb_step_result('连接IO控制器(USR_M100)', True, True)



```



万用表控制
```python
# 连接万用表
DM_A.inst = create_dm(DM_A_PRODUCT, ip=DM_A_IP, serial_port=DM_A_SERIAL_PORT)
DM_A.inst.connect()
StepCBB.step_cbb_step_result('连接万用表', True, True)

# 测量下浮插CAN终端电阻
MicroPackCBB.dm_pbl_set_res("万用表设置成测量电阻模式")
MicroPackCBB.dm_pbl_set_range("万用表电阻量程设置成1K", "1K")
MicroPackCBB.io_cl_set("闭合开关KCANH1", 1, SWITCH_KCANH1)
MicroPackCBB.io_cl_set("闭合开关KCANL1", 1, SWITCH_KCANL1)
# todo: CAN的终端电阻是129Ω，需要重新确认
MicroPackCBB.dm_pbl_get_res("CAN终端电阻", [110, 140])
MicroPackCBB.io_cl_set("断开开关KCANH1", 0, SWITCH_KCANH1)
MicroPackCBB.io_cl_set("断开开关KCANL1", 0, SWITCH_KCANL1)





```