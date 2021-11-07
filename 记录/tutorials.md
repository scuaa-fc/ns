# 简单实例说明

在`contrib/basic-sim/buildexample_run_folders`中, 有很多实例

```
fat_tree_k4_servers
fat_tree_k4_servers_poisson
grid
leaf_spine
leaf_spine_2_core
leaf_spine_servers
one_tcp_flow
one_udp_burst
one_udp_ping
ring
single
single_everything
tutorial

```
这里面的每一个, 都可以作为输入,通过`main-full`脚本程序处理.

`run_one_tcp_flow`

**1. ns3_config.properties**

```properties
#20s
simulation_end_time_ns=20000000000
simulation_seed=123456789

topology_ptop_filename="topology_one.properties"

tcp_config=basic

enable_link_net_device_queue_tracking=true

enable_link_net_device_utilization_tracking=true
link_net_device_utilization_tracking_interval_ns=100000000

enable_tcp_flow_scheduler=true
tcp_flow_schedule_filename="tcp_flow_schedule.csv"
tcp_flow_enable_logging_for_tcp_flow_ids=set(0)

```

**topology.properties**

```properties
# Single link topology
#
# 0 --- 1

num_nodes=2
num_undirected_edges=1
switches=set(0,1)
switches_which_are_tors=set(0,1)
servers=set()
undirected_edges=set(0-1)

link_channel_delay_ns=10000
link_net_device_data_rate_megabit_per_s=10.0
link_net_device_queue=drop_tail(100p)
link_net_device_receive_error_model=iid_uniform_random_pkt(0.0)
link_interface_traffic_control_qdisc=disabled
```

**tcp_flow_schedule.csv**
```
0,0,1,10000000,0,,
```
注意, 这里scheduler中数据量的单位是1 Byte, = 8bits, 所以尽量写成 125000Byte = 1Mbits 的倍数


**执行指令**


```bash
waf step

waf --run="main-full --run_dir='ns3ws/data/one_tcp_flow'"

run_folder="one_tcp_flow"
cd contrib/basic-sim/tools/plotting/plot_tcp_flow 
python plot_tcp_flow.py \
/home/roit/aws/ns3ws/data/${run_folder}/logs_ns3  \
/home/roit/aws/ns3ws/data/${run_folder}/plt_data_out_dir_0  \
/home/roit/aws/ns3ws/data/${run_folder}/pdf_out_dir_0 \
0 10000000
cd ../../../../.. 

```

