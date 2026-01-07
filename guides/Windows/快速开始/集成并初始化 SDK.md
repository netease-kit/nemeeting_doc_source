本文主要介绍如何集成 NEMeetingKit，并对会议组件进行初始化。

## 开发环境

在客户端实现音视频会议功能之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| 操作系统 | Windows 10 及以上版本 |
| IDE | Qt Creator，Qt 6 及以上 |

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在 [网易云信控制台](https://app.yunxin.163.com/global/home) 创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA">创建应用并获取 AppKey</a>。

2. 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**。具体步骤可参考 [开通方案](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 集成 SDK

本文主要以 cmake 的形式管理 Qt demo 工程。

1. 创建 Qt 工程。

2. 下载最新的会议组件离线包 [NEMeet_windows_SDK](https://doc.yunxin.163.com/meeting/resource?platform=windows)。

    ::: note note
    您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide?platform=windows)。
    :::

3. 添加 SDK 编译依赖。

    ```
    // 以 cmake 为例
    CMakeLists.txt
    include_directories(
        ${CMAKE_CURRENT_LIST_DIR}/sdk/include
    )

    link_directories(${CMAKE_CURRENT_LIST_DIR}/sdk/lib)
    ```

## 初始化

### 调用时机

在调用 SDK 其他接口之前，您首先需要完成初始化操作。

### 注意事项

- 初始化是一个异步操作，您需要确保异步回调成功之后，再进行调用 API。
- 应用名称会显示在会议界面的顶部标题栏中。若不额外设置应用名称，则标题默认显示为 **会议**。

### 调用步骤

1. 配置初始化相关参数，详情请参考 [`NEMeetingKitConfig`](https://doc.yunxin.163.com/docs/interface/meetingkit/windows/doxygen/Latest/zh/class_n_e_meeting_kit_config.html)。

    **示例代码如下：**

    ```C++
    std::string appkey = "APPKEY";
    std::string runPath = "NE_MEET_RUN_PATH";
    nem_sdk_interface::NEMeetingKitConfig config;
    config.setAppKey(appkey);
    config.getAppInfo()->SDKPath(runPath);
    ```
    
2. 调用 [`initialize`](https://doc.yunxin.163.com/docs/interface/meetingkit/windows/doxygen/Latest/zh/class_n_e_meeting_kit.html#a5000a39a6cd3205465972cca78fb0810) 方法完成初始化操作。该接口无额外回调结果数据。

   **示例代码如下：**

    ```C++
    NEMeetingKit::getInstance()->initialize(config, [this]( MeetingErrorCode errorCode, const std::string& errorMessage, const NEMeetingCorpInfo& info) {
        if (errorCode == kSuccess) {
        }
    });
    ```

3. （可选）当您不确定是否已经初始化会议组件，可调用 [`isInitialized`](https://doc.yunxin.163.com/docs/interface/meetingkit/windows/doxygen/Latest/zh/class_n_e_meeting_kit.html#a7852afaf34e51f19bac44f538c86e107) 添加查询是否已经初始化的调用。

   **示例代码如下：**

    ```C++
    // 检查会议组件是否已经初始化
    bool isInitialized = NEMeetingKit::getInstance()->isInitialized();
    ```