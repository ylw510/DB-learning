使用如下命令进行一键安装`obd`
```
bash -c "$(curl -s https://obbusiness-private.oss-cn-shanghai.aliyuncs.com/download-center/opensource/oceanbase-all-in-one/installer.sh)"
```
安装后`obd`的产物一般在`~/.obd`.

其主要文件目录为：
```
root@ubantu64:~# tree -L 1 ~/.obd
/root/.obd
├── config_parser
├── lock
├── log
├── mirror
├── optimize
├── plugins
├── version
└── workflows
```
其中占磁盘最大的目录为`mirror`：
```
root@ubantu64:~# du -hs ~/.obd/*
24K     /root/.obd/config_parser
4.0K    /root/.obd/lock
60K     /root/.obd/log
1.6G    /root/.obd/mirror
272K    /root/.obd/optimize
4.7M    /root/.obd/plugins
4.0K    /root/.obd/version
1.3M    /root/.obd/workflows
```
在第一次安装时，会把相关ob生态组件的rpm包一并下载下来，例如监控用的`grafana`，存指标数据`prometheus`，以及负载均衡组件`obproxy`, 诊断工具，性能测试工具等。
以上组件的rpm包会存放在`local`目录中。
```
root@ubantu64:~# tree -L 1 ~/.obd/mirror/
/root/.obd/mirror/
├── local
└── remote

2 directories, 0 files
root@ubantu64:~# tree -L 1 ~/.obd/mirror/local/
/root/.obd/mirror/local/
├── alertmanager-0.28.1-32025073111.el7.x86_64.rpm
├── grafana-7.5.17-1.x86_64.rpm
├── ob-configserver-1.0.0-2.el7.x86_64.rpm
├── ob-deploy-3.6.0-3.el7.x86_64.rpm
├── ob-sysbench-1.0.20-21.el7.x86_64.rpm
├── obagent-4.2.2-100000042024011120.el7.x86_64.rpm
├── obproxy-ce-4.3.5.0-3.el7.x86_64.rpm
├── obtpcc-5.0.0-1.el7.x86_64.rpm
├── obtpch-3.0.0-1.el7.x86_64.rpm
├── oceanbase-ce-4.2.1.8-108000022024072217.el7.x86_64.rpm
├── oceanbase-ce-4.3.5.4-104000042025090916.el7.x86_64.rpm
├── oceanbase-ce-libs-4.2.1.8-108000022024072217.el7.x86_64.rpm
├── oceanbase-ce-libs-4.3.5.4-104000042025090916.el7.x86_64.rpm
├── oceanbase-ce-utils-4.2.1.8-108000022024072217.el7.x86_64.rpm
├── oceanbase-ce-utils-4.3.5.4-104000042025090916.el7.x86_64.rpm
├── oceanbase-diagnostic-tool-3.6.0-22025080417.el7.x86_64.rpm
├── ocp-agent-ce-4.3.6-20250815135607.el7.aarch64.rpm
├── ocp-agent-ce-4.3.6-20250815135607.el7.x86_64.rpm
├── ocp-express-4.2.2-100000022024011120.el7.x86_64.rpm
├── ocp-server-ce-4.3.6-20250815135607.el7.noarch.rpm
├── openjdk-jre-1.8.0_322-b09.el7.x86_64.rpm
└── prometheus-2.37.1-10000102022110211.el7.x86_64.rpm

0 directories, 22 files
root@ubantu64:~# tree -L 1 ~/.obd/mirror/remote/
/root/.obd/mirror/remote/
└── OceanBase.repo

0 directories, 1 file
```
而`remote`目录通常放 OBD (OceanBase Deploy) 自带的镜像仓库配置，用于安装依赖或升级 OceanBase 相关软件。

