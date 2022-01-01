---
description: 常见问题
---

# FAQ

## 这是什么？

这是首个 clash on iOS 的公开版本。

完全基于原生 clash 开发，并针对 iOS 使用场景进行深度适配 - 并非 **兼容** clash 配置。

您可以用这个软件完成所有 clash 能做的事情，并且同时可以使用 iOS 版本独有的特性。

## clash 是什么？

下一个问题。

## 如何使用？

添加符合 clash 格式的配置文件后点击启动按钮。

## 如何添加配置文件？

参考：https://choc.watfaq.com/basic-usage/remote-config

## 和其他软件相比哪个快？

不一定。

## 能替代其他软件吗？

请看第一个问题。

## 如何实现 Safari 搜索跳转到 Google.com ？

参考配置片段：

```yaml
- regex: ^http://www.google.cn
  match: url
  action: "header"  # rewrite request header Host
  target: http://www.google.com
```

或者：

```yaml
- regex: ^https?://www.google.cn/(.*)
  match: url
  action: "302"
  target: https://www.google.com/$1  # redirect with params
```
