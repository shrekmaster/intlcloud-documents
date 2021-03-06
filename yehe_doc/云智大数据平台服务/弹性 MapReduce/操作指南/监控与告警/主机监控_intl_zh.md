主机监控页面展示了当前集群所有主机监控概览和所有节点列表，并支持查看所有主机热点图。

## 查看主机监控
1. 登录 [EMR 控制台](https://console.cloud.tencent.com/emr)，单击左侧栏【集群监控】，进入集群监控页。
2. 选择【主机监控】可查看集群中所有主机监控信息。
3. 在主机监控中，可查当前集群所有主机聚合监控指标概览和所有节点列表。
![](https://main.qcloudimg.com/raw/77d9387e976207a09337ccc4c779e65a.png)
 - 【概览】可直观查看对应时间段所有主机聚合监控指标及指标各项统计规则，默认最多展示6个指标项。可单击【设置指标】自定义展示指标。
![](https://main.qcloudimg.com/raw/b08401412fca47bbecb0613eab712a74.png)
![](https://main.qcloudimg.com/raw/e89fdedf8746428dfd9194ad682cceb7.png)
 - 【热点图】负载热点图更加直观的展示了主机的负载情况，同时可指定时间段和负载条件进行查看。负载热点图主要分为两部分，一部分为当前集群所有主机聚合负载图；一部分为所有节点单个热点图，可直观查看所有节点的负载情况。
![](https://main.qcloudimg.com/raw/d554d19dbdddfe237dc568e6652d80af.png)
 - 【节点列表】展示了当前所有节点和部署节点类型、CPU 利用率、内存利用率、磁盘利用率。单击对应节点 IP 可查看单个节点基本配置、部署状态、负载状态、主机监控等。
    - 基本配置：可查看当前主机的基本信息，例如节点类型、实例 ID、资源 ID、计费类型、规格大小等。
![](https://main.qcloudimg.com/raw/730e56ba0c3925ee505416a4d6c0ae04.png)
    - 部署状态：可查看当前主机的服务部署情况，是否为标准进程、进程状态是否运行正常等。
![](https://main.qcloudimg.com/raw/08c2b407045df6cfdc78c88b9f4e9cfb.png)
    - 负载状态：可查看当前主机维度 TOP N 进程情况，同时可根据指定时间进行查看。
![](https://main.qcloudimg.com/raw/3db9aa05fe62c525e8ee8590e4eff34c.png)
    - 主机监控：可查看当前主机各分组指标负载趋势图，默认展示6个指标，最多可展示12个指标。可单击【设置指标】自定义展示指标。
   ![](https://main.qcloudimg.com/raw/f82b8bbb04440eb757de5932e1d74360.png)

