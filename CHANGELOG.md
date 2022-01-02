---
description: 此页面将会展示最近版本（仅正式版）的更新内容。
---

# 最近更新

## 1.3.1(95)

### Features
 - 配置文件排序
 - 切换深色模式
 - Haptics 开关
 - Rewrite 支持重写请求 headers Host 字段以及正则替换*

### Changes
 - 优先使用 Group URL 进行延迟测试

> *: 详情请看 https://choc.watfaq.com/advanced-usage/mitm#rewrite

## 1.3.0(91)

### Features
- 主页新增一些信息模块
- 设置页面自定义测速使用的 URL
- 长按订阅信息刷新
- 设置页新增在线订阅转换入口链接
- 新的节日图标
- 实验性功能：MitM 以及 Rewrite

### Changes
- 面板不再使用「策略组」概念，统一改为「分组」
- 重新实现面板管理后端，可在VPN断开时查看和修改代理分组
- 优化文本编辑器体验
- 性能优化
- UI 优化
- 其他优化
### Fixes
- 优化频繁切换主题颜色导致应用崩溃
- 优化 provider 延迟展示

## 1.2.0(61)

### Feature

* 全新界面 UI
* 全新实现管理面板
* 新增 RULE-SET 支持
* 新增 iCloud 自动同步本地配置选项
* 新增配置文件自动更新支持选择更新频率
* 新增自定义主题颜色

### Changes

* 设置页面布局调整

### Fixed

* 修复某些情况下下载配置文件名不正确的问题
* 其他问题修复

## 1.0.1(41)

### Feature

* 独立设置界面
* 当配置文件中未设置 DNS 时可在 UI 中添加 DNS
* 支持 `bypass tunnel routes`
* 支持隐藏 VPN 图标

### Changes

* 界面布局调整
* 其他 UI 调整
* 使用开源文本编辑类库 - 支持显示配置文件行号

