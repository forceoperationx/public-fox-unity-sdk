[TOP](../../../README.md)　>　[Unity插件的导入步骤](../README.md)　>　**iOS 项目设置**

---

# iOS项目设置

## **Xcode项目设置**

### Xcode项目的发布

为制作iOS项目，按以下步骤发布Xcode项目，在Xcode上进行必要设置。

1. 选择菜单中「File」>「BUild Settings…」
2. Platform中选择「iOS」，点击「Switch Platform」
3. 点击「Player Settings」，Inspector中根据自身环境进行设置
4. 	点击「Build」或「Build And Run」，进行Xcode项目发布

### Xcode项目编辑

打开发布的Xcode项目进行编辑。

* **框架设置**

请将以下框架设置到项目里。

<table>
<tr><th>框架名</th><th>Status</th></tr>
<tr><td>AdSupport.framework</td><td>任意</td></tr>
<tr><td>Security.framework </td><td>必须</td></tr>
<tr><td>SystemConfig.framework </td><td>必须</td></tr>
<tr><td>WebKit.framework</td><td>必须</td></tr>
</table>

* **关于App Transport Security**

从F.O.X SDK ver4.0.0开始，底层全部都基于HTTPS协议进行通信，不需要做额外的对应。

---
[返回](../README.md)

[Top](../../../README.md)
