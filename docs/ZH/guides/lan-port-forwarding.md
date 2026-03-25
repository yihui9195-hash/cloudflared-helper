https://appstore.lazycat.cloud/#/shop/detail/cloud.lazycat.app.forward



## 局域网端口转发访问流程：



![懒猫端口转发工具](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320220713155.png?imageSlim) 



> 设备访问——>入口地址——>通过端口转发后——>访问目标地址
> 始终记住访问的永远是左边的地址
> 注意端口范围：**1~65535**（尽量用 **1024** 以上的端口，避免一些常用协议端口，避免转发一些使用过的端口，创建之前一定先检测连通性）



## 配置选项说明以及使用方法

### 1、微服网卡 (仅微服所在局域网可访问)

限制在微服所在的局域网内访问，外部网络无法直接连接

适用于局域网内部的服务调用和数据传输

**适用使用举例**：

通过微服网卡IP——>访问——>微服应用服务 

![微服应用服务转发](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320223405159.png?imageSlim) 

**通过微服网卡IP——>访问——>微服登录的客户端主机上的服务（目标地址的客户端要登录懒猫微服应用）**

这个可以用远程桌面，访问登录客户端的设备（示例演示的是一个手机上的 `python -m http.server` 的例子）

![微服客户端服务转发](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320231744041.png?imageSlim) 

![手机上的数据](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320232016058.png?imageSlim) 

通过微服网卡IP——>访问——>其他网络地址：微服局域网设备或者微服127.0.0.1的服务（本身127.0.0.1的服务是指：**mainframe** 文件中有 `network_mode：host` 的，或者 **dockge** 部署的容器）

这个可以用远程桌面，微服局域网设备或者微服 `127.0.0.1` 的服务（示例演示 **dockge** 起的服务）

![微服上Dockge启动 Nginx](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320234040856.png?imageSlim) 

![微服上Dockge启动的Nginx服务](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320233118895.png?imageSlim) 

### 2、微服虚拟网卡 (仅微服应用容器可访问)

适用于微服应用容器内部的通信，外部无法直接访问

提供隔离的网络环境，确保容器间通信的安全性

**懒猫微服平台中的应用默认享有网络隔离，类似Docker容器间隔离。若需两个应用互访，此选项来实现转发配置。**

**适用使用举例：**

应用访问——> host.lzcapp:端口 ——>另一个应用服务

