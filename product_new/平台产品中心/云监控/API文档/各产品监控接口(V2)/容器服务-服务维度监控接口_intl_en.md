## 1. API Description

API: GetMonitorData

Domain name for API request: `monitor.tencentcloudapi.com`

This API is used to get the monitoring data of a Tencent Cloud product by passing in the product's namespace, object dimension, and monitoring metric. 

API call rate limit: 20 calls/second (1,200 calls/minute). A single request can get the data of up to 10 instances and up to 1,440 data points.

This API may fail due to the rate limit if you need to call a lot of metrics and objects. We recommend spreading call requests across a period of time.

To query the monitoring data of TKE in the service dimension, use the following input parameters:

&Namespace=QCE/DOCKER

&Instances.N.Dimensions.0.Name=clusterId

&Instances.N.Dimensions.0.Value=cluster ID


## 2. Input Parameters

The list below contains only the API request parameters and certain common request parameters. Common request parameters need to be added when a call is made. For more information, please see [Common Request Parameters](https://intl.cloud.tencent.com/document/api/248/4478).

### 2.1. Input parameters

#### 2.1.1. Overview of input parameters

| Parameter Name | Required | Type | Description |
| ----------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Action | Yes | String | Common parameter. The value used for this API: GetMonitorData |
| Version | Yes | String | Common parameter. The value used for this API: 2018-07-24 |
| Region | No | String | Common parameter, indicating the region of the instance to be queried. For supported regions, please see the [list of regions](https://intl.cloud.tencent.com/document/api/213/15692) supported by CVM |
| Namespace | Yes | String | Namespace. Each Tencent Cloud product has a namespace such as `QCE/DOCKER`. This value must be capitalized for API 3.0 |
| MetricName | Yes | String | Metric name. For more information, please see section 2.2 |
| Instances.N | Yes | Array of [Instance](https://intl.cloud.tencent.com/document/product/248/33883) | Combination of instance object dimensions |
| Period | No | Integer | Statistical period for monitoring data in seconds. Default value: 300 |
| StartTime | No | Datetime | Start time, such as "2016-01-01 10:25:00". Default value: "00:00:00" on the current day |
| EndTime | No | Timestamp | End time, which is the current time by default. `endTime` cannot be earlier than `startTime` |

#### 2.1.2. Overview of parameters in each dimension

| Parameter Name | Dimension Name | Dimension Description | Format |
| ------------------------------ | ----------- | ------------------------------------------------------------ | -------------------------------- |
| Instances.N.Dimensions.0.Name | clusterId | Cluster ID | String-type dimension name: clusterId |
| Instances.N.Dimensions.0.Value | clusterId | Specific cluster ID, i.e., the `clusterId` field returned by the DescribeClusters API | Specific cluster name, such as cls-xxxxx |
| Instances.N.Dimensions.1.Name | serviceName | Service name | String-type dimension name: serviceName |
| Instances.N.Dimensions.1.Value | serviceName | Specific service name, i.e., the `serviceName` field returned by the DescribeClusterService API | Specific service name, such as test |
| Instances.N.Dimensions.2.Name | namespace | Namespace name | String-type dimension name: namespace |
| Instances.N.Dimensions.2.Value | namespace | Specific namespace name, i.e., the `namespace` field returned by the DescribeClusterService API | Specific namespace name, such as default |



### 2.2. Metric name

The statistical granularity (`period`) and dimension (`dimension`) may vary by metric. The [DescribeBaseMetrics](https://cloud.tencent.com/document/product/248/30351) API can be used to get the `period` and `dimension` supported by each metric.

| Monitoring Item | Metric Name | Unit | Description |
| ------------------------- | ------------------------- | ---- | ------------------------------------ |
| Service CPU usage | ServiceCpuUsed | Cores | Sum of CPU usage of all pods in service |
| Service CPU utilization (for cluster) | ServiceCpuUsageForCluster | % | Service CPU utilization for cluster |
| Service memory usage | ServiceMemUsed | MiB | Sum of memory usage of all pods in service |
| Service memory utilization (for cluster) | ServiceMemUsageForCluster | % | Service memory utilization for cluster |
| Service network inbound traffic | ServiceInFlux | MB | Sum of inbound traffic of all pods in service in time window |
| Service network outbound traffic | ServiceOutFlux | MB | Sum of outbound traffic of all pods in service in time window |
| Service network inbound bandwidth | ServiceInBandwidth | Mbps | Sum of inbound bandwidth of all pods in service |
| Service network outbound bandwidth | ServiceOutBandwidth | Mbps | Sum of outbound bandwidth of all pods in service |
| Service network inbound packets | ServiceInPackets | Packets/sec | Sum of inbound packets of all pods in service |
| Service network outbound packets | ServiceOutPackets | Packets/sec | Sum of outbound packets of all pods in service |

## 3. Output Parameters

| Parameter Name | Type | Description |
| ---------- | --------------------- | ------------------------------------------------------------ |
| MetricName | String | Monitoring metric |
| StartTime | Timestamp | Data point start time |
| EndTime | Timestamp | Data point end time |
| Period | Integer | Statistical period |
| DataPoints | Array of PointsObject | Monitoring data list |
| RequestId | String | Unique ID of request. Each request returns a unique ID. The `RequestId` is required to troubleshoot issues |

## 4. Error Codes

| Error Code | Error Description | Error Message |
| -------- | -------------- | ------------------------------------ |
| -502 | The resource does not exist. | OperationDenied.SourceNotExists |
| -503 | Incorrect request parameter. | InvalidParameter |
| -505 | Missing parameter. | InvalidParameter.MissingParameter |
| -507 | Limit exceeded. | OperationDenied.ExceedLimit |
| -509 | Incorrect combination of dimensions. | InvalidParameter.DimensionGroupError |
| -513 | Database operation failed. | InternalError.DBoperationFail |

## 5. Samples

### Sample 1 

This example shows you how to get the service CPU usage of one TKE service using a statistical period of 60 seconds for a specified length of time.

#### Input sample code

```
https://monitor.tencentcloudapi.com/?Action=GetMonitorData
&Namespace=QCE/DOCKER
&MetricName=ServiceCpuUsed
&Period=60
&StartTime=2019-06-17T16:35:00+08:00
&EndTime=2019-06-17T16:40:00+08:00
&Instances.0.Dimensions.0.Name=clusterId
&Instances.0.Dimensions.0.Value=cls-12345
&Instances.0.Dimensions.1.Name=namespace
&Instances.0.Dimensions.1.Value=default
&Instances.0.Dimensions.2.Name=serviceName
&Instances.0.Dimensions.2.Value=monitor
&<Common request parameters>
```

#### Output sample code

```
{
"Response": {
"StartTime": "2019-06-17 16:35:00",
"EndTime": "2019-06-17 16:40:00",
"Period": 60,
"MetricName": "ServiceCpuUsed",
"DataPoints": [
{
"Dimensions": [
{
"Name": "clusterId",
"Value": "cls-12345"
},
{
"Name": "namespace",
"Value": "default"
},
{
"Name": "serviceName",
"Value": "monitor"
}
],
"Timestamps": [
1560760500,
1560760560,
1560760620,
1560760680,
1560760740,
1560760800
],
"Values": [
0.000004,
0.000002,
0.000005,
0.000001,
0.000004,
0.000002
]
}
],
"RequestId": "7dde43ca-9fba-4ef9-8973-d0007a7dc628"
}
}
```

### Sample 2 

This example shows you how to get the service CPU usage of multiple TKE services using a statistical period of 60 seconds for a specified length of time.

#### Input sample code

```
https://monitor.tencentcloudapi.com/?Action=GetMonitorData
&Namespace=QCE/DOCKER
&MetricName=ServiceCpuUsed
&Period=60
&StartTime=2019-06-17T16:35:00+08:00
&EndTime=2019-06-17T16:40:00+08:00
&Instances.0.Dimensions.0.Name=clusterId
&Instances.0.Dimensions.0.Value=cls-12345
&Instances.0.Dimensions.1.Name=namespace
&Instances.0.Dimensions.1.Value=default
&Instances.0.Dimensions.2.Name=serviceName
&Instances.0.Dimensions.2.Value=monitor
&Instances.1.Dimensions.0.Name=clusterId
&Instances.1.Dimensions.0.Value=cls-123456
&Instances.1.Dimensions.1.Name=namespace
&Instances.1.Dimensions.1.Value=default
&Instances.1.Dimensions.2.Name=serviceName
&Instances.1.Dimensions.2.Value=monitor
&<Common request parameters>
```

#### Output sample code

```
{
"Response": {
"StartTime": "2019-06-17 16:35:00",
"EndTime": "2019-06-17 16:40:00",
"Period": 60,
"MetricName": "ServiceCpuUsed",
"DataPoints": [
{
"Dimensions": [
{
"Name": "clusterId",
"Value": "cls-12345"
},
{
"Name": "namespace",
"Value": "default"
},
{
"Name": "serviceName",
"Value": "monitor"
}
],
"Timestamps": [
1560760500,
1560760560,
1560760620,
1560760680,
1560760740,
1560760800
],
"Values": [
0.000004,
0.000002,
0.000005,
0.000001,
0.000004,
0.000002
]
},
{
"Dimensions": [
{
"Name": "clusterId",
"Value": "cls-123456"
},
{
"Name": "namespace",
"Value": "default"
},
{
"Name": "serviceName",
"Value": "monitor"
}
],
"Timestamps": [
1560760500,
1560760560,
1560760620,
1560760680,
1560760740,
1560760800
],
"Values": [
0.000004,
0.000002,
0.000005,
0.000001,
0.000004,
0.000002
]
}
],
"RequestId": "7dde43ca-9fba-4ef9-8973-d0007a7dc628"
}
}
```