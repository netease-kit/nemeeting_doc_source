# 网易会议 SDK 文档

欢迎使用网易会议 SDK！本仓库包含网易会议多平台 SDK 的完整开发文档，帮助您快速集成音视频会议功能到您的应用中。

## 支持的平台

网易会议 SDK 支持以下 8 个平台，点击进入对应平台的文档：

| 平台 | 说明 | 文档入口 |
|------|------|----------|
| [Android](./Android/) | Android 原生应用 | [查看文档](./Android/README.md) |
| [iOS](./iOS/) | iOS 原生应用 | [查看文档](./iOS/README.md) |
| [Web](./Web/) | 浏览器 Web 应用 | [查看文档](./Web/README.md) |
| [H5](./H5/) | 移动端 H5 应用 | [查看文档](./H5/README.md) |
| [Windows](./Windows/) | Windows 桌面应用 | [查看文档](./Windows/README.md) |
| [macOS](./macOS/) | macOS 桌面应用 | [查看文档](./macOS/README.md) |
| [Linux](./Linux/) | Linux 桌面应用 | [查看文档](./Linux/README.md) |
| [Electron](./Electron/) | 跨平台 Electron 应用 | [查看文档](./Electron/README.md) |

## 快速开始

每个平台的集成流程基本一致，通常包括以下步骤：

### 1. 集成 SDK
- 下载并引入 SDK 到您的项目
- 配置项目依赖和权限

### 2. 初始化
- 初始化 SDK
- 配置服务器地址（私有化部署时需要）

### 3. 登录鉴权
- 获取账号和 Token
- 调用登录接口完成鉴权

### 4. 创建或加入会议
- 创建即时会议或预约会议
- 加入已有会议

## 主要功能

### 核心功能
- **会议管理** - 创建、预约、加入、结束会议
- **音视频通话** - 高质量的实时音视频通信
- **屏幕共享** - 分享屏幕内容（部分平台支持）
- **会议控制** - 主持人控制、成员管理

### 安全功能
- **会中水印** - 防录屏水印保护
- **媒体流加密** - 端到端加密传输
- **入会控制** - 音视频状态控制、锁定会议

### 界面定制
- **主题设置** - 深色/浅色主题切换
- **语言设置** - 多语言支持
- **自定义菜单** - 会议工具栏定制
- **UI 组件定制** - 自定义界面元素

### 高级功能
- **美颜** - 基础美颜效果
- **虚拟背景** - 自定义会议背景
- **会议扩展** - 插件和扩展能力
- **IM 集成** - 复用现有 IM 账号体系
- **私有云部署** - 支持私有化部署

## 文档结构

每个平台的文档包含以下内容：

```
平台目录/
├── README.md                    # 平台文档导航（从这里开始）
├── 集成概述.md                  # SDK 功能和架构概述
├── 快速开始/
│   ├── 集成并初始化 SDK.md
│   ├── 登录鉴权.md
│   └── 快速跑通 Sample Code.md
├── 会议管理/
│   ├── 创建会议.md
│   ├── 预约会议.md
│   ├── 加入会议.md
│   └── 邀请入会.md
├── 会议安全/
│   ├── 会中水印.md
│   ├── 入会音视频状态控制.md
│   └── 媒体流加密.md
├── 界面定制/
│   ├── 设置语言&主题.md
│   ├── 自定义菜单.md
│   └── 其他界面定制.md
├── 常见问题/
│   └── ...
└── 更新日志.md
```

## 如何使用本文档

### 新手入门
1. 选择您的目标平台，进入对应的平台目录
2. 阅读 **集成概述** 了解 SDK 整体架构
3. 按照 **快速开始** 部分完成基础集成
4. 参考 **会议管理** 实现核心业务功能

### 功能开发
根据需求查找对应的功能模块文档：
- 需要定制 UI → 查看 **界面定制** 模块
- 需要安全功能 → 查看 **会议安全** 模块
- 需要高级特性 → 查看具体功能文档（美颜、虚拟背景等）

### 问题排查
- 查看 **常见问题** 目录
- 参考 **更新日志** 了解版本变更
- 查阅平台相关的 API 文档

## API 文档

完整的 API 参考文档：

- [Android API 文档](https://doc.yunxin.163.com/meetingkit/references/android/dokka/Latest/zh/com/netease/yunxin/kit/meeting/sdk/NEMeetingKit.html)
- [iOS API 文档](https://doc.yunxin.163.com/meetingkit/references/ios/Latest/index.html)
- [Web API 文档](https://doc.yunxin.163.com/meetingkit/references/web/Latest/index.html)
- [H5 API 文档](https://doc.yunxin.163.com/meetingkit/references/h5/Latest/index.html)
- [Windows API 文档](https://doc.yunxin.163.com/meetingkit/references/windows/Latest/index.html)
- [macOS API 文档](https://doc.yunxin.163.com/meetingkit/references/macos/Latest/index.html)
- [Linux API 文档](https://doc.yunxin.163.com/meetingkit/references/linux/Latest/index.html)
- [Electron API 文档](https://doc.yunxin.163.com/meetingkit/references/electron/Latest/index.html)

## 相关资源

- [网易云信官网](https://yunxin.163.com/)
- [控制台](https://app.yunxin.163.com/)
- [技术支持](https://yunxin.163.com/im-sdk-demo)

## 许可证

请参考网易云信服务协议使用本 SDK。

## 联系我们

如有问题或建议，请通过以下方式联系我们：
- 技术支持：访问网易云信官网获取支持
- 问题反馈：通过控制台提交工单
