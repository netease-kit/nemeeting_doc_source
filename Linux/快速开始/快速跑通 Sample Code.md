本文主要介绍如何快速跑通网易会议组件的 [Linux Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Windows_macOS)，体验在线的多人视频会议功能。

该示例项目包含了详细的 API 调用场景、参数封装以及回调处理。包含的功能如下：

- 通过账号、密码完成会议组件的登录鉴权、注销登录。
- 预约会议、创建会议、加入会议。
- 会议内提供的其他功能（如会议控制、屏幕共享等）。

## 开发环境

在开始运行示例项目之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| 操作系统 | Ubuntu 18 + 版本 |
| IDE | Qt Creator，Qt 6 及以上 |

## 前提条件

- 在网易云信控制台创建至少一个应用，获取应用的 AppKey。若无应用，请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA)。

- 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**，具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 安装环境

根据您的操作系统版本，前往 [Qt Group 官方网站](https://download.qt.io/official_releases/qt/) 下载并安装适合的 Qt 安装程序。

## 下载 Sample Code

从 github 下载网易会议组件 [Sample Code 源码](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Windows_macOS/)，或者直接在命令行运行以下命令：

```
git clone git@github.com:netease-kit/NEMeeting.git
```

Demo 工程主要用于展示 SDK 功能和 API 调用示例。Demo 工程存放在 `tree/main/SampleCode/Windows_linux/meeting_sdk_example_qt` 目录下。

## 配置 Sample Code

下载最新的会议组件离线包 [NEMeet_linux_SDK](https://doc.yunxin.163.com/meeting/resource?platform=linux)。下载并解压完成后，linux SDK 目录结构如下：

```Bash
.
├── meeting_sdk_example_qt <-- 示例程序
└── sdk 
    └── bin 
        └── nemeet_sdk
    └── include <-- SDK 接口头文件
    └── lib <-- SDK 依赖的资源文件和库文件
```

::: note note
您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide?platform=linux)。
:::

1. 解压下载的 SDK 包，进入 `sdk` 目录。

2. 在 Visual Studio Code 中打开 `meeting_sdk_example_qt/` 工程。

3. 若 Demo 工程中有 `sdk` 文件夹，需要删除。

4. 根据当前系统的硬件架构，将对应目录下的 `sdk` 复制到 `meeting_sdk_example_qt/` 目录下。

    <img alt="image_example_sdk.png" src="https://yx-web-nosdn.netease.im/common/fe7899b0223012d4bfac6ad6d1379e1b/image_example_sdk.png" style="width:40%;border: 1px solid #BFBFBF;">

5. 运行工程。

    ```Shell
    cd meeting_sdk_example_qt
    #Qt环境替换成本地Qt环境

    Qt=/home/yunxin/Qt/6.5.3/gcc_64

    cmake   -Bmeeting_sdk_example_qt  -G"Unix Makefiles"  \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_PREFIX_PATH=${Qt}

    cmake   --build meeting_sdk_example_qt --config Release
    /usr/local/bin/linuxdeployqt  meeting_sdk_example_qt/meeting_sdk_example_qt -qmake=/home/yunxin/Qt/6.5.3/gcc_64/bin/qmake
    ```

## 运行 Sample App 

在输入框中填写您的会议组件 AppKey。

您在申请并声明 AppKey 后，运行并编译示例项目，即可体验 **登录**、**预约会议**、**加入会议**、**创建会议** 功能。

`nemeet_sdk` 的路径为 `sdk/bin/nemeet_sdk` 目录下的 `nemeet_sdk` 文件。需要确定本地 `nemeet_sdk` 文件的路径是否正确。

本地路径示例：`sdk/bin/nemeet_sdk/nemeet_sdk`

效果图如下所示：

<img style="width:80%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/8b7b21a48137c9fb03a412e167e1ee2b/image_init.png" alt="image" />