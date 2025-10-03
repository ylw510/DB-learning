[实践：OBD部署源码编译OceanBase社区版本-数据库技术博客-OceanBase分布式数据库](https://open.oceanbase.com/blog/3501926400)



1.进入编译好的`build_debug`目录

```
[yanglw@139 ~]$ cd oceanbase/build_debug/
[yanglw@139 build_debug]$ pwd
/home/yanglw/oceanbase/build_debug
```

2.执行`make install`，默认安装到`/usr/local`下

```
make install

[yanglw@139 local]$ ls
admin  bin  etc  games  include  lib  lib64  libexec  pgsql  profile  sbin  share  src
[yanglw@139 local]$ cd bin/
[yanglw@139 bin]$ ls
ex  import_srs_data.py  import_time_zone_info.py  observer  obshell  rview  rvim  view  vim  vimdiff  vimtutor  xxd

```

安装文件很多，分布在local的各个目录

3.取消远程镜像

```
[yanglw@139 bin]$ obd mirror disable remote
Disable remote ok
Trace ID: 5b06ce26-1f1c-11ef-b6bc-000c297ca7a3
If you want to view detailed obd logs, please run: obd display-trace 5b06ce26-1f1c-11ef-b6bc-000c297ca7a3
```

4.关联本地镜像

例如，如果您根据文档 [使用源码构建 OceanBase 数据库](https://www.oceanbase.com/docs/community-observer-cn-0000000000160092) 编译 OceanBase 数据库，在编译成功后，可以使用 `make DESTDIR=./ install && obd mirror create -n oceanbase-ce -V <component version> -p ./usr/local` 命令将编译产物添加至 OBD 本地仓库。

```
obd mirror create -n oceanbase-ce -V 4.3.2.0 -p /usr/local/ -t my-oceanbase
```

具体各个参数可以参考[镜像和仓库命令组-OceanBase 安装部署工具-OceanBase文档中心-分布式数据库使用文档](https://www.oceanbase.com/docs/community-obd-cn-1000000000197052)

5.部署

```
obd cluster deploy myob -c mini-config.yaml
```

6.启动

```
obd obd cluster start myob
```

7.GDB调试

```
obd tool command myob gdb
```

注意：

每次编译好新的版本之后，需要修改部署配置文件中的package_hash。

这个hash值由obd mirror create得到。

使用`-f`选项覆盖之前创建好的版本
