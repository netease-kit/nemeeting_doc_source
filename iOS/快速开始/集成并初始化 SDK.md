本文主要介绍如何集成 NEMeetingKit，并对会议组件进行初始化。

## 开发环境

在客户端实现音视频会议功能之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| CPU 架构 | 支持 ARM64 架构 |
| iOS 系统 | 13 及以上版本的 iOS 设备 |
| IDE | XCode 16 及以上版本 |
| 其他 | 1.9.1 及以上版本的 CocoaPods |

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在 [网易云信控制台](https://app.yunxin.163.com/global/home) 创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA">创建应用并获取 AppKey</a>。
2. 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**。具体步骤可参考 [开通方案](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 集成 SDK

1. 新建 iOS 工程。

    1. 运行 XCode，依次选择 **Create a New XCode Project** > **Single View App** > **Next** 新建工程。

        <img style="width:80%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/b7e79e40feefe5ecab0d04930b87af43/ios_new_project.png" alt="image" />

    2. 配置工程相关信息，单击 **Next**。

        <img style="width:80%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/2deb73bd6f161a81e2f6e849a21ab2a4/ios_configure_project.png" alt="image" />

    3. 选择合适的工程本地路径，单击 **Create** 完成工程创建。

2. 通过 CocoaPods 集成 SDK。

    1. 进入到工程路径，执行 pod 命令，生成 `Podfile` 文件，注意 CocoaPods 版本使用 1.9.1 以上的，防止因为版本过低导致无法拉取 SDK。

        ```CocoaPods
            pod init
        ```

        ::: note notice
        从 4.12.0 版本起，NEMeetingKit 支持跨平台开发的 XCFramework 框架格式。SDK 目录下的 `.framework` 文件后缀名修改为 `.xcframework`。如果您集成 NEMeetingKit 后从低版本升级至 4.12.0 及以上，请重新添加 XCFramework 依赖。详情请参考《常见问题》[升级 NEMeetingKit 版本后，Xcode 编译报错文件找不到](https://doc.yunxin.163.com/meeting/faq/TY5Mjg4MTI?platform=client)。
        :::

    2. 打开 `Podfile` 文件添加如下代码并保存。

        ```CocoaPods
        pod 'NEMeetingKit'
        ```

        ::: note note
        您也可根据需要选择依赖的版本，详情可参考网易会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide/jk0NzYzNzA)。
        :::

    3. 执行 pod 命令，安装 SDK。

        ```CocoaPods
        pod install
        ```

        自 V3.11.0 版本起，NEMeetingKit 支持以插件化的方式按需加载子模块，以便 **缩小包体积**。集成方法如下：

        - 方式一：

            ```
            pod 'NEMeetingKit', :subspecs => ['Base', 'Beauty', 'Segment', 'Audio', 'Video', 'ScreenShare']
            ```
        
        - 方式 二：

            ```CocoaPods
            pod 'NEMeetingKit/Base'
            pod 'NEMeetingKit/Beauty'
            pod 'NEMeetingKit/Segment'
            pod 'NEMeetingKit/Audio'
            pod 'NEMeetingKit/Video'
            pod 'NEMeetingKit/ScreenShare'
            ```

        `subspecs` 中请填入待引入的动态库对应的值，具体说明如下表所示。

        功能/插件 | `subspecs` 的值 | 涉及功能 | 是否必选
        ---- | ---- | ---- | ----
        音视频 + IM | `Base` | 音视频通话、IM会控通信、聊天室等 | 必选
        美颜 + 人脸识别 | `Beauty` | 美颜、人脸检测 | 可选
        虚拟背景 | `Segment` | 背景分割替换 | 可选
        AI 降噪 + AI 啸叫检测 | `Audio` | AI降噪消除背景声、AI啸叫检测 | 可选
        视频超分 + 视频降噪 | `Video` | 提升视频清晰度和质量 | 可选
        屏幕共享 | `ScreenShare` | 共享本端界面 | 可选

3. 权限处理。NEMeetingKit 正常工作需要应用获取摄像头、麦克风、相册权限。

    1. 若您的 App 需要在退到后台时仍然运行相关功能，请打开后台音频权限。

        在 **Signing & Capabilities** 页面，将设置项 **Background Modes** 设定为 ON，并勾选 **Audio，AirPlay and Picture in Picture** 和 **Voice over IP**。

        <img alt="Xnip2022-12-02_16-33-26.jpg" src="https://yx-web-nosdn.netease.im/common/d55366102f9403d6c0f27109e25878fe/image.png" style="width:80%;border: 1px solid #BFBFBF;">

        打开后台音频权限之后，应用在手机后台运行时，SDK 默认在后台也可以继续处理音频流，维持通话。

    2. 若您的 App 需要正常使用 SDK 提供的音视频功能，请给 App 授权麦克风、摄像头、相册和 Wi-Fi 的使用权限。

        编辑 `Info.plist` 文件，添加以下四项。

        - **Privacy - Microphone Usage Description**，并填入麦克风使用目的提示语。
        - **Privacy - Contact Usage Description**，并填入通讯录使用目的提示语。
        - **Privacy - Camera Usage Description**，并填入摄像头使用目的提示语。
        - **Privacy - Photo Library Usage Description**，并填入相册使用目的提示语。

            <img alt="Xnip2022-12-02_16-46-19.jpg" src="https://yx-web-nosdn.netease.im/common/3be28d03c212fd0684dc02929cb13709/image.png" style="width:80%;border: 1px solid #BFBFBF;">
            <!-- <img style="width:80%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/a288fbc3d27737d3789e5f00335f1b39/info.png" alt="image" /> -->

<a id="Special"></a>

## 初始化

### 调用时机

在调用 SDK 其他接口之前，您首先需要完成初始化操作。

### 注意事项

- 初始化是一个异步操作，您需要确保异步回调成功之后，再进行调用 API。
- 应用名称会显示在会议界面的顶部标题栏中。若不额外设置应用名称，则标题默认显示为 会议。

### 调用步骤

1. 配置初始化相关参数，详情请参考 [`NEMeetingKitConfig`](https://doc.yunxin.163.com/docs/interface/meetingkit/iOS/doxygen/Latest/zh/interface_n_e_meeting_kit_config.html)。

   **示例代码** 如下：

    ```Objective-C
    NEMeetingKitConfig *config = [[NEMeetingKitConfig alloc] init];
    config.appKey = [DemoConfig shareConfig].appKey; //应用 App Key
    config.appName = @"your_appName"; //应用 AppName
    ```
2. 调用 [`initialize`](https://doc.yunxin.163.com/docs/interface/meetingkit/iOS/doxygen/Latest/zh/interface_n_e_meeting_kit.html#a330550c5bf498dfe8da471c0e50e53d5) 方法完成初始化操作。该接口无额外回调结果数据。

   **示例代码** 如下：

    ```Objective-C
    [[NEMeetingKit getInstance] initialize:config
                            callback:^(NSInteger resultCode, NSString *resultMsg, id result)
    {
        if (resultCode == ERROR_CODE_SUCCESS) {
            //初始化成功
        } else {
            //初始化失败
        }
    }];
    ```
3. （可选）当您不确定是否已经初始化会议组件，可调用 [`isInitialized`](https://doc.yunxin.163.com/docs/interface/meetingkit/iOS/doxygen/Latest/zh/interface_n_e_meeting_kit.html#a9b4b12d78cd4745b8022e18a813df0f5) 添加查询是否已经初始化的调用。

   **示例代码** 如下：

    ```Objective-C
    BOOL isInitialized = [[NEMeetingKit getInstance] isInitialized];
    if (isInitialized) {
        // 如果已经初始化，执行后续操作
        // ...
    } else {
        // 如果未初始化，可能需要重新初始化或者记录日志
        NSLog(@"Meeting component is not initialized.");
    }
    ```