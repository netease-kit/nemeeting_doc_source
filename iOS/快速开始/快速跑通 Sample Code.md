本文主要介绍如何快速跑通网易会议组件的 [iOS Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/iOS)，体验在线的多人视频会议功能。

该示例项目包含了详细的 API 调用场景、参数封装以及回调处理。包含的功能如下：

- 通过账号、密码完成会议组件的登录鉴权、注销登录。
- 预约会议、创建会议、加入会议。
- 会议内提供的其他功能（如会议控制、屏幕共享等）。

## 开发环境

在开始运行示例项目之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| XCode | 16 及以上版本 |
| 调试设备 | iOS 13 及以上版本，仅支持使用 **真机** 进行调试和运行 |
| 其他 | 有效的开发者签名，设备权限配置 |

## 前提条件

- 在网易云信控制台创建至少一个应用，获取应用的 AppKey。若无应用，请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA)。

- 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**，具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 下载 Sample Code

1. 从 github 下载网易会议组件 Sample Code 源码，或者直接在命令行运行以下命令：

    ```
    git clone https://github.com/netease-kit/NEMeeting.git
    ```

2. 通过 XCode 打开 `NEMeeting/SampleCode/iOS` 项目。

## 配置 Sample Code

### 配置 SDK

打开 `Podfile` 文件添加如下代码并保存。

```CocoaPods
pod 'NEMeetingKit'
```

::: note note
您也可根据业务需要选择依赖的版本，详情可参考网易会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide/jk0NzYzNzA?platform=iOS)。
:::

### 配置 AppKey

在 `NEMeeting/SampleCode/iOS/NEMeetingDemo/ServerConfig.m` 源代码文件中，将您的会议组件 App Key 填写至对应的变量定义中。

```Objective-C
NSString *const kAppKey = @"Your_Meeting_App_Key";
```

## 运行 Sample Code

您在申请并声明 AppKey 后，运行并编译示例项目，即可体验 **登录**、**预约会议**、**加入会议**、**创建会议** 功能。

1. 进入 `NEMeeting/SampleCode/iOS` 路径，执行如下命令。

    ```
    pod install
    ```

2. 连接上 iOS 设备后，用 XCode 打开 `NEMeeting/SampleCode/iOS` 示例项目，然后编译并运行示例项目。

    ::: note notice
    请使用 iOS 真机设备进行调试和运行，暂不支持模拟器设备。
    :::

效果图如下所示：

<img style="width:30%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/ed5525e11091b26031aee9230d573d7b/meeting_demo_main_page_iOS.png" alt="image" />