
VPN 连接是一种通过公网加密通道连接您对端 IDC 和私有网络（Virtual Private Cloud，VPC）的方式。如下图所示，腾讯云 VPN 连接分为如下组成部分：
- VPN 网关：创建的私有网络 IPsec VPN 网关。
- 对端网关： 记录 IDC 端 IPsec VPN 网关公网 IP 地址的逻辑对象（IDC 端必须有固定公网 IP）。
- VPN 通道：加密的 IPsec VPN 通道。

![](https://main.qcloudimg.com/raw/1b5ea6f4bbda1d71400ccd5d4618849b.png)
VPC 内可以建立 VPN 网关，每个 VPN 网关可以建立多个 VPN 通道，每个 VPN 通道可以打通一个本地 IDC。
>建立 VPN 连接后，您需要在路由表中配置相关路由策略，才能真正实现通信。


