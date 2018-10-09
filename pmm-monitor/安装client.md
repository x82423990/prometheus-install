##### pmm-client

```shell
sudo yum install http://dwonload.hd1pro.boyacx.com/monitor/app/pmm-client-1.14.1-1.el7.x86_64.rpm -y
IP=`ip a | grep eth0 | grep inet | awk '{print $2}' | cut -d/ -f1`
pmm-admin config --client-address $IP --bind-address $IP --server  mysql-ui.hd1pro.boyacx.com
pmm-admin add mysql

## 给每台安装一个额外的node_exporter，因为系统集成的不能到出道我们目前的用的prometheus上
wget -O  /tmp/init.sh http://dwonload.hd1pro.boyacx.com/monitor/scripts/init.sh
bash -x /tmp/init.sh node hd1 bycx
systemctl daemon-reload
systemctl restart node_exporter
systemctl status node_exporter
```

