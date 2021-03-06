## Feature Description 
- The admin recalls a one-to-one chat message.
- The API can only be used to recall one-to-one chat messages sent by the RESTful API either [one by one](https://intl.cloud.tencent.com/document/product/1047/34919) or [in batches](https://intl.cloud.tencent.com/document/product/1047/34920). 
- After the API is invoked to recall the message, the offline storage, roaming storage, and local cache of the message on the client will all be recalled.

> After a one-to-one chat message is recalled through this API, the recalled message cannot be restored. Therefore, exercise caution when invoking this API. 

## API Invocation Description
### Request URL example
```
https://console.tim.qq.com/v4/openim/admin_msgwithdraw?sdkappid=88888888&identifier=admin&usersig=xxx&random=99999999&contenttype=json
```
### Request parameters

The following table lists and describes only parameters to be modified when the API is invoked. For details on other parameters, see [RESTful API Overview](https://intl.cloud.tencent.com/document/product/1047/34620). 

| Parameter | Description |
| ------------------ | ------------------------------------ |
| v4/openim/admin_msgwithdraw | Request API |
| sdkappid | SDKAppID assigned by the IM console when an app is created |
| identifier | This must be the app admin account. For details, see [App Admins](https://intl.cloud.tencent.com/document/product/1047/33517#app-.E7.AE.A1.E7.90.86.E5.91.98). |
| usersig | The signature generated by the app admin account. For details, see [Generating UserSig](https://intl.cloud.tencent.com/document/product/1047/34385). |
| random | Enter a random 32-bit unsigned integer ranging from 0 to 4294967295. |

### Maximum invocation frequency

The maximum invocation frequency is 200 times per second.

### Request packet example
```
{
	"From_Account": "vinson",
	"To_Account": "dramon",
	"MsgKey": "31906_833502_1572869830"
}
```


### Request packet fields

| Field | Type | Attribute | Description |
|---------|---------|----|---------|
| From_Account | String | Required | UserID of the message sender |
| To_Account | String | Required | UserID of the message receiver |
| MsgKey | String | Required | The unique identifier of the message to be recalled. This field is returned by the RESTful API for [sending a one-to-one chat message](https://intl.cloud.tencent.com/document/product/1047/34919) or that for [sending one-to-one chat messages in batches](https://intl.cloud.tencent.com/document/product/1047/34920). |



### Response packet examples
- Normal response
```
{
    "ActionStatus": "OK", 
    "ErrorInfo": "", 
    "ErrorCode": 0
}
```
- Abnormal response
```
{
    "ActionStatus": "FAIL", 
    "ErrorInfo": "Fail to Parse json data of body, Please check it", 
    "ErrorCode": 90001
}
```

### Response packet fields

| Field | Type | Description |
|---------|---------|---------|
| ActionStatus | String | The request processing result. OK: succeeded. FAIL: failed. |
| ErrorCode | Integer | The error code. 0: succeeded. Others: failed. |
| ErrorInfo | String | The error information. |

## Error Codes

Unless a network error (such as error 502) occurs, the HTTP return code for this API is always 200. ErrorCode and ErrorInfo in the response packet represent the actual error code and error information, respectively.
For common error codes (60000 to 79999), see [Error Codes](https://intl.cloud.tencent.com/document/product/1047/34348). 
The following table describes the error codes specific to this API.

| Error Code | Description |
| ------------- | ------------------------------------------------------------ |
| 90001 | Failed to parse the JSON request packet. In this case, check whether the request packet meets JSON specifications. |
| 90003 | The JSON request packet does not contain To_Account or To_Account does not exist. |
| 90008 | The JSON request packet does not contain From_Account or From_Account does not exist. |
| 90009 | The request requires app admin permissions. |
| 90054 | The MsgKey in the recall request is invalid. |
| 91000 | An internal server error occurred. In this case, try again. |


## References
- Sending a one-to-one chat message ([v4/openim/sendmsg](https://intl.cloud.tencent.com/document/product/1047/34919))
- Sending one-to-one chat messages in batches ([v4/openim/batchsendmsg](https://intl.cloud.tencent.com/document/product/1047/34920))


