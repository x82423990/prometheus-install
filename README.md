### Prometheus 是什么？

Prometheus是一套开源的监控&报警&时间序列数据库的组合，起始是由[SoundCloud](https://soundcloud.com/)公司开发的。随着发展，越来越多公司和组织接受采用Prometheus，社区也十分活跃，他们便将它独立成开源项目，并且有公司来运作。google SRE的书内也曾提到跟他们BorgMon监控系统相似的实现是Prometheus。现在最常见的Kubernetes容器管理系统中，通常会搭配Prometheus进行监控。

#### Prometheus 的优点

*   非常少的外部依赖，安装使用超简单
*   已经有非常多的系统集成 例如：docker HAProxy Nginx JMX等等
*   服务自动化发现
*   直接集成到代码
*   设计思想是按照分布式、微服务架构来实现的

#### Prometheus 的特性

*   自定义多维度的数据模型
*   非常高效的存储 平均一个采样数据占 ~3.5 bytes左右，320万的时间序列，每30秒采样，保持60天，消耗磁盘大概228G。
*   强大的查询语句
*   轻松实现数据可视化
#### 架构图
![prometheus 架构图](http://upload-images.jianshu.io/upload_images/4691863-8629758941834d71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 组件介绍
Prometheus生态系统由多个组件组成。其中许多组件都是可选的

###### Promethus  server
* 必须安装，
* 本质是一个时序数据库
* 主要负责数据pull、存储、分析

######  Push Gateway
* 非必选项
* 支持临时性Job主动推送指标的中间网关

###### exporters
* 部署在客户端的agent,如 node_exporte, mysql_exporter等

常用的exporter

* node_exporter
* mysql_exporter
* redis_exporter
* container_exporter
* elasticsearch_exporter
* cadvisor
* 等等。。


###### alertmanager

* 用来进行报警，Promethus server 经过分析， 把出发的警报发送给 alertmanager 组件，alertmanager 组件通过自身的规则，来发送通知,(邮件，或者webhook)

###### UI 展示
* 我们采用的grafana,项目地址:https://grafana.com/



---




