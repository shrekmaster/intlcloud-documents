## 接口描述

本接口（DescribeServiceReleaseVersion）查询一个服务下面所有已经发布的版本列表。
用户在发布服务时，常有多个版本发布，可使用本接口查询已发布的版本。

## 输入参数

以下请求参数列表仅列出了接口请求参数，其它参数可参考 [公共请求参数](https://intl.cloud.tencent.com/document/product/628/18814)。

| 参数名称      | 是否必选 | 类型     | 描述          |
| --------- | ---- | ------ | ----------- |
| serviceId | 是    | String | 待查询的服务唯一 ID |
| limit     | 否    | Int    | 返回数量       |
| offset    | 否    | Int    | 偏移量        |


## 输出参数
| 参数名称    | 类型          | 描述                                                         |
| ----------- | ------------- | ------------------------------------------------------------ |
| code        | Int           | 公共错误码，0表示成功，其他值表示失败。详见错误码页面的 [公共错误码](https://intl.cloud.tencent.com/document/product/628/18822) |
| codeDesc    | String        | 业务侧错误码。成功时返回 Success，错误时返回具体业务错误原因 |
| message     | String        | 模块错误信息描述，与接口相关                                 |
| totalCount  | String        | 发布版本总数                                                 |
| versionList | List of Array | 版本列表                                                     |

versionList 是历史版本列表，它是由 versionAttribute 组成的数组，versionAttribute 构成如下：

| 参数名称         | 类型             | 描述            |
| ------------ | -------------- | ------------- |
| versionName  | String         | 版本号          |
| versionDesc  | String         | 版本描述         |
| environments | List of String | 该版本当前发布的环境列表 |
| createTime   | Time           | 版本创建时间       |

## 示例 
```
https://apigateway.api.qcloud.com/v2/index.php?
&<公共请求参数>
&Action=DescribeServiceReleaseVersion
&serviceId=service-XX
```
返回示例如下：
```
{
	"code": "0",
	"message": "",
	"codeDesc": "Success",
	"totalCount": 2,
	"versionList": [{
			"versionName": "version-20170904xx",
			"versionDesc": "desc",
			"environments": [
				"test",
				"prepub"
			],
			"createTime": "2017-09-27 00:00::00"
		},
		{
			"versionName": "version-20170905xx",
			"versionDesc": "desc",
			"environments": [
				"release"
			],
			"createTime": "2017-09-27 00:00::00"
		}
	]
}
```




