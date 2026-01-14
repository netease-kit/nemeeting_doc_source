本文主要介绍如何快速跑通网易会议组件的 [macOS Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Desktop)，体验在线的多人视频会议功能。

该示例项目包含了详细的 API 调用场景、参数封装以及回调处理。包含的功能如下：

- 通过账号、密码完成会议组件的登录鉴权、注销登录。
- 预约会议、创建会议、加入会议。
- 会议内提供的其他功能（如会议控制、屏幕共享等）。

<!--
## Sample Code 说明

功能| 网易会议 AppKey | 网易会议账号
--- | --- | ---
加入会议 | 需要 | 不需要
创建会议 | 需要 | 需要
预约会议 | 需要 | 需要
-->

## 开发环境

在开始运行示例项目之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| 操作系统 | macOS 10.11 + 版本 |
| IDE | Qt Creator，Qt 6 及以上 |
| 其他 | 有效的开发者签名，设备权限配置 |

## 前提条件

- 在网易云信控制台创建至少一个应用，获取应用的 AppKey。若无应用，请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA)。

- 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**，具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 安装 Qt 环境

根据您的操作系统版本，前往 [Qt Group 官方网站](https://download.qt.io/official_releases/qt/) 下载并安装适合的 Qt 安装程序。

## 下载 Sample Code

从 github 下载网易会议组件 [Sample Code 源码](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Desktop/)，或者直接在命令行运行以下命令：

```
git clone git@github.com:netease-kit/NEMeeting.git
```

Demo 工程主要用于展示 SDK 功能和 API 调用示例。Demo 工程存放在 `NEMeeting/SampleCode/Desktop/meeting_sdk_example_qt` 目录下。

## 配置 Sample Code

下载最新的会议组件离线包 [NEMeet_macOS_SDK](https://doc.yunxin.163.com/meeting/resource?platform=macOS)。下载并解压完成后，macOS SDK 目录结构如下：

```Bash
.
├── meeting_sdk_example_qt
    └── meeting_sdk_example_qt.app <-- 可执行的 Qt 示例程序
└── sdk 
    └── bin 
        └── nemeet_sdk.app <-- 会议 SDK 可执行程序
    └── include <-- SDK 接口头文件
    └── lib <-- SDK 动态库
```

::: note note
您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide?platform=macOS)。
:::

1. 解压下载的 SDK 包，并将 `sdk` 文件夹复制到 `NEMeeting/SampleCode/Desktop/meeting_sdk_example_qt` 目录下。

    <img alt="image_example_sdk.png" src="https://yx-web-nosdn.netease.im/common/fe7899b0223012d4bfac6ad6d1379e1b/image_example_sdk.png" style="width:40%;border: 1px solid #BFBFBF;">

2. 编辑 `meeting_sdk_example_qt/build_mac.sh` 文件，将 Qt 路径修改为正确的本地 Qt 安装路径。

   ::: note note
   此处的路径需要指定到 Qt 安装路径内部的 `macos` 文件夹。
   :::

    ```Shell
    #!/bin/sh

    rm -rf build
    #替换成本地 Qt 环境
    Qt=~/Qt/6.4.3/macos
    ```

3. 执行脚本，编译工程。

    ```Shell
    $ cd meeting_sdk_example_qt
    $ sh build_mac.sh
    ```

   编译过程中，如果出现以下的安全提示弹窗，单击 **仍要打开**，或在 **设置-隐私与安全** 中选择 **仍要打开**。

   ![仍要打开.png](https://yx-web-nosdn.netease.im/common/ad2c7cee4d27b9d8ba68bf879459d110/仍要打开.png)

   构建完成后，最终的产物路径为 `build/meeting_sdk_example_qt.app`。

## 使用 Sample App

1. 在 **Your AppKey** 处填入应用 AppKey，然后单击左侧 **初始化** 按钮完成初始化操作。

   ![初始化.png](https://yx-web-nosdn.netease.im/common/ad2ba56c030f5f2bb19b3268b6d85bd0/初始化.png)

2. 切换到账号页，填入 **账号** 和 **token**，单击 **token登入** 完成登录操作。

   ![token登录.png](https://yx-web-nosdn.netease.im/common/d78acb34496f475979321222e09e662e/token登录.png)

3. 切换到会议页，左侧选择 **startMeeting**，单击 **Run** 运行，完成创建会议操作。

   ![创建会议.png](https://yx-web-nosdn.netease.im/common/55794d1b8c2e10d9ef7c0e2b3752f550/创建会议.png)

   ::: note note
   在运行过程中，如果出现以下的安全提示弹窗，单击 **仍要打开**，或在 **设置-隐私与安全** 中选择 **仍要打开**。
   :::

   ![仍要打开1.png](https://yx-web-nosdn.netease.im/common/fdcc50114e4dd93af4cc35f41693a41a/仍要打开1.png)

   ![仍要打开2.png](https://yx-web-nosdn.netease.im/common/3f801fd21e926daa273547dca2a86c02/仍要打开2.png)