本文主要介绍如何快速跑通网易会议组件的 [Windows Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Desktop)，体验在线的多人视频会议功能。

该示例项目包含了详细的 API 调用场景、参数封装以及回调处理。包含的功能如下：

- 通过账号、密码完成会议组件的登录鉴权、注销登录。
- 预约会议、创建会议、加入会议。
- 会议内提供的其他功能（如会议控制、屏幕共享等）。

## 开发环境

在开始运行示例项目之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| 操作系统 | Windows 10 及以上版本 |
| IDE | Qt Creator，Qt 6 及以上 |

## 前提条件

- 在网易云信控制台创建至少一个应用，获取应用的 AppKey。若无应用，请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA)。

- 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**，具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 安装环境

根据您的操作系统版本，前往 [Qt Group 官方网站](https://download.qt.io/official_releases/qt/) 下载并安装适合的 Qt 安装程序。

## 下载 Sample Code

从 github 下载网易会议组件 [Sample Code 源码](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Desktop/) ，或者直接在命令行运行以下命令：

```
git clone https://github.com/netease-kit/NEMeeting.git
```

Demo 工程主要用于展示 SDK 功能和 API 调用示例。Demo 工程存放在 `tree/main/SampleCode/Desktop/meeting_sdk_example_qt` 目录下。

## 配置 Sample Code

下载最新的会议组件离线包 <a href="https://doc.yunxin.163.com/meeting/resource?platform=windows" target="_blank">NEMeet_Windows_SDK</a>。下载并解压完成后，Windows SDK 目录结构如下：

```text
sdk/
├── include/                     # SDK 对外暴露的 C++ 头文件
├── lib/                         # SDK .lib 库文件
└── bin/                      
    ├── nemeet_sdk/*             # SDK 可执行程序和资源（独立进程）
    ├── nemeet_base_client.dll   # SDK 动态库，供宿主链接
    └── nemeet_base.dll          # SDK 动态库，供宿主链接
```

::: note note
您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide/zc1NTk4ODc?platform=windows)。
:::

1. 解压下载的 SDK 包，并将 `sdk` 文件夹复制到 `NEMeeting/SampleCode/Desktop/meeting_sdk_example_qt` 目录下。

    <img alt="image_example_sdk.png" src="https://yx-web-nosdn.netease.im/common/fe7899b0223012d4bfac6ad6d1379e1b/image_example_sdk.png" style="width:40%;border: 1px solid #BFBFBF;">

2. 编辑 `meeting_sdk_example_qt/build_win.bat` 文件，将 Qt 路径修改为正确的本地 Qt 安装路径。

   ::: note note
   此处的路径需要指定到 Qt 安装路径内部的 `msvc2019_64` 文件夹。
   :::

    ```Shell
    :: 设置你的QT_PATH
    :: Qt环境替换成本地Qt环境
    set QT_PATH=D:\Qt\6.7.2\msvc2019_64
    ```

3. 在 PowerShell 中直接运行脚本，编译工程。

    ```Shell
    $ cd meeting_sdk_example_qt
    $ .\build_win.bat
    ```

   构建完成后，最终的产物路径为 `build/Release/meeting_sdk_example_qt.exe`。

## 运行 Sample App

1. 在 **Your AppKey** 处填入应用 AppKey，然后单击左侧 **初始化** 按钮完成初始化操作。

   ![初始化.png](https://yx-web-nosdn.netease.im/common/2a3e114c3796af97e7488ed46698385b/初始化.png)

2. 切换到账号页，填入 **账号** 和 **token**，单击 **token登入** 完成登录操作。

   ![登录.png](https://yx-web-nosdn.netease.im/common/d9148050a911c15f43612a9f09667f0e/登录.png)

3. 切换到会议页，左侧选择 **startMeeting**，单击 **Run** 运行，完成创建会议操作。

   ![创建会议.png](https://yx-web-nosdn.netease.im/common/ea0c4ebe9e368daf9089e369e7358934/创建会议.png)
