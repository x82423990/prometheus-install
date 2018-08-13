### 下载自动化脚本

```
wget -O  /tmp/init.sh http://dwonload.hd1pro.boyacx.com/monitor/scripts/init.sh
```

### 初始化脚本

```
bash /tmp/init.sh elasticsearch hd1 bycx
```

##### 解释说明

参数解释

* $1 为要监控的服务如 elasticsearch 代表将要监控es
* $2 为监控tag，目前该tag为地区tag，如hd1代表华东1一区
* $3 为公司名字，如bycx代表博雅成信

E.g

要监控主机为192.168.1.100上的redis，该主机位于华南1一区，公司名字为博雅成信



```
ssh 192.168.1.100 (or use ansible)
bash /tmp/init.sh redis hn1 bycx
```

##### 服务解释

执行该脚本后， 会初始化agent， 然后去consul上去注册，会生成sysytemctl启动脚本，会在/etc/sysystemd/sysyte/$servicename.d/下生成配置文件。

mysql 需要在该文件夹下添加.my.cnf
