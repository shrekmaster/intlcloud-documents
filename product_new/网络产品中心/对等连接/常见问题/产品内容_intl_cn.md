### 对等连接的互通性传递吗？
对等连接能使 VPC 两两建立互联，但是这种互通关系不发生传递。
例如，如下图所示，VPC 1 与 VPC 2 建立了对等连接，VPC 1 和 VPC 3 也建立了对等连接。然而由于对等连接的不传递性，VPC 2 和 VPC 3 的流量不能互通。
![](//mccdn.qcloud.com/static/img/9127397dcb1df231bfd8d32bcd628223/image.png)
>**注意：**
>即使建立了对等连接，如果两端没有配置发包、回包路由，也无法实现通信。

### 同地域和跨地域对等连接有什么差异？
- 同地域对等连接主要用于打通同地域处于不同私有网络中的应用。
- 跨地域对等连接主要用于实现不同地域的私有网络数据互通。

详情请参见 [产品功能](https://intl.cloud.tencent.com/document/product/553/18829#.E5.90.8C.E5.9C.B0.E5.9F.9F.E5.92.8C.E8.B7.A8.E5.9C.B0.E5.9F.9F.E5.AF.B9.E7.AD.89.E8.BF.9E.E6.8E.A5)。

### 对等连接可以设置带宽限制吗？
- 跨地域对等连接：使用 API 新建时可以设置带宽限制，控制台设置带宽限制功能将在近期推出。
- 同地域对等连接：带宽不收费，无带宽限制。

### 一方删除已建立的对等连接，另一方还能访问删除方的 VPC 吗？
不能，建立对等连接双方中任意一方均可以随时中断对等连接，中断后连接立即失效，只有重新建立连接才能访问对方 VPC。


### 能否将私有网络对等连接到其他腾讯云账户的私有网络？
可以，只要其他私有网络的所有者接受用户的对等连接请求。

### 能否将 IP 地址范围重叠的两个私有网络进行对等连接？
不能，对等连接的私有网络 IP 范围必须不能重叠。

### 流量是加密的吗？
不是。对等连接建立后，两个私有网络之间的互访与同一个私有网络内两台云服务器互访相同，没有做额外加密。私有网络内的网络流量自始至终与其他网络之间是隔离保密的。

### 是否可能出现单点故障？
私有网络对等连接既不是网关，也不是 VPN 连接，不依赖某个独立的实体硬件，不存在单一故障点或带宽瓶颈。


