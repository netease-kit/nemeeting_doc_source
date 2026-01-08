# NEMeeting SDK Documentation

Welcome to the NEMeeting SDK documentation. This repository contains comprehensive guides and references for integrating NEMeeting SDK into your applications.

## Documentation Structure

### Guides (`/guides`)

Complete integration guides, tutorials, and best practices for all supported platforms:

- **Android** - Mobile Android integration (`guides/Android/`)
- **iOS** - Mobile iOS integration (`guides/iOS/`)
- **Web** - Web browser integration (`guides/Web/`)
- **H5** - Mobile H5 integration (`guides/H5/`)
- **Windows** - Windows desktop integration (`guides/Windows/`)
- **macOS** - macOS desktop integration (`guides/macOS/`)
- **Linux** - Linux platform integration (`guides/Linux/`)
- **Electron** - Cross-platform desktop apps (`guides/Electron/`)

Each platform guide follows a consistent structure:

#### Directory Structure
```
guides/[Platform]/
├── 快速开始/              # Quick Start
│   ├── 快速跑通 Sample Code.md
│   ├── 集成并初始化 SDK.md
│   └── 登录鉴权.md
├── 会议管理/              # Meeting Management
│   ├── 创建会议.md
│   ├── 加入会议.md
│   ├── 预约会议.md
│   ├── 邀请入会.md
│   ├── 会议管理.md
│   └── 会议状态变更流程.md
├── 会议安全/              # Meeting Security
│   ├── 概述.md
│   ├── 会中水印.md
│   ├── 入会音视频状态控制.md
│   └── 媒体流加密.md (或 流媒体加密.md for iOS)
├── 界面定制/              # UI Customization
│   ├── 概述.md
│   ├── 设置语言&主题.md
│   ├── 自定义菜单.md
│   └── 其他界面定制.md (或 其他界面设置.md for Web)
├── 基础美颜.md            # Beauty Filters
├── 虚拟背景.md            # Virtual Backgrounds
├── 会议扩展性.md          # Meeting Extensions
├── 复用 IM 账号.md        # IM Account Integration (Android/iOS/H5/Web)
├── 私有云配置.md          # Private Cloud Configuration
├── 屏幕共享.md            # Screen Sharing (iOS only)
├── 会议巡检.md            # Meeting Inspection (Web only)
├── 常见问题/              # Troubleshooting
│   ├── 会议安全.md
│   ├── 依赖冲突.md (Android/iOS)
│   ├── 功能版本对照.md (Android/iOS/Electron)
│   └── 集成编译打包.md (Electron)
├── 更新日志.md            # Changelog
└── 集成概述.md            # Integration Overview
```

#### Platform-Specific Notes
- **iOS**: Uses `流媒体加密.md` instead of `媒体流加密.md`, includes `屏幕共享.md`
- **Web**: Uses `其他界面设置.md` instead of `其他界面定制.md`, includes `会议巡检.md`
- **H5**: Uses `集成并初始化SDK.md` (no space) instead of `集成并初始化 SDK.md`
- **Android/iOS/H5/Web**: Include `复用 IM 账号.md`
- **Android/iOS**: Include `依赖冲突.md` in 常见问题
- **Android/iOS/Electron**: Include `功能版本对照.md` in 常见问题
- **Electron**: Includes `集成编译打包.md` in 常见问题

### API Reference (`/api-reference`)

*Detailed API references* - Visit: [API Reference](./api-reference/README.md)

## Getting Started

1. Choose your target platform from the `/guides` directory
2. Follow the Quick Start guide to download SDK and run the sample code
3. Integrate the SDK into your project following the integration guide
4. Explore advanced features based on your use case

## Platform-Specific Quick Links

### Mobile Platforms

| Platform | Quick Start | Integration | Download SDK |
|----------|-------------|-------------|--------------|
| **Android** | [实现音视频会议](guides/Android/快速开始/) | [集成 SDK](guides/Android/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/Android/快速开始/) |
| **iOS** | [实现音视频会议](guides/iOS/快速开始/) | [集成 SDK](guides/iOS/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/iOS/快速开始/) |
| **H5** | [实现音视频会议](guides/H5/快速开始/) | [集成 SDK](guides/H5/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/H5/快速开始/) |

### Desktop Platforms

