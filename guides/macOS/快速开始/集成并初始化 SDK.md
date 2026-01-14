本文主要介绍如何集成 NEMeetingKit，并对会议组件进行初始化。

## 开发环境

在客户端实现音视频会议功能之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| 操作系统 | macOS 10.11 + 版本 |
| IDE | Qt Creator，Qt 6 及以上 |
| 其他 | 有效的开发者签名，设备权限配置 |

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在 [网易云信控制台](https://app.yunxin.163.com/global/home) 创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA">创建应用并获取 AppKey</a>。
2. 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**。具体步骤可参考 [开通方案](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## SDK 概述

NEMeetingKit macOS SDK 采用 **独立进程架构** 设计，将会议核心能力运行在独立的 SDK 可执行程序中，对外通过 SDK 动态库 + 头文件的方式提供统一接口，供宿主应用（App）集成使用。

### SDK 目录结构

SDK 的标准目录结构如下：

```text
sdk/
├── bin/
│   └── nemeet_sdk.app        # 会议 SDK 可执行程序（独立进程）
├── include/                  # SDK 对外暴露的 C++ 头文件
└── lib/                      # SDK 动态库（供宿主进程链接）
    └── libnemeet_base.dylib
```

各目录说明：

| 目录 | 说明 |
|----|----|
| `bin/nemeet_sdk.app` | SDK 核心进程，负责会议引擎、音视频处理、网络通信等核心能力。 |
| `include/*` | SDK API 声明，定义初始化、登录、入会、回调等接口。 |
| `lib/` | SDK API 实现动态库。 |

### 工作方式

SDK 通过 **IPC（进程间通信）机制** 实现宿主进程与 SDK 进程之间的交互：

1. 宿主应用调用 SDK 动态库提供的 API。
2. SDK 动态库通过 IPC 将请求转发给独立的 SDK 进程（`nemeet_sdk.app`）。
3. SDK 进程执行会议相关业务逻辑（会议控制、音视频处理、网络通信等）。
4. 执行结果通过 IPC 返回给 SDK 动态库，最终通过回调通知宿主应用。

开发者无需关注底层通信细节，只需通过 SDK 接口完成初始化、登录、入会等操作。

### 架构特点

- **进程隔离**：会议 SDK 运行在独立进程中，避免 SDK 崩溃影响宿主应用，提升整体稳定性。
- **统一接口层**：宿主应用仅通过 SDK 动态库暴露的 API 进行交互，无需关心底层实现细节。
- **异步回调模型**：所有耗时操作（初始化、登录、入会等）均通过回调返回结果。
- **跨语言友好**：SDK 接口为 C++ API，便于 Qt / 原生 C++ 等不同技术栈集成。

## 集成 SDK

本文以 QT 工程为例，说明如何集成 NEMeetingKit SDK。

### 步骤一：创建 Qt 工程

创建您的 Qt 工程或打开现有工程。

### 步骤二：下载 SDK

下载最新的会议组件离线包 [NEMeet_macOS_SDK](https://doc.yunxin.163.com/meeting/resource?platform=macOS)。

::: note note
您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide?platform=macOS)。
:::

### 步骤三：添加 SDK 编译依赖

将 `sdk` 目录拷贝到工程目录中， 并配置头文件路径与链接。

#### CMake 示例

```cmake
# 配置 `sdk/include` 为头文件路径
include_directories(
    ${CMAKE_CURRENT_LIST_DIR}/sdk/include
)

# 配置 `sdk/lib` 为动态库链接搜索路径
link_directories(${CMAKE_CURRENT_LIST_DIR}/sdk/lib)

# 链接 SDK 动态库（请将 your_app 替换为您的实际目标名称）
target_link_libraries(your_app PRIVATE
    nemeet_base
)
```

#### Qt.pro 文件示例

```pro
INCLUDEPATH += $$PWD/sdk/include
LIBS += -L$$PWD/sdk/lib -lnemeet_base
```

### 步骤四：构建应用

完成编译依赖配置后，构建您的应用。

::: note note
在构建应用可执行程序时，务必确保 `bin/nemeet_sdk.app` SDK 提供的 **独立进程可执行程序** 跟随宿主一起打包和分发。
:::

## 初始化

### 调用时机

在调用 SDK 其他接口之前，您首先需要完成初始化操作。

### 注意事项

- 初始化是一个异步操作，您需要确保异步回调成功之后，再进行调用 API。
- 应用名称会显示在会议界面的顶部标题栏中。若不额外设置应用名称，则标题默认显示为 **会议**。

### 调用步骤

1. 配置初始化相关参数，详情请参考 [`NEMeetingKitConfig`](https://doc.yunxin.163.com/docs/interface/meetingkit/macOS/doxygen/Latest/zh/class_n_e_meeting_kit_config.html)。

   **示例代码如下：**

    ```C++
    std::string appkey = "APPKEY";
    std::string runPath = "NE_MEET_RUN_PATH";
    nem_sdk_interface::NEMeetingKitConfig config;
    config.setAppKey(appkey);
    config.getAppInfo()->SDKPath(runPath);
    ```

   ::: note note
    - `appkey` 填入您管理后台创建的应用 AppKey。
    - `runPath` 应指向 `nemeet_sdk.app/Contents/MacOS/nemeet_sdk` 可执行程序的完整路径，例如：`/path/to/your/app/Contents/Resources/nemeet_sdk.app/Contents/MacOS/nemeet_sdk`。
      :::

2. 调用 [`initialize`](https://doc.yunxin.163.com/docs/interface/meetingkit/macOS/doxygen/Latest/zh/class_n_e_meeting_kit.html#a5000a39a6cd3205465972cca78fb0810) 方法完成初始化操作。

   **示例代码如下：**

    ```C++
    NEMeetingKit::getInstance()->initialize(config, [this]( MeetingErrorCode errorCode, const std::string& errorMessage, const NEMeetingCorpInfo& info) {
        PrintLog("MeetingSDK initialize errorMessage: " + errorMessage);
        if (errorCode == kSuccess) {
            // 初始化成功，可以开始使用 SDK 其他功能
        } else {
            // 初始化失败，请检查错误信息并处理
        }
    });
    ```

3. （可选）当您不确定是否已经初始化会议组件，可调用 [`isInitialized`](https://doc.yunxin.163.com/docs/interface/meetingkit/macOS/doxygen/Latest/zh/class_n_e_meeting_kit.html#a7852afaf34e51f19bac44f538c86e107) 查询是否已经初始化。

   **示例代码如下：**

    ```C++
    // 检查会议组件是否已经初始化
    bool isInitialized = NEMeetingKit::getInstance()->isInitialized();
    if (!isInitialized) {
        // 执行初始化操作
    }
    ```

4. 当不再使用 SDK 或需要更换 AppKey 时，调用 [`uninitialize`](https://doc.yunxin.163.com/docs/interface/meetingkit/macOS/doxygen/Latest/zh/class_n_e_meeting_kit.html#a37bec3743ba9342a5f92ede090c9b3ab) 方法进行反初始化。

   **示例代码如下：**

    ```C++
    NEMeetingKit::getInstance()->unInitialize([this](MeetingErrorCode errorCode, const std::string& errorMessage) {
        PrintLog("MeetingSDK unInitialize code:" + std::to_string(errorCode) + ", errorMessage: " + errorMessage);
    });
    ```