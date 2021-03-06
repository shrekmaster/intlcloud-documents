腾讯云负载均衡支持 TCP/UDP/TCP SSL/HTTP/HTTPS 协议，提供基于域名和 URL 路径的灵活转发能力。本文将引导您如何快速使用负载均衡。

## 前提条件
1. 负载均衡只负责转发流量，不具备处理请求的能力。因此，您需要有处理用户请求的云服务器实例。
在本示例中，只要具有两台云服务器实例即可，您也可以自行规划云服务器数量。本例中已经在广州地域下创建了云服务器实例 `rs-1` 和 `rs-2`。有关如何创建云服务器实例，请参考 [购买并启动云服务器实例](https://intl.cloud.tencent.com/document/product/213/4855)。
2. 本文以 HTTP 转发为例，云服务器上必须部署相应的 Web 服务器，如 Apache、Nginx、IIS 等。
为了验证结果，示例在 `rs-1` 上部署了 Apache 并返回一个带有 “Hello Tomcat! This is rs-1!” 的 HTML，在 `rs-2` 上部署了 Apache 并返回一个带有 “Hello Tomcat! This is rs-2!” 的 HTML。更多云服务器部署内容，请参考 [Linux（CentOS）下部署 Java Web](https://intl.cloud.tencent.com/document/product/214/32391) 及 [Windows 下安装配置 PHP](https://intl.cloud.tencent.com/document/product/213/10182)。
3. 访问云服务器的公网 IP+路径，若显示结果为您部署好的页面，则表示服务部署成功。
>
> - 云服务器上必须购买公网带宽，因为当前的带宽属性在 CVM 上，而非 CLB 上。
> - 示例中后端服务器部署的服务返回值不同，实际情况下，为保持所有用户均有一致体验，后端服务器上一般是部署完全相同的服务。

## 购买负载均衡实例
1. 登录腾讯云 [负载均衡服务购买页](https://buy.cloud.tencent.com/lb)。
2. 本例地域选择与云服务器相同的【广州】，实例类型选择【负载均衡】，网络属性选择【公网】，网络选择【Default-VPC（默认）】，实例名称填写【clb-test】。
3. 单击【立即购买】，完成付款。
有关负载均衡实例的更多内容，请参考 [产品属性选择](https://intl.cloud.tencent.com/document/product/214/13629) 。
![](https://main.qcloudimg.com/raw/f6d73eed167081e5c051cd2eaba22750.png)
4. 在“CLB 实例列表”页，选择对应的地域即可看到新建的实例。
![](https://main.qcloudimg.com/raw/8b03655a8152b7767d81e7f947c5c97f.png)

## 创建负载均衡监听器
负载均衡监听器通过指定协议及端口来负责实际转发。本文以负载均衡转发客户端的 HTTP 请求配置为例。
### 配置 HTTP 监听协议和端口
1. 登录 [负载均衡控制台](https://console.cloud.tencent.com/clb/index?rid=1&type=2%2C3)。
2. 在实例管理页面中，找到已创建的负载均衡实例 `clb-test`，单击实例 ID，进入负载均衡详情页。
3. 在“基本信息”模块，可以单击名称后的修改图标修改实例名称。
4. 在“监听器管理”中的【HTTP/HTTPS 监听器】下，单击【新建】，新建负载均衡监听器。
![](https://main.qcloudimg.com/raw/432c3ca99d1c68ea443d1f02d6fc5201.png)
5. 在弹出框中，配置以下内容：
  - 名称自定义为 “Listener1”。
  - 监听协议端口为 `HTTP：80`。
6. 单击【提交】，创建负载均衡监听器。

### 配置监听器的转发规则
1. 在“监听器管理”中，选中刚才新建的监听器 Listener1，单击【＋】，开始添加规则。
![](https://main.qcloudimg.com/raw/ad3ad578fa4de43d271420bb62bb2efa.png)
2. 在弹出框中，配置域名、URL 路径和均衡方式。
  - 域名：您的后端服务所使用的域名，本例使用 `www.qcloudtest.com`。域名支持通配符，详情请参见 [配置说明](https://intl.cloud.tencent.com/document/product/214/9032)。
  - 默认域名：当客户端请求没有匹配本监听器的任何域名时，CLB 会将请求转发给默认域名（default server），每个监听器只能配置一个默认域名。若该监听器没有配置默认域名时，CLB 会将请求转发给第一个域名。本例不配置。
  - URL 路径：您的后端服务的访问路径，本例使用 `/image/`。
  - 均衡方式选择“加权轮询”。
![](https://main.qcloudimg.com/raw/727abf7e9333e3b7c35c6db044078904.png)
3. 配置健康检查：开启健康检查，检查域名使用默认的转发域名和转发路径。
![](https://main.qcloudimg.com/raw/3d9a4ca9a71c3de15f6a795783add370.png)
4. 会话保持：不勾选会话保持。
5. 单击【完成】，完成监听器转发规则的配置。
![](https://main.qcloudimg.com/raw/44bfa5d1667a3b7a91fd71c6f74c5bc6.png)

有关负载均衡监听器的更多内容，请参考 [负载均衡监听器概述](https://intl.cloud.tencent.com/document/product/214/6151)。
>
>- 一个监听器（即监听协议：端口）可以配置多个域名，一个域名下可以配置多条 URL 路径，选中监听器或域名，单击【＋】，号即可创建新的规则。
>- 会话保持：如果用户关闭会话保持功能，选择轮询的方式进行调度，则请求依次分配到不同后端服务器上；如果用户开启会话保持功能，或关闭会话保持功能但选择 ip_hash 的调度方式，则请求持续分配到同一台后端服务器上去。

### 绑定云服务器
1. 在“监听器管理”页面，选中并展开刚才创建的监听器 Listener1，选中域名、选中 URL 路径，在屏幕右侧即可看到该 URL 路径绑定的云服务器信息，单击【绑定】。
![](https://main.qcloudimg.com/raw/75f058befe3081c947a2e4ba415ee486.png)
2. 在弹出框中，选择绑定实例类型为【云服务器】，再选择与 CLB 同地域下的云服务器实例 `rs-1` 和 `rs-2`，设置云服务器端口均为“80”，云服务器权重均为默认值“10”。
![](https://main.qcloudimg.com/raw/422814be88c68912bfdc80cbaaea5e33.png)
3. 单击【确认】，完成绑定。
4. 展开监听器到 URL 路径维度，可以查看绑定的云服务器和其健康检查状态，当状态为“健康”时表示云服务器可以正常处理负载均衡转发的请求。
![](https://main.qcloudimg.com/raw/dfd2429e93e533799a8aca98dcaa9d84.png)
>一条转发规则（监听协议 + 端口 + 域名 + URL 路径）可以绑定同一台云服务器的多个端口。如用户在 `rs-1` 的 `80` 和 `81` 端口部署了一样的服务，则 CLB 支持示例中的转发规则同时绑定 `rs-1` 的 `80` 和 `81` 端口，两个端口都会接收到 CLB 转发的请求。

## 验证负载均衡服务
配置完成负载均衡后，可以验证该架构是否生效，即验证通过一个 CLB 实例下不同的 **域名+URL** 访问不同的后端云服务器，也即验证**内容路由（content-based routing）** 的功能。
### 方法一：配置 hosts 将域名指向 CLB
1. 在 Windows 系统中，进入 `C:\Windows\System32\drivers\etc` 目录，修改 hosts 文件，把域名映射到 CLB 实例的 VIP 上。
![](https://main.qcloudimg.com/raw/b276f09e903bfb48e7ac3c5f3c415e94.png)
2. 为了验证 hosts 是否配置成功，可以运行 cmd，用 ping 命令探测一下该域名是否成功绑定了 VIP，如有数据包，则证明绑定成功。
![](https://main.qcloudimg.com/raw/54809213ac8468f8f28e8b725a59a29b.png)
3. 在浏览器中输入访问路径 `http://www.qcloudtest.com/image/`，测试负载均衡服务。如下图所示，则表示本次请求被 CLB 转发到了 rs-1 这台 CVM 上，CVM 正常处理请求并返回。
![](https://main.qcloudimg.com/raw/9ee8a5b348e81aa59d7eb366663be11a.png)
4. 此监听器的轮询算法是“按权重轮询”，且两台 CVM 的权重都是“10”，刷新浏览器，再次发送请求，可以看到本次请求被 CLB 转发到了 rs-2 这台 CVM 上。
![](https://main.qcloudimg.com/raw/b22a11e8e4e9d4ee5ce22f9fe6739690.png)
>`image/` 后 `/` 必须保留，代表 image 是默认的目录，而不是名为 image 的文件。


## 配置重定向功能（可选）
重定向配置分为手动重定向和自动重定向：
- 自动重定向（强制 HTTPS）：PC、手机浏览器等以 HTTP 请求访问 Web 服务，希望 CLB 代理后，返回 HTTPS 的 respond。默认强制浏览器以 HTTPS 访问网页。
- 手动重定向：当出现 Web 业务需要临时下线（如电商售罄、页面维护，更新升级时）会需要重定向能力。如果不做重定向，用户的收藏和搜索引擎数据库中的旧地址只能让访客得到一个404或503错误信息页面降低了用户体验度，导致访问流量白白流失，且该页面积累的搜索引擎评分也会无效。
详情请参见 [重定向配置](https://intl.cloud.tencent.com/document/product/214/8839)。
