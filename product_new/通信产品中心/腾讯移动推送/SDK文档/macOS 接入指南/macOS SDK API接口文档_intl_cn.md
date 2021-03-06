# SDK API说明

## 启动信鸽推送服务

**说明**

* 通过使用在信鸽官网注册的应用的信息，启动信鸽推送服务

**接口**

```objective-c
- (void)startXGWithAppID:(uint32_t)appID appKey:(nonnull NSString *)appKey delegate:(nullable id<XGPushDelegate>)delegate ;
```

**参数说明**

* appID：通过前台申请的应用 ID, 即 Access ID
* appKey： 通过前台申请的 appKey，即 Access Key
* delegate：回调对象 

_**注意：接口所需参数必须要正确填写，反之信鸽服务将不能正确为应用推送消息**_

**示例**

```Objective-C
[[XGPush defaultManager] startXGWithAppID: <#your access ID#>appKey:<#your access key#> delegate:<#your delegate#>];
```

## 终止信鸽推送服务

**说明**

* 终止信鸽推送服务以后，将无法通过信鸽推送服务向设备推送消息，如果再次需要接收信鸽服务的消息推送，则必须需要再次调用 `startXGWithAppID:appKey:delegate:` 方法重启信鸽推送服务

**接口**

```objective-c
- (void)stopXGNotification;
```

**示例**

```Objective-C
[[XGPush defaultManager] stopXGNotification];
```

## 自定义通知栏消息行为

### 创建消息支持的行为

**说明**

在通知消息中创建一个可以点击的事件行为

**接口**

```objective-c
+ (nullable id)actionWithIdentifier:(nonnull NSString *)identifier title:(nonnull NSString *)title options:(XGNotificationActionOptions)options;
```

**参数说明**

* identifier：行为唯一标识 
* title：行为名称 
* options：行为支持的选项

**示例**

```objective-c
XGNotificationAction *action1 = [XGNotificationAction actionWithIdentifier:@"xgaction001" title:@"xgAction1" options:XGNotificationActionOptionNone];
```

_**注意：通知栏带有点击事件的特性，只有在 macOS10.14 + 以上支持，earlier的版本，此方法返回空**_

### 创建分类对象

**说明**

创建分类对象，用以管理通知栏的Action对象

**接口**

```objective-c
+ (nullable id)categoryWithIdentifier:(nonnull NSString *)identifier actions:(nullable NSArray<XGNotificationAction *> *)actions intentIdentifiers:(nullable NSArray<NSString *> *)intentIdentifiers options:(XGNotificationCategoryOptions)options;
```

**参数说明**

* identifier：分类对象的标识
* actions：当前分类拥有的行为对象组
* intentIdentifiers：用以表明可以通过Siri识别的标识
* options：分类的特性

_**注意：通知栏带有点击事件的特性，只有在macOS10.14+以上支持，earlier的版本，此方法返回空**_

**示例**

```Objective-C
XGNotificationCategory *category = [XGNotificationCategory categoryWithIdentifier:@"xgCategory" actions:@[action1, action2] intentIdentifiers:@[] options:XGNotificationCategoryOptionNone];
```

### 创建配置类

管理推送消息通知栏的样式和特性

**接口**

```objective-c
+ (nullable instancetype)configureNotificationWithCategories:(nullable NSSet<XGNotificationCategory *> *)categories types:(XGUserNotificationTypes)types;
```

**参数说明**

* categories：通知栏中支持的分类集合 
* types：注册通知的样式

**示例**

```objective-c
XGNotificationConfigure *configure = [XGNotificationConfigure configureNotificationWithCategories:[NSSet setWithObject:category] types:XGUserNotificationTypeAlert|XGUserNotificationTypeBadge|XGUserNotificationTypeSound];
```


## 角标自动加1

**说明**

* 调用此接口上报当前 App 角标数到信鸽服务器,客户端配置完成即可使用「macOS角标自动加1」的功能，此功能在管理台位置（创建推送→通知栏消息→常用设置→角标数字）

**接口**

```objective-c
- (void)setBadge:(NSInteger)badgeNumber;
```

**参数说明**

* badgeNumber 应用的角标数

**注意：  
1.此接口必须本地调用，否则管理台使用「macOS角标自动加1」功能时，角标会默认不变  
**

**示例**

```Objective-C
[[XGPush defaultManager] setBadge:7];
```

## 管理应用角标

**说明**

* 管理 App 显示的角标数量

**接口**

```objective-c
@property (nonatomic) NSInteger xgApplicationBadgeNumber;
```

**示例**

```objective-c
// 设置应用角标
[[XGPush defaultManager] setXgApplicationBadgeNumber:0];

// 获取应用角标
NSInteger number = [[XGPush defaultManager] xgApplicationBadgeNumber];
```

## 统计推送效果

**说明**

* 为了更好的了解每一条推送消息的运营效果，需要将用户对消息的行为上报

需要调用上报数据的接口  
**API接口**

```Objective-C
- (void)reportXGNotificationInfo:(nonnull NSDictionary *)info;
```

**示例**1

```Objective-C
- (BOOL)application:(NSApplication *)application
  didFinishLaunchingWithOptions:(NSDictionary *)launchOptions 
  {
      [[XGPush defaultManager] reportXGNotificationInfo:launchOptions];
      return YES;
  }
```

