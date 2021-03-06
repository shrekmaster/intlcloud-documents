## 简介

文件解压缩功能是腾讯云对象存储 COS 基于 [云函数 SCF](https://intl.cloud.tencent.com/document/product/583) 为用户提供的数据处理解决方案。添加文件解压缩功能后，当压缩文件上传到 COS 时，将自动触发 COS 为您预配置的云函数，自动将文件解压到指定的存储桶和目录中。


## 相关说明

- 请**不要**在 [云函数 SCF](https://console.cloud.tencent.com/scf/index?rid=1) 中删除文件解压缩函数，否则可能导致您的规则不生效。
- 已上线 SCF 的地域均已支持 ZIP 包解压，分别为广州、上海、北京、成都、香港、新加坡、孟买、多伦多、硅谷。
- 压缩包中的目录或者文件名请严格使用 UTF-8 或 GB 2312 编码，否则可能导致解压后的文件名或者目录名出线乱码、解压过程中断等情况；如果出现报错，您可以前往 [SCF 控制台](https://console.cloud.tencent.com/scf/index?rid=1) 查看运行日志查看错误详情。
- 归档存储类型文件不支持解压缩，如您需要解压缩归档存储类型的压缩包，请恢复后再进行；恢复操作请参见 [恢复归档对象](https://intl.cloud.tencent.com/document/product/436/30961)。
- 解压单个压缩包最大出力时间为900秒，超过900秒未完成的解压任务会失败。解压缩功能依赖于云函数限制，其他限制请参照 [SCF 函数限制](https://intl.cloud.tencent.com/document/product/583/11637)。
- 压缩包中单个文件大小不得大于5G，否则将导致解压缩失败。
- 解压缩功能依赖云函数服务提供，云函数服务为用户提供了 [免费额度](https://intl.cloud.tencent.com/document/product/583/12282)，超出免费额度的部分需要按照 [SCF 产品定价](https://intl.cloud.tencent.com/document/product/583/12281) 收费。在解压缩功能中，如果您的压缩包越大，将消耗更多的资源使用量；如果您解压缩的次数越多，则将消耗更多的调用次数。

## 操作步骤

1. 登录 [对象存储控制台](https://console.cloud.tencent.com/cos5)。
2. 在左侧导航中，单击【存储桶列表】，然后单击需要创建解压缩规则的存储，进入存储桶管理页。
3. 单击左侧导航栏的【函数计算】，进入文件解压页面。
4. 单击【添加文件解压】，在弹出的窗口中配置如下信息：
	- **函数名称**：函数名称作为函数的唯一标识名称，创建后不可修改。
	- **事件类型**：事件是指触发云函数的操作。以上传操作为例，上传的方式可能是调用`PUT Object`接口，也可能是调用`POST Object`接口，当选择事件为【Put方法创建】时，只有通过`PUT Object`接口上传的压缩包会触发解压缩。
	- **触发条件**：触发条件指的是压缩包上传到哪个目录时会触发云函数。如果选择指定前缀，则仅当压缩包上传到指定前缀路径下时会触发云函数；如果选择全部存储桶，则压缩包上传到存储桶任意位置均会触发。
> 如果配置的目标文件前缀与触发条件存在包含关系，可能导致循环触发，请尽量避免这种情况。比如目标前缀为`prefix`，触发条件为`pre`，当上传一个`pref`的压缩包时，将触发循环解压。

- **解压格式**：指当前可支持的压缩格式，目前仅支持 ZIP 格式压缩包解压。
- **目标存储桶**：压缩包解压缩后，文件存储的存储桶。
- **目标文件前缀**：压缩包解压缩后，文件存储的具体目录，如果不设置则默认为存储桶根目录。
- **SCF 授权**：解压缩需要授权云函数从您的存储桶中读取压缩包，并将解压缩后的文件上传到您指定的位置。因此需要添加此授权。
5. 添加配置后，可以单击【查看日志】盘点解压缩的历史运行情况；如果需要删除不需要的文件解压缩规则，可以单击【删除】删除相关配置。
