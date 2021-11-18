# 按需连接配置

在原有配置文件基础上添加以下配置（仅作为参考）：

```yaml
# port: ...
# ...

ios-config:
  ondemand-rules:
    cellular: true
    wifi: true
    ssid-match: only
    ssid-list: ["ssid"]
```

配置说明：

* cellular：`true` 或者 `false`。当设置为 `true` 时，会在蜂窝网络时自动连接；`false` 为自动断开。

* wifi：当设置为 `false` 时在 WiFi 网络下自动断开；当设置为 `true` 时，ssid-match 可选值为：

    * only：仅在 ssid-list 中的 WiFi 下连接

    * except：当处于 ssid-list 中的 WiFi 下时断开连接

    * any：任何 WiFi 网络下自动连接

> 注意：从 Choc 主界面更改*按需连接*会在运行时覆盖配置文件中的配置，但变更不会被写入到配置文件  
> 快速点击三下*按需连接*开关可以重新载入配置文件中的按需连接设置