**回调接口**

```Objective-C
- (void)xgPushUserNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler;
```

**示例**2

```Objective-C
- (void)xgPushUserNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    [[XGPush defaultManager] reportXGNotificationInfo:notification.request.content.userInfo];
    completionHandler(UNNotificationPresentationOptionBadge | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert);
}
```

## 管理设备 Token

### 查询设备 Token

**说明**

* 查询当前应用从 APNs 获取的 Token 字符串

**接口**

```objective-c
@property (copy, nonatomic, nullable, readonly) NSString *deviceTokenString;
```

**示例**

```objective-c
NSString *token = [[XGPushTokenManager defaultTokenManager] deviceTokenString];
```

### 查询 APNs 注册结果

**说明**

* 如果注册成功，则应用会调用 `NSApplicationDelegate` 代理对象的回调方法\(如下\)，

**接口**

```Objective-C
- (void)application:(NSApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken;
```

### 查询信鸽注册结果

**说明**

* SDK 的启动方法自动注册设备从 APNs 获取的 Token 到信鸽服务器，注册结果会在 `XGPushDelegate` \(以下\)的回调方法返回

**接口**

```objective-c
- (void)xgPushDidRegisteredDeviceToken:(NSString *)deviceToken error:(NSError *)error;
```

_**注意：此回调方法在注册成功之后调用，当前的 Token 已经注册过之后，SDK 将缓存注册信息，此方法将不会再调用**_

### 绑定/解绑 标签和账号

**说明**

* 开发者可以针对不同的用户绑定标签,然后对该标签推送.对标签推送会让该标签下的所有设备都收到推送.一个设备可以绑定多个标签.

** 单操作接口 **
```Objective-C
- (void)bindWithIdentifier:(nullable NSString *)identifier type:(XGPushTokenBindType)type;
- (void)unbindWithIdentifer:(nullable NSString *)identifier type:(XGPushTokenBindType)type;
```

**参数说明**

* identifier:标签或账号
* type:绑定类型

**示例**

```Objective-C
//绑定标签：
[[XGPushTokenManager defaultTokenManager] bindWithIdentifier:@"your tag" type:XGPushTokenBindTypeTag];

//解绑标签
[[XGPushTokenManager defaultTokenManager] unbindWithIdentifer:@"your tag" type:XGPushTokenBindTypeTag];

//绑定账号：
[[XGPushTokenManager defaultTokenManager] bindWithIdentifier:@"your account" type:XGPushTokenBindTypeAccount];

//解绑账号：
[[XGPushTokenManager defaultTokenManager] unbindWithIdentifer:@"your account" type:XGPushTokenBindTypeAccount];
```

** 批量操作接口 **

```Objective-C
- (void)bindWithIdentifiers:(nonnull NSArray <NSString *> *)identifiers type:(XGPushTokenBindType)type
- (void)unbindWithIdentifers:(nonnull NSArray <NSString *> *)identifiers type:(XGPushTokenBindType)type;
```
** 参数说明 **

* identifiers:标签或账号列表
* type:绑定类型

** 注意 **
* 暂不支持账号类型，标签字符串不允许有空格或者是tab字符

### 批量更新标签/账号

** 接口 **
```Objective-C
- (void)updateBindedIdentifiers:(nonnull NSArray <NSString *> *)identifiers bindType:(XGPushTokenBindType)type;
```
** 参数说明 **

* identifiers:标签标识字符串数组，标签字符串不允许有空格或者是tab字符
* type:标识类型

** 注意 **
* 若指定为标签类型，此接口会将当前 Token 对应的旧有的标签全部替换为当前的标签；若指定账号类型，此接口仅取 identifiers 列表中第一个

### 清除全部标签/账号

** 接口 **
```Objective-C
- (void)clearAllIdentifiers:(XGPushTokenBindType)type;
```
** 参数说明 **
* type:标识类型


### 查询绑定的标签和账号

**说明**

* 根据指定类型查询当前 Token 对象绑定的标识 

**接口**

```objective-c
- (nullable NSArray<NSString *> *)identifiersWithType:(XGPushTokenBindType)type;
```

**示例**

```objective-c
// 查询标签
[[XGPushTokenManager defaultTokenManager] identifiersWithType:XGPushTokenBindTypeTag];
// 查询账号
[[XGPushTokenManager defaultTokenManager] identifiersWithType:XGPushTokenBindTypeAccount];
```

## 查询设备通知权限

**说明**

* 查询设备通知权限是否被用户允许 

**接口**

```objective-c
- (void)deviceNotificationIsAllowed:(nonnull void (^)(BOOL isAllowed))handler;
```

**参数说明**

* handler：查询结果的返回方法

**示例**

```objective-c
[[XGPush defaultManager] deviceNotificationIsAllowed:^(BOOL isAllowed) {
        <#code#>
    }];
```

## 查询 SDK 版本

**说明**

* 查询当前 SDK 的版本

**接口**

```objective-c
- (nonnull NSString *)sdkVersion;
```

**示例**

```objective-c
[[XGPush defaultManager] sdkVersion];
```

## 本地推送

本地推送相关功能请参考[苹果开发者文档](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SchedulingandHandlingLocalNotifications.html#//apple_ref/doc/uid/TP40008194-CH5-SW1).

