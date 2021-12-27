# MitM 以及 HTTP(S) Rewrite

{% hint style="info" %}
_此功能目前仍属于实验性功能，如配置后连接出现问题请暂时关闭该功能并通过[邮件](mailto:support@watfaq.com?subject=%5BMitM%20%E9%97%AE%E9%A2%98%E5%8F%8D%E9%A6%88%5D%E8%AF%B7%E6%8F%8F%E8%BF%B0%E4%BD%A0%E7%9A%84%E9%97%AE%E9%A2%98)或 [Telegram 群组](https://t.me/choc_chat)反馈。_
{% endhint %}

{% hint style="info" %}
_当前版本的 MitM 支持对 HTTP(S) 请求的 URL 进行匹配以及 URL Rewrite，后续版本可能会增加 Headers 匹配/改写、运行脚本等更多功能。_
{% endhint %}


## 准备

### 准备 CA 证书

在启用 MitM 之前，需要有一个 CA 证书。

由于当前版本的 app 不支持自动生成，您可以导入已有的 CA 证书，或通过其他工具创建新的 CA 证书。

#### 导入已有 CA 证书

如您已有可用 PKCS 12/PFX 证书（例如您已经正在使用其他支持 MitM 的软件，或部署私有 PKI 系统），可将其进行 base64 编码，并保存编码后的结果备用。

您同时还需要创建该证书时指定的密钥。

{% hint style="info" %}
对于大部分用户来说，您可能已有可用证书（及其密钥）。
{% endhint %}

#### 创建全新 CA

* macOS 参考：[https://support.apple.com/zh-cn/guide/keychain-access/kyca2686/mac](https://support.apple.com/zh-cn/guide/keychain-access/kyca2686/mac)
* Linux/Windows 参考：
    - 使用 cfssl 生成 PEM 格式证书和密钥：[https://rob-blackbourn.medium.com/how-to-use-cfssl-to-create-self-signed-certificates-d55f76ba5781](https://rob-blackbourn.medium.com/how-to-use-cfssl-to-create-self-signed-certificates-d55f76ba5781)
    - 将 PEM 格式转换为 PFX：[https://stackoverflow.com/a/808742/1109167](https://stackoverflow.com/a/808742/1109167)

> 对于大部分用户，您可能已有可用 CA 证书。如您有安全性顾虑，建议创建个人 CA 证书。

将 P12 证书转换为 base64 编码：

```console
$  base64 /path/to/your.p12

...
...
...
```

至此，应该已经准备好了：
1. base64 编码的 p12 格式 CA 证书
2. 该证书对应的密钥

### 信任证书

上一步准备好的 pfx 证书发送到手机（通过邮件/AirDrop等方式并在手机中打开安装该证书。

安装好后，进入系统设置，手动信任该证书。参考：[https://support.apple.com/zh-cn/HT204477#:~:text=If%20you%20want%20to%20turn,Mobile%20Device%20Management%20(MDM)](https://support.apple.com/zh-cn/HT204477#:~:text=If%20you%20want%20to%20turn,Mobile%20Device%20Management%20(MDM))

至此，准备工作结束。

## 配置

### MitM

由于 clash 使用 yaml 格式的配置文件，为保持一致性，MitM 相关功能仍然通过 yaml 配置文件来设置。

相关的配置项：

```yaml
mixed-port: 8080  # 原有 clash 配置

# 新增以下配置项
mitm:
  enable: true  # 是否开启
  hostnames:  # 需要进行 MitM 匹配的 hostname，支持通配符 *
  - '*.google.com'
  - www.qq.com

  ca:
    passphase: "填入上一步准备好的证书密钥"
    certificate: "填入上一步准备好的证书 base64 编码"
```

至此 MitM 配置完毕。

如果 log-level 设置为 debug，匹配 MitM hostname 的请求会出现在日志中。

> 启用 MitM 后可以解密 HTTPS 请求与响应，由于目前版本不支持显示相关内容，所以无法通过 UI 查看。未来的版本中会增加该功能。

### Rewrite

由于仅配置 MitM 目前来说没有任何实际意义，可通过配合 URL Rewrite 规则来实现进阶的规则匹配与处理。

相关的配置项：

```yaml
port: 8080  # 原有 clash 配置

# 新增以下配置
rewrite:
- regex: ^https?:\/\/api\.douban\.com\/v2\/app_ads\/common_ads
  match: url
  action: reject-image
  target: ""
```

参数解释：

每一组 Rewrite 规则由以下参数组成：

* regex：需要重写的请求 URL，支持[正则表达式](https://github.com/google/re2/wiki/Syntax)
* match: `url` 表示匹配请求的 URL。
    - 目前仅支持 `url` 一项，之后会新增
* action：
    - reject：直接关闭连接
    - reject-image：修改该请求的响应，返回 1x1 像素大小的图片文件
    - 自定义 status code：例如，200/404 等
* target：仅当 action 设置为 3xx status code 时，会设置 Location header 为 target 值，实现 URL 重定向

更多规则可**参考**，并更具实际使用场景做出调整：[https://raw.githubusercontent.com/Watfaq/choc-configs/main/samples/rewrite.yaml](https://raw.githubusercontent.com/Watfaq/choc-configs/main/samples/rewrite.yaml)

## 参考链接

* [https://en.wikipedia.org/wiki/Certificate_authority](https://en.wikipedia.org/wiki/Certificate_authority)
* [https://en.wikipedia.org/wiki/Man-in-the-middle_attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
* [https://datatracker.ietf.org/doc/html/rfc7231#section-6](https://datatracker.ietf.org/doc/html/rfc7231#section-6)

---

如有疑问，可通过文章顶部邮件/Telegram 用户群组反馈。

也可通过支持 Web3 的浏览器访问本文部署于 IPFS 上的版本：[http://choc.watfaq.eth/advanced-usage/mitm.html](http://choc.watfaq.eth/advanced-usage/mitm.html)