使用`obd mirror list`便可查看`local`的内容：
```
root@ubantu64:~/.obd/mirror/remote# obd mirror list
[WARN] Use centos 7 remote mirror repository for ubuntu 22.04
[WARN] Use centos 7 remote mirror repository for ubuntu 22.04
+-----------------------------------------------------------------------------+
|                            Mirror Repository List                           |
+----------------------------+--------+---------+----------+------------------+
| SectionName                | Type   | Enabled | Avaiable | Update Time      |
+----------------------------+--------+---------+----------+------------------+
| local                      | local  | -       | True     | 2025-10-03 10:15 |
| oceanbase.community.stable | remote | False   | False    | 1970-01-01 00:00 |
| oceanbase.development-kit  | remote | False   | False    | 1970-01-01 00:00 |
+----------------------------+--------+---------+----------+------------------+
Use `obd mirror list <section name>` for more details
Trace ID: ed71bac6-a041-11f0-864c-000c291d2d34
If you want to view detailed obd logs, please run: obd display-trace ed71bac6-a041-11f0-864c-000c291d2d34
root@ubantu64:~/.obd/mirror/remote# obd mirror list local
+---------------------------------------------------------------------------------------------------------------------+
|                                                  local Package List                                                 |
+---------------------------+-----------+------------------------+---------+------------------------------------------+
| name                      | version   | release                | arch    | md5                                      |
+---------------------------+-----------+------------------------+---------+------------------------------------------+
| alertmanager              | 0.28.1    | 32025073111.el7        | x86_64  | c5fe05fcc8263b83f6d0602a871d7e1a7a79bdb8 |
| grafana                   | 7.5.17    | 1                      | x86_64  | 1bf1f338d3a3445d8599dc6902e7aeed4de4e0d6 |
| ob-configserver           | 1.0.0     | 2.el7                  | x86_64  | feca6b9c76e26ac49464f34bfa0780b5a8d3f4a0 |
| ob-deploy                 | 3.6.0     | 3.el7                  | x86_64  | c48f82f988f8da512a8dc0bf0948b9584c3e7a34 |
| ob-sysbench               | 1.0.20    | 21.el7                 | x86_64  | 34eb6ecba0ebc4c31c4cfa01162045cbbbec55f7 |
| obagent                   | 4.2.2     | 100000042024011120.el7 | x86_64  | 19739a07a12eab736aff86ecf357b1ae660b554e |
| obproxy-ce                | 4.3.5.0   | 3.el7                  | x86_64  | f17b277b681adb1c86bfc3cfda369ad88896da9d |
| obtpcc                    | 5.0.0     | 1.el7                  | x86_64  | 8624590be4bfe16f28bdd9fc5e4849cda19577d6 |
| obtpch                    | 3.0.0     | 1.el7                  | x86_64  | 3e3e88f87527677998fedf25087f5c87779dee62 |
| oceanbase-ce              | 4.2.1.8   | 108000022024072217.el7 | x86_64  | 499b676f2ede5a16e0c07b2b15991d1160d972e8 |
| oceanbase-ce              | 4.3.5.4   | 104000042025090916.el7 | x86_64  | a30a15dd7a80f6acd0f113993bb70b2e56d40f80 |
| oceanbase-ce-libs         | 4.2.1.8   | 108000022024072217.el7 | x86_64  | d02f4bfd321370a02550424293beb1be31204038 |
| oceanbase-ce-libs         | 4.3.5.4   | 104000042025090916.el7 | x86_64  | 86d1bfdde0e48da54f81a5e9eb09e6d3ad67f0d9 |
| oceanbase-ce-utils        | 4.2.1.8   | 108000022024072217.el7 | x86_64  | 6f87392f95b399a21382323f256cfda5969375c4 |
| oceanbase-ce-utils        | 4.3.5.4   | 104000042025090916.el7 | x86_64  | eac57ef76301a631519ce0b0794b373dee3fd595 |
| oceanbase-diagnostic-tool | 3.6.0     | 22025080417.el7        | x86_64  | ce431826e5571a6e00d81b9849e32eb53991169f |
| ocp-agent-ce              | 4.3.6     | 20250815135607.el7     | aarch64 | 0cd43c9c59ff3f0b5133c21fc394611d95fa9974 |
| ocp-agent-ce              | 4.3.6     | 20250815135607.el7     | x86_64  | d7a743e37308bdb88678edcc5370ab770c2c72b2 |
| ocp-express               | 4.2.2     | 100000022024011120.el7 | x86_64  | 09ffcf156d1df9318a78af52656f499d2315e3f7 |
| ocp-server-ce             | 4.3.6     | 20250815135607.el7     | noarch  | a8ec6271c5b8fa11c068dd396dcba763510e2adc |
| openjdk-jre               | 1.8.0_322 | b09.el7                | x86_64  | 051aa69c5abb8697d15c2f0dcb1392b3f815f7ed |
| prometheus                | 2.37.1    | 10000102022110211.el7  | x86_64  | 58913c7606f05feb01bc1c6410346e5fc31cf263 |
+---------------------------+-----------+------------------------+---------+------------------------------------------+
Trace ID: ef67a796-a041-11f0-82dd-000c291d2d34
If you want to view detailed obd logs, please run: obd display-trace ef67a796-a041-11f0-82dd-000c291d2d34
```