| Platform | Quick Start | Integration | Download SDK |
|----------|-------------|-------------|--------------|
| **Windows** | [实现音视频会议](guides/Windows/快速开始/) | [集成 SDK](guides/Windows/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/Windows/快速开始/) |
| **macOS** | [实现音视频会议](guides/macOS/快速开始/) | [集成 SDK](guides/macOS/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/macOS/快速开始/) |
| **Linux** | [实现音视频会议](guides/Linux/快速开始/) | [集成 SDK](guides/Linux/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/Linux/快速开始/) |

### Web & Cross-Platform

| Platform | Quick Start | Integration | Download SDK |
|----------|-------------|-------------|--------------|
| **Web** | [实现音视频会议](guides/Web/快速开始/) | [集成 SDK](guides/Web/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/Web/快速开始/) |
| **Electron** | [实现音视频会议](guides/Electron/快速开始/) | [集成 SDK](guides/Electron/快速开始/集成并初始化%20SDK.md) | [下载 SDK](guides/Electron/快速开始/) |

## Key Features

### Core Capabilities

- **Multi-platform Support** - Native SDKs for 8 platforms including mobile, desktop, and web
- **High Quality Audio/Video** - Professional-grade real-time video conferencing
- **Flexible Architecture** - Support for various scenarios:
  - 1-on-1 video calls
  - Multi-party group meetings
  - Scheduled meetings and instant meetings
  - Meeting controls and participant management

### Feature Index with File Paths

#### Quick Start & Integration
- **快速跑通 Sample Code** - `guides/[Platform]/快速开始/快速跑通 Sample Code.md`
- **集成并初始化 SDK** - `guides/[Platform]/快速开始/集成并初始化 SDK.md` (或 `集成并初始化SDK.md` for H5)
- **登录鉴权** - `guides/[Platform]/快速开始/登录鉴权.md`
- **集成概述** - `guides/[Platform]/集成概述.md`

#### Meeting Management (会议管理)
- **创建会议** - `guides/[Platform]/会议管理/创建会议.md`
- **加入会议** - `guides/[Platform]/会议管理/加入会议.md`
- **预约会议** - `guides/[Platform]/会议管理/预约会议.md`
- **邀请入会** - `guides/[Platform]/会议管理/邀请入会.md`
- **会议管理** - `guides/[Platform]/会议管理/会议管理.md` (离开/结束会议、成员加入或离开会议、会议设置)
- **会议状态变更流程** - `guides/[Platform]/会议管理/会议状态变更流程.md`

#### Meeting Security (会议安全)
- **会中水印** - `guides/[Platform]/会议安全/会中水印.md`
- **入会音视频状态控制** - `guides/[Platform]/会议安全/入会音视频状态控制.md`
- **媒体流加密** - `guides/[Platform]/会议安全/媒体流加密.md` (或 `流媒体加密.md` for iOS)
- **概述** - `guides/[Platform]/会议安全/概述.md`

#### UI Customization (界面定制)
- **设置语言&主题** - `guides/[Platform]/界面定制/设置语言&主题.md` (切换主题色、设置语言)
- **自定义菜单** - `guides/[Platform]/界面定制/自定义菜单.md`
- **其他界面定制** - `guides/[Platform]/界面定制/其他界面定制.md` (或 `其他界面设置.md` for Web)
- **概述** - `guides/[Platform]/界面定制/概述.md`

#### Advanced Features
- **基础美颜** - `guides/[Platform]/基础美颜.md`
- **虚拟背景** - `guides/[Platform]/虚拟背景.md`
- **会议扩展性** - `guides/[Platform]/会议扩展性.md`
- **复用 IM 账号** - `guides/[Platform]/复用 IM 账号.md` (Android/iOS/H5/Web)
- **私有云配置** - `guides/[Platform]/私有云配置.md`
- **屏幕共享** - `guides/iOS/屏幕共享.md` (iOS only)
- **会议巡检** - `guides/Web/会议巡检.md` (Web only)

#### Troubleshooting & Reference
- **常见问题** - `guides/[Platform]/常见问题/` (会议安全、依赖冲突、功能版本对照、集成编译打包等)
- **更新日志** - `guides/[Platform]/更新日志.md`
- **API Reference** - `api-reference/README.md`

## Support

- **Official Website** - [NetEase CommsEase](https://yunxin.163.com/)
- **Console** - [Management Console](https://app.yunxin.163.com/)
- **Technical Support** - Visit official website for support

## License

Please refer to NetEase CommsEase Service Agreement for SDK usage.
