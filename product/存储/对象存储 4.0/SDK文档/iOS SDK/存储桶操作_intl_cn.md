## 简介

本文档重点提供关于存储桶的基本操作和访问控制列表（ACL）的相关 API 概览以及 SDK 示例代码。

- 我们假设您已经按照 [快速入门](https://intl.cloud.tencent.com/document/product/436/11280) 文档中的指引完成了 SDK 下载、安装和初始化的过程。
- 查询时建议使用 Command+F 搜索到想要查询的接口，然后看我们给出的接口简单说明，复制示例到您的工程中运行。

> ?如果需要了解接口的功能或者参数的意义，建议直接查看代码里的注释，Xcode 支持通过三指轻按、Force-touch 重按或者将鼠标停留在变量上，按 Control+Command+D 的方式查看它的释义。   

**基本操作**

| API                                                          | 操作名             | 操作描述                           |
| ------------------------------------------------------------ | ------------------ | ---------------------------------- |
| [GET Service](https://intl.cloud.tencent.com/document/product/436/8291) | 获取存储桶列表     | 获取指定账号下所有的存储桶列表     |
| [PUT Bucket](https://intl.cloud.tencent.com/document/product/436/7738) | 创建存储桶         | 在指定账号下创建一个存储桶         |
| [HEAD Bucket](https://intl.cloud.tencent.com/document/product/436/7735) | 检索存储桶及其权限 | 检索存储桶是否存在且是否有权限访问 |
| GET Bucket location | 获取存储桶地域信息 | 获取存储桶所在的地域信息           |
| [DELETE Bucket](https://intl.cloud.tencent.com/document/product/436/7732) | 删除存储桶         | 删除指定账号下的空存储桶           |

**访问控制列表**

| API                                                          | 操作名         | 操作描述              |
| ------------------------------------------------------------ | -------------- | --------------------- |
| [PUT Bucket acl](https://intl.cloud.tencent.com/document/product/436/7737) | 设置存储桶 ACL | 设置存储桶的 ACL 配置 |
| [GET Bucket acl](https://intl.cloud.tencent.com/document/product/436/7733) | 获取存储桶 ACL | 获取存储桶的 ACL 配置 |

## 基本操作

### 获取存储桶列表

#### 功能说明

用来获取指定账号下所有存储桶列表（Bucket list）。

#### 返回结果说明

通过 QCloudListAllMyBucketsResult 返回请求结果。

| 参数名称 | 描述               | 类型                           |
| -------- | ------------------ | ------------------------------ |
| owner    | 存储桶持有者的信息 | QCloudOwner *                  |
| buckets  | 所有的 bucket 信息 | NSArray&lt;QCloudBucket`*` > * |

QCloudOwner  参数说明

| 参数名称    | 描述               | 类型       |
| ----------- | ------------------ | ---------- |
| displayName | 持有者的名称       | NSString * |
| identifier  | Bucket 所有者的 ID | NSString * |

QCloudBucket  参数说明

| 参数名称   | 描述                                                         | 类型       |
| ---------- | ------------------------------------------------------------ | ---------- |
| name       | Bucket 的名称                                                | NSString * |
| location   | Bucket 所在地域。枚举值请参阅 [地域和访问域名](https://intl.cloud.tencent.com/document/product/436/6224) 文档，如：ap-beijing，ap-hongkong，eu-frankfurt 等 | NSString * |
| createDate | Bucket 创建时间。ISO8601 格式，例如 2016-11-09T08:46:32.000Z | NSString * |

#### 请求示例

```
QCloudGetServiceRequest* request = [[QCloudGetServiceRequest alloc] init];
[request setFinishBlock:^(QCloudListAllMyBucketsResult* result, NSError* error) {
//从result中获取返回信息
}];
[[QCloudCOSXMLService defaultCOSXML] GetService:request];e("CosServerException: " + serverEx.GetInfo());
}
```

### 创建存储桶

#### 功能说明

创建一个存储桶（Put Bucket）。

#### 方法原型

在开始使用 COS 时，需要在指定的账号下先创建一个 Bucket 以便于对象的使用和管理，并指定 Bucket 所属的地域。创建 Bucket 的用户默认成为 Bucket 的持有者。若创建 Bucket 时没有指定访问权限，则默认为私有读写（private）权限。具体步骤如下：    

1. 实例化 QCloudPutBucketRequest，填入需要的参数。
2. 调用 QCloudCOSXMLService 对象中的 PutBucket 方法发出请求。
3. 从回调的 finishBlock 中的 outputObject 获取具体内容。

#### 参数说明

#### QCloudPutBucketRequest 请求参数说明

| 参数名称          | 描述                                                         | 类型       | 必填 |
| ----------------- | ------------------------------------------------------------ | ---------- | ---- |
| bucket            | 要创建的存储桶名称，命名规范请参阅 [存储桶命名规范](https://intl.cloud.tencent.com/document/product/436/13312#.E5.91.BD.E5.90.8D.E8.A7.84.E8.8C.83) | NSString * | 是   |
| accessControlList | 定义 Bucket 的 ACL 属性，有效值：private，public-read-write，public-read；默认值：private | NSString * | 否   |
| grantRead         | 赋予被授权者读的权限，格式：id="OwnerUin"                    | NSString * | 否   |
| grantWrite        | 授予被授权者写的权限，格式：id="OwnerUin"                    | NSString * | 否   |
| grantFullControl  | 授予被授权者读写权限，格式：id="OwnerUin"                    | NSString * | 否   |

#### 请求示例

```objective-c
QCloudPutBucketRequest* request = [QCloudPutBucketRequest new];
request.bucket = @"examplebucket-1250000000"; //additional actions after finishing
[request setFinishBlock:^(id outputObject, NSError* error) {
//可以从 outputObject 中获取服务器返回的 header 信息
}];
[[QCloudCOSXMLService defaultCOSXML] PutBucket:request];
```

#### 返回错误码说明

当 SDK 请求失败的时候，返回的 error 将不为空，并且包括了错误码、错误描述和其它一些调试必备的信息，以帮助开发者快速解决问题。
返回错误码（封装在返回的 error 里）主要包括两类：设备本身因为网络原因等返回的错误码，以及 COS 返回的错误码。

- 对于设备本身因为网络原因产生的错误码，都是负数并且是四位数，例如-1001，这类错误码是苹果定义的，可以参考 Foundation 框架中的 NSURLError.h 头文件内的定义，或者是 [苹果官方文档说明](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes)。
- 对于 COS 返回的错误码，是基于 HTTP 的状态码而来的，也就是404，503这类。对于这类错误码，可以参考 [错误码]( https://intl.cloud.tencent.com/document/product/436/7730) 文档寻求解决方案。
- 对于 SDK 自定义的错误码，均为5位数且都是正数，如10000、20000等。对于这类错误码，可以参考 [SDK 错误码](https://intl.cloud.tencent.com/document/product/436/30610) 文档寻求解决方案。

### 检索存储桶及其权限

#### 功能说明

- Head Bucket 请求可以确认该 Bucket 是否存在，是否有权限访问。
- Head 的权限与 Read 一致。当该 Bucket 存在时，返回200。当该 Bucket 无访问权限时，返回403。当该 Bucket 不存在时，返回404。   

#### QCloudHeadBucketRequest 参数说明

| 参数名称 | 描述                                                         | 类型       | 必填 |
| -------- | ------------------------------------------------------------ | ---------- | ---- |
| bucket   | 存储桶名称，可在 [COS V5 控制台](https://console.cloud.tencent.com/cos5/bucket) 查看，命名格式：BucketName-APPID。例如 examplebucket-1250000000 | NSString * | 是   |

#### 示例

```objective-c
QCloudHeadBucketRequest* request = [QCloudHeadBucketRequest new];
request.bucket = @"examplebucket-1250000000";
[request setFinishBlock:^(id outputObject, NSError* error) {
//可以从 outputObject 中获取服务器返回的header信息
//设置完成回调。如果没有 error，则可以正常访问 bucket。如果有 error，可以从 error code 和 messasge 中获取具体的失败原因。
}];
[[QCloudCOSXMLService defaultCOSXML] HeadBucket:request];
```

#### 返回错误码说明

当 SDK 请求失败的时候，返回的 error 将不为空，并且包括了错误码、错误描述和其它一些调试必备的信息，以帮助开发者快速解决问题。

返回错误码（封装在返回的 error 里）主要包括两类：设备本身因为网络原因等返回的错误码，以及 COS 返回的错误码。

- 对于设备本身因为网络原因产生的错误码，都是负数并且是四位数，例如-1001，这类错误码由苹果公司定于，可以参考 Foundation 框架中的 NSURLError.h 头文件内的定义，或者是 [苹果官方文档说明](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes)。
- 对于COS返回的错误码，是基于 HTTP 的状态码而来的，也就是404，503这类。对于这类错误码，可以参考 [错误码]( https://intl.cloud.tencent.com/document/product/436/7730) 文档寻求解决方案。
- 对于 SDK 自定义的错误码，均为5位数且都是正数，如10000、20000等。对于这类错误码，可以参考 [SDK 错误码](https://intl.cloud.tencent.com/document/product/436/30610) 文档寻求解决方案。

### 获取存储桶的地域信息

#### 功能说明

获取指定存储桶所在的地域信息。

#### 方法原型

进行存储桶操作之前，需要导入头文件 QCloudCOSXML/QCloudCOSXML.h。在此之前您需要完成 [初始化操作](https://intl.cloud.tencent.com/document/product/436/11280#step1)。先生成一个 QCloudGetBucketLocationRequest 实例，然后填入一些需要的额外限制条件，通过并获得内容。具体步骤如下：    

1. 实例化 QCloudGetBucketLocationRequest，填入 Bucket 名。    
2. 调用 QCloudCOSXMLService 对象中的 GetBucketLocation 方法发出请求。    
3. 从回调的 finishBlock 中的获取具体内容。   

#### QCloudGetBucketLocationRequest 请求参数说明

| 参数名称 | 描述                                                         | 类型       | 必填 |
| -------- | ------------------------------------------------------------ | ---------- | ---- |
| bucket   | 存储桶名称，可在 [COS V5 控制台](https://console.cloud.tencent.com/cos5/bucket) 查看，命名格式：BucketName-APPID。例如 examplebucket-1250000000 | NSString * | 是   |

#### 返回结果说明

 QCloudBucketLocationConstraint 参数说明

| 参数名称           | 描述                 | 类型      |
| ------------------ | -------------------- | --------- |
| locationConstraint | 说明 Bucket 所在区域 | NSString* |

#### 示例

```objective-c
QCloudGetBucketLocationRequest* locationReq = [QCloudGetBucketLocationRequest new];
locationReq.bucket = @"examplebucket-1250000000";
 __block QCloudBucketLocationConstraint* location;
[locationReq setFinishBlock:^(QCloudBucketLocationConstraint * _Nonnull result, NSError * _Nonnull error) {
       location = result;
}];
[[QCloudCOSXMLService defaultCOSXML] GetBucketLocation:locationReq];
```

#### 返回错误码说明

当 SDK 请求失败的时候，返回的 error 将不为空，并且包括了错误码、错误描述和其它一些调试必备的信息，以帮助开发者快速解决问题。
返回错误码（封装在返回的 error 里）主要包括两类：设备本身因为网络原因等返回的错误码，以及 COS 返回的错误码。

- 对于设备本身因为网络原因产生的错误码，都是负数并且是四位数，例如-1001，这类错误码是苹果定义的，可以参考 Foundation 框架中的 NSURLError.h 头文件内的定义，或者是 [苹果官方文档说明](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes)。
- 对于COS返回的错误码，是基于 HTTP 的状态码而来的，也就是404，503这类。对于这类错误码，可以参考 [错误码]( https://intl.cloud.tencent.com/document/product/436/7730) 文档寻求解决方案。
- 对于 SDK 自定义的错误码，均为5位数且都是正数，如10000、20000等。对于这类错误码，可以参考 [SDK 错误码](https://intl.cloud.tencent.com/document/product/436/30610) 文档寻求解决方案。

### 删除存储桶

#### 功能说明

删除指定的存储桶。

#### 方法原型

COS iOS SDK 中删除 Bucket 的方法具体步骤如下：

1. 实例化 QCloudDeleteBucketRequest，填入需要的参数。
2. 调用 QCloudCOSXMLService 对象中的 DeleteBucket 方法发出请求。
3. 从回调的 finishBlock 中的 outputObject 获取具体内容。

#### 参数说明

#### QCloudDeleteBucketRequest 请求参数说明

| 参数名称 | 描述                                                         | 类型       | 必填 |
| -------- | ------------------------------------------------------------ | ---------- | ---- |
| bucket   | 要删除的存储桶名称，可在 [COS V5 控制台](https://console.cloud.tencent.com/cos5/bucket) 查看，命名格式：BucketName-APPID。例如 examplebucket-1250000000，注意删除之前要求 bucket 里的内容为空。 | NSString * | 是   |

#### 请求示例

```objective-c
QCloudDeleteBucketRequest* request = [[QCloudDeleteBucketRequest alloc ] init];
request.bucket = @"examplebucket-1250000000";  //存储桶名称，命名格式：BucketName-APPID
[request setFinishBlock:^(id outputObject,NSError*error) {
//additional actions after finishing
//可以从 outputObject 中获取服务器返回的header信息
}];
[[QCloudCOSXMLService defaultCOSXML] DeleteBucket:request];
```

#### 返回错误码说明

当 SDK 请求失败的时候，返回的 error 将不为空，并且包括了错误码、错误描述和其它一些调试必备的信息，以帮助开发者快速解决问题。

返回错误码（封装在返回的 error 里）主要包括两类：设备本身因为网络原因等返回的错误码，以及 COS 返回的错误码。

- 对于设备本身因为网络原因产生的错误码，都是负数并且是四位数，例如-1001，这类错误码是苹果定义的，可以参考 Foundation 框架中的 NSURLError.h 头文件内的定义，或者是 [苹果官方文档说明](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes)。
- 对于COS返回的错误码，是基于 HTTP 的状态码而来的，也就是404，503这类。对于这类错误码，可以参考 [错误码]( https://intl.cloud.tencent.com/document/product/436/7730) 文档寻求解决方案。
- 对于 SDK 自定义的错误码，均为5位数且都是正数，如10000、20000等。对于这类错误码，可以参考 [SDK 错误码](https://intl.cloud.tencent.com/document/product/436/30610) 文档寻求解决方案。

## 访问控制列表

### 设置存储桶的 ACL

#### 方法原型

进行存储桶操作之前，需要导入头文件 QCloudCOSXML/QCloudCOSXML.h。在此之前您需要完成 [初始化操作](https://intl.cloud.tencent.com/document/product/436/11280#step1)。先生成一个 QCloudPutBucketACLRequest 实例，然后填入一些需要的额外限制条件，通过并获得内容。具体步骤如下：    

1. 实例化 QCloudPutBucketACLRequest，填入需要设置的存储桶，然后根据设置值的权限类型分别填入不同的参数。    
2. 调用 QCloudCOSXMLService 对象中的 PutBucketACL 方法发出请求。    
3. 从回调的 finishBlock 中的获取设置是否成功，并做设置成功后的一些额外动作。   

#### QCloudPutBucketACLRequest 参数说明

| 参数名称          | 描述                                                         | 类型       | 必填 |
| ----------------- | ------------------------------------------------------------ | ---------- | ---- |
| bucket            | 存储桶名称，可在 [COS V5 控制台](https://console.cloud.tencent.com/cos5/bucket) 查看，命名格式：BucketName-APPID。例如 examplebucket-1250000000 | NSString * | 是   |
| accessControlList | 定义存储桶的 ACL 属性。有效值：private，public-read-write，public-read；默认值：private | NSString * | 否   |
| grantRead         | 赋予被授权者读的权限，格式：id="OwnerUin"                    | NSString * | 否   |
| grantWrite        | 授予被授权者写的权限，格式：id="OwnerUin"                    | NSString * | 否   |
| grantFullControl  | 授予被授权者读写权限，格式：id="OwnerUin"                    | NSString * | 否   |

#### 示例

```objective-c
QCloudPutBucketACLRequest* putACL = [QCloudPutBucketACLRequest new];
NSString* appID = @“1250000000”;//您的AppID
NSString *ownerIdentifier = [NSString stringWithFormat:@"qcs::cam::uin/%@:uin/%@", appID, appID];
NSString *grantString = [NSString stringWithFormat:@"id=\"%@\"",ownerIdentifier];
putACL.grantFullControl = grantString;
putACL.bucket = @“examplebucket-1250000000”;
[putACL setFinishBlock:^(id outputObject, NSError *error) {
//error occucs if error != nil
//可以从 outputObject 中获取服务器返回的header信息
}];
[[QCloudCOSXMLService defaultCOSXML] PutBucketACL:putACL];

```

#### 返回错误码说明

当 SDK 请求失败的时候，返回的 error 将不为空，并且包括了错误码、错误描述和其它一些调试必备的信息，以帮助开发者快速解决问题。

返回错误码（封装在返回的 error 里）主要包括两类：设备本身因为网络原因等返回的错误码，以及 COS 返回的错误码。

- 对于设备本身因为网络原因产生的错误码，都是负数并且是四位数，例如-1001，这类错误码是苹果定义的，可以参考 Foundation 框架中的 NSURLError.h 头文件内的定义，或者是 [苹果官方文档说明](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes)。
- 对于 COS 返回的错误码，是基于 HTTP 的状态码而来的，也就是404，503这类。对于这类错误码，可以参考 [错误码]( https://intl.cloud.tencent.com/document/product/436/7730) 文档寻求解决方案。
- 对于 SDK 自定义的错误码，均为5位数且都是正数，如10000、20000等。对于这类错误码，可以参考 [SDK 错误码](https://intl.cloud.tencent.com/document/product/436/30610) 文档寻求解决方案。

### 获取存储桶 ACL

#### 方法原型

进行存储桶操作之前，需要导入头文件 QCloudCOSXML/QCloudCOSXML.h。在此之前您需要完成 [初始化操作](https://intl.cloud.tencent.com/document/product/436/11280#step1)。先生成一个 QCloudGetBucketACLRequest 实例，然后填入一些需要的额外限制条件，通过并获得内容。具体步骤如下：    

1. 实例化 QCloudGetBucketACLRequest，填入获取 ACL 的存储桶。    
2. 调用 QCloudCOSXMLService 对象中的 GetBucketACL 方法发出请求。    
3. 从回调的 finishBlock 中的 QCloudACLPolicy 获取具体内容。    

#### QCloudGetBucketACLRequest 参数说明

| 参数名称 | 描述                                                         | 类型       | 必填 |
| -------- | ------------------------------------------------------------ | ---------- | ---- |
| bucket   | 存储桶名称，可在 [COS V5 控制台](https://console.cloud.tencent.com/cos5/bucket) 查看，命名格式：BucketName-APPID。例如 examplebucket-1250000000 | NSString * | 是   |

#### 返回结果说明

QCloudACLPolicy 参数说明

| 参数名称    | 描述                                                         | 类型       |
| ----------- | ------------------------------------------------------------ | ---------- |
| displayName | 存储桶持有者的信息                                           | NSString * |
| identifier  | Bucket 持有者 ID，格式：`qcs::cam::uin/<OwnerUin>:uin/<SubUin>` 如果是根帐号，`<OwnerUin>`和`<SubUin>`是同一个值 | NSString * |

QCloudACLOwner 参数说明

| 参数名称          | 描述                 | 类型                      |
| ----------------- | -------------------- | ------------------------- |
| owner             | 存储桶持有者的信息   | QCloudACLOwner *          |
| accessControlList | 被授权者与权限的信息 | QCloudAccessControlList * |

QCloudAccessControlList 参数说明

| 参数名称  | 描述                   | 类型                             |
| --------- | ---------------------- | -------------------------------- |
| ACLGrants | 存放被授权者信息的数组 | NSArray&lt;QCloudACLGrant`*` > * |

#### 示例

```objective-c
QCloudGetBucketACLRequest* getBucketACl   = [QCloudGetBucketACLRequest new];
getBucketACl.bucket = @"testbucket-123456789";
[getBucketACl setFinishBlock:^(QCloudACLPolicy * _Nonnull result, NSError * _Nonnull error) {
   //QCloudACLPolicy中包含了 Bucket 的 ACL 信息。
}];

[[QCloudCOSXMLService defaultCOSXML] GetBucketACL:getBucketACl];
```

#### 返回错误码说明

当 SDK 请求失败的时候，返回的 error 将不为空，并且包括了错误码、错误描述和其它一些调试必备的信息，以帮助开发者快速解决问题。

返回错误码（封装在返回的 error 里）主要包括两类：设备本身因为网络原因等返回的错误码，以及 COS 返回的错误码。

- 对于设备本身因为网络原因产生的错误码，都是负数并且是四位数，例如-1001，这类错误码是苹果定义的，可以参考 Foundation 框架中的 NSURLError.h 头文件内的定义，或者是 [苹果官方文档说明](https://developer.apple.com/documentation/foundation/1508628-url_loading_system_error_codes)。
- 对于 COS 返回的错误码，是基于 HTTP 的状态码而来的，也就是404, 503这类。对于这类错误码，可以参考 [错误码]( https://intl.cloud.tencent.com/document/product/436/7730) 文档寻求解决方案。
- 对于 SDK 自定义的错误码，均为5位数且都是正数，如10000、20000等。对于这类错误码，可以参考 [SDK 错误码](https://intl.cloud.tencent.com/document/product/436/30610) 文档寻求解决方案。