[以Radarr和Jackett为例](https://playground.lazycat.cloud/#/guideline/295)

![应用间转发](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320234532846.png?imageSlim) 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/20250605165748091.png?imageSlim" alt="image-20250409140008473" style="zoom: 50%;" /> 

### 3、微服域名 (仅登录微服务户端可访问)

通过域名访问，仅限于已登录微服务客户端的用户

提供基于身份验证的访问控制，增强安全性

**适用使用举例**：

登录客户端——>访问：$微服名.heiyu.space:端口——>微服登录的客户端主机上的服务（目标地址的客户端要登录懒猫微服应用）

这个可以用远程桌面，访问登录客户端的设备（示例演示的是一个python的例子）

![微服域名转发](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320235337454.png?imageSlim) 

![域名转发手机上Python服务](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320235404512.png?imageSlim)  

登录客户端——>访问：$微服名.heiyu.space:端口——>其他网络地址：微服局域网设备或者微服127.0.0.1的服务（本身127.0.0.1的服务是指：mainframe中有network_mode：host的，或者dockge部署的容器）

这个可以用远程桌面，微服局域网设备或者微服 `127.0.0.1` 的服务（示例演示微服局域网的 **Ubuntu** 的 **ssh** 远程）

![域名转发内网服务](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260320235850971.png?imageSlim)  

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20251118133025952.png?imageSlim" alt="image-20251118133025952" style="zoom:50%;" />  

### 4、微服务户端 (仅映射的客户端本地可访问)

服务仅映射到客户端本地，其他设备或用户无法访问

适用于需要本地化服务的场景，保护数据隐私

这个就相当与把微服应用服务、其他客户端的服务端口、其他地址服务端口，转到某一个设备上面，在那个设备上面使用127.0.0.1即可访问

**适用使用举例：**

登录客户端——>本地客户端访问 `127.0.0.1:端口` ——>微服应用服务、其他客户端的服务端口、其他地址服务端口

![仅微服客户端本地可访问](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260321000216508.png?imageSlim) 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20251118135613205.png?imageSlim" alt="image-20251118135613205" style="zoom: 50%;" /> 

### 5、微服通配地址 (0.0.0.0)

绑定到所有网络接口，允许从任意地址访问

通常用于开放服务的场景，保护数据隐私

这一个配置可以代替以上除了   微服务户端 (仅映射的客户端本地可访问)  的所有配置

## 目标转发地址

#### 1、微服应用

**操作过程**：

选择应用（映射懒猫已运行的应用服务）

选择服务(mainfest.yml里的service名称)

选择服务协议（默认tcp/udp）

可下拉选择映射的端口

**主要用途**：

应用的服务暴露与访问

**注意事项**：

应用需要running状态，如果启动了应用，但是列表里面没有running，可以点一下刷新

如果不知道转发应用那个端口，可以点击端口旁边的小问号，查看具体服务对应的具体端口

![只有运行中的服务才可以被转发](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260321000506404.png?imageSlim) 

![快速列出应用内监听的端口](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260321000622184.png?imageSlim)

#### 2、微服客户端

**操作过程：**

选择客户端网络地址（主机名称）

选择服务协议（默认tcp/udp）

选择需要映射的端口

（可选）添加备注

**主要用途：**

客户端调试与访问

这个可以用作远程访问，前提是对端设备也登陆了懒猫微服客户端

![展示微服客户端](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260321000729939.png?imageSlim) 

#### 3、其他网络地址

操作过程：

输入目标网络地址

选择服务协议（默认tcp/udp）

输入需要映射的端口

（可选）添加备注

**主要用途：通用网络转发（如：dockge写127.0.0.1+端口compose里的端口、微服能通信的网络：写IP地址+服务端口）**

一定要测试连通性

可用用作远程访问微服同局域网的设备上的服务

dockge的写法

![其他网络地址](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260321000944418.png?imageSlim) 

微服同局域网的写法

![微服同局域网地址](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260321001056262.png?imageSlim) 



## 添加转发LPK

> 0.9.2 新增功能

我们在别的设备部署了服务，想要通过微服的域名访问，可以使用这个功能。

![image-20260324151056614](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260324151056614.png?imageSlim)

点击**转发LPK**会弹出**创建转发LPK界面**

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325144411014.png?imageSlim" alt="转发本地py程序" style="zoom:50%;" />



- 子域名：打开应用的子域名，列如 {填写的子域名}.{你的盒子名}.heiyu.space
- 名称：在控制台的名称
- 图标：在控制台显示的图标
- 上游地址：要转发的服务地址
  - 协议可选 http 和 https，选择与转发服务相同的协议
  - ip 要转发服务的地址
  - 端口 转发服务的端口

- **跳过认证，可以在不登录微服的情况下使用（API类型的服务，推荐勾选这个选项，在发送请求时不需要在添加微服的认证请求头）**
- 图标，在启动台展示的应用图片
- Package ID，自动生成不能修改



<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325144838888.png?imageSlim" alt="image-20260325144838888" style="zoom:50%;" />

点击创建后会自动安装一个应用到启动台里面

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325145059804.png?imageSlim" alt="image-20260325145059804" style="zoom:33%;" />

打开后可以看到本地启动的服务，可以通过

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325145202696.png?imageSlim" alt="image-20260325145202696" style="zoom:33%;" />

## NetMap LPK

> 0.9.2 新增功能

其他设备部署的服务如果不是 web 相关的，就可以使用这个功能，把目标主机的所有服务全部都转发过来，通过域名来访问。

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325153044263.png?imageSlim" alt="image-20260325153044263" style="zoom:50%;" />

我这里把我电脑的IP 通过微服转发，点击创建并安装

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325153208779.png?imageSlim" alt="image-20260325153208779" style="zoom:50%;" />

安装之后在微服控制台中会出现一个新的图标，打开之后如下图：

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325153326042.png?imageSlim" alt="image-20260325153326042" style="zoom:50%;" />

我们访问这个 `archlinux.mhwy.heiyu.space` 域名的所有端口，都会被转发到 `192.168.3.225` 这台电脑上面。

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1128/image-20260325153616979.png?imageSlim" alt="image-20260325153616979" style="zoom:50%;" />

我本机开启了 SSH 服务，这里通过域名访问我本机的 ssh 这里可以看到已经访问成功了

> 注意，访问时需要确保端口转发工具创建的 LPK 是在运行的状态，不然是无法访问的。

## 结语

端口转发玩法性很多，入口地址与目标地址可以有很多种搭配方式，规则保存之前一定要测试连通性
