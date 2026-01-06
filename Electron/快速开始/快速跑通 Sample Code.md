本文主要介绍如何快速跑通网易会议组件的 [Electron Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Electron)，体验在线的多人视频会议功能。

该示例项目包含了详细的 API 调用场景、参数封装以及回调处理。包含的功能如下：

- 通过账号、密码完成会议组件的登录鉴权、注销登录。
- 预约会议、创建会议、加入会议。
- 会议内提供的其他功能（如会议控制、屏幕共享等）。

## 开发环境

在客户端实现音视频会议功能之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| Node 版本 | 14 及以上版本 |
| Electron 版本 | 22 及以上版本。<note type=note>如果需要支持 MacOS 系统，会议组件SDK在 Electron 25 及以上版本的存在兼容性问题，可能导致部分功能异常，请使用 24.8.8 或更低版本。</note> |

## 前提条件

- 在网易云信控制台创建至少一个应用，获取应用的 AppKey。若无应用，请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA)。

- 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**，具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 下载 Sample Code

1. 从 github 下载网易会议组件 Sample Code 源码，或者直接在命令行运行以下命令：

    ```
    git clone https://github.com/netease-kit/NEMeeting.git
    ```

2. 用 vscode 等常用代码编辑器打开 `NEMeeting/SampleCode/Electron` 项目。

## 运行 Sample Code

您在申请并声明 AppKey 后，运行并编译示例项目，即可体验 **登录**、**预约会议**、**加入会议**、**创建会议** 功能。

1. 在终端运行以下命令安装依赖。

    ```
    npm install
    ```

2. 在终端运行以下命令运行 Sample Code。

    ```
    npm run start
    ```

3. 配置 AppKey。

    项目运行后，在输入框填写您的会议组件 AppKey。

效果图如下所示：

<img style="width:60%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/f1378694fa193157ce3886402a854a9e/meetingkit_electron_demo_main.png" alt="meetingkit_demo_main" />