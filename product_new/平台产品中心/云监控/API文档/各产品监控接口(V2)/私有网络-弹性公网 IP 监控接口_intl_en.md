## 1. API Description

API: GetMonitorData

Domain name for API request: `monitor.tencentcloudapi.com`

This API is used to get the monitoring data of a Tencent Cloud product by passing in the product's namespace, object dimension, and monitoring metric. 

API call rate limit: 20 calls/second (1,200 calls/minute). A single request can get the data of up to 10 instances and up to 1,440 data points.

This API may fail due to the rate limit if you need to call a lot of metrics and objects. We recommend spreading call requests across a period of time.

To query the monitoring data of an EIP in a VPC, use the following input parameters:

&Namespace=QCE/LB

&Instances.N.Dimensions.0.Name=eip

&Instances.N.Dimensions.0.Value=unique EIP ID


## 2. Input Parameters


The list below contains only the API request parameters and certain common request parameters. Common request parameters need to be added when a call is made. For more information, please see [Common Request Parameters](https://intl.cloud.tencent.com/document/api/248/4478).

### 2.1. Input parameters
#### 2.1.1. Overview of input parameters
| Parameter Name | Required | Type | Description |
| ----------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Action | Yes | String | Common parameter. The value used for this API: GetMonitorData |
| Version | Yes | String | Common parameter. The value used for this API: 2018-07-24 |
| Region | No | String | Common parameter, indicating the region of the instance to be queried. For supported regions, please see the [list of regions](https://intl.cloud.tencent.com/document/api/213/15692) supported by CVM |
| Namespace | Yes | String | Namespace. Each Tencent Cloud product has a namespace, such as `QCE/LB`. This value must be capitalized for API 3.0 |
| MetricName | Yes | String | Metric name. For more information, please see section 2.2 |
| Instances.N | Yes | Array of [Instance](https://intl.cloud.tencent.com/document/product/248/33883) | Combination of instance object dimensions |
| Period | No | Integer | Statistical period for monitoring data in seconds. Default value: 300 |
| StartTime | No | Datetime | Start time, such as "2016-01-01 10:25:00". Default value: "00:00:00" on the current day |
| EndTime | No | Timestamp | End time, which is the current time by default. `EndTime` cannot be earlier than `StartTime` |

#### 2.1.2. Overview of parameters in each dimension

| Parameter Name | Dimension Name | Dimension Description | Format |
| ------------------ | ---------------- | ------------- | ----------------------------- |
| Instances.N.Dimensions.0.Name | eip | EIP ID | String-type dimension name: eip |
| Instances.N.Dimensions.0.Value | eip | Specific EIP address | Specific IP address, such as 111.111.111.11 |

### 2.2. Metric name

The statistical granularity (`period`) and dimension (`dimension`) may vary by metric. The [DescribeBaseMetrics](https://cloud.tencent.com/document/product/248/30351) API can be used to get the `period` and `dimension` supported by each metric.

| Metric Name | Description | Unit | Dimension |
| -------------- | ---------- | ----- | ---- |
| VipOuttraffic | Public network outbound bandwidth | Mbps | EIP |
| VipIntraffic | Public network inbound bandwidth | Mbps | EIP |
| VipOutpkg | Outbound packets | Packets/sec | EIP |
| VipInpkg | Inbound packets | Packets/sec | EIP |



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
This example shows you how to get the public network outbound bandwidth of one EIP in a VPC using a statistical period of 60 seconds for a specified length of time.

#### Input sample code

```
https://monitor.tencentcloudapi.com/?Action=GetMonitorData
&Namespace=QCE/LB
&MetricName=VipOuttraffic
&Period=60
&StartTime=2019-06-11T16:00:00+08:00
&EndTime=2018-05-08T16:05:00+08:00
&Instances.0.Dimensions.0.Name=eip
&Instances.0.Dimensions.0.Value=111.111.111.11
&<Common request parameters>
```

#### Output sample code

```
{
"Response": {
"StartTime": "2019-06-11 16:00:00",
"EndTime": "2019-06-11 16:05:00",
"Period": 60,
"MetricName": "VipOuttraffic",
"DataPoints": [
{
"Dimensions": [
{
"Name": "eip",
"Value": "111.111.111.11"
}
],
"Timestamps": [
1560240000,
1560240060,
1560240120,
1560240180,
1560240240,
1560240300
],
"Values": [
0.00006,
0.002,
0.01,
0.008,
0.0001,
0
]
}
],
"RequestId": "a9b717b5-5cff-4ff9-ab90-78f13556c897"
}
}
```
### Sample 2. Getting public network outbound bandwidth of multiple EIPs in VPC
This example shows you how to get the public network outbound bandwidth of multiple EIPs in a VPC using a statistical period of 60 seconds for a specified length of time.

#### Input sample code

```
https://monitor.tencentcloudapi.com/?Action=GetMonitorData
&Namespace=QCE/LB
&MetricName=VipOuttraffic
&Period=60
&StartTime=2019-06-11T16:00:00+08:00
&EndTime=2018-05-08T16:05:00+08:00
&Instances.0.Dimensions.0.Name=eip
&Instances.0.Dimensions.0.Value=111.111.111.11
&Instances.1.Dimensions.0.Name=eip
&Instances.1.Dimensions.0.Value=111.111.111.22
&<Common request parameters>
```

#### Output sample code

```
{
"Response": {
"StartTime": "2019-06-11 16:00:00",
"EndTime": "2019-06-11 16:05:00",
"Period": 60,
"MetricName": "VipOuttraffic",
"DataPoints": [
{
"Dimensions": [
{
"Name": "eip",
"Value": "111.111.111.11"
}
],
"Timestamps": [
1560240000,
1560240060,
1560240120,
1560240180,
1560240240,
1560240300
],
"Values": [
0.00006,
0.002,
0.01,
0.008,
0.0001,
0
]
},
{
"Dimensions": [
{
"Name": "eip",
"Value": "111.111.111.22"
}
],
"Timestamps": [
1560240000,
1560240060,
1560240120,
1560240180,
1560240240,
1560240300
],
"Values": [
0.00006,
0.002,
0.01,
0.008,
0.0001,
0
]
}
],
"RequestId": "a9b717b5-5cff-4ff9-ab90-78f13556c897"
}
}
```