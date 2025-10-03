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
