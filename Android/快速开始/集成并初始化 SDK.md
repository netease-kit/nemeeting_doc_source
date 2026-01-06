本文主要介绍如何集成 NEMeetingKit，并对会议组件进行初始化。

## 开发环境

在客户端实现音视频会议功能之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| JDK | 17 及以上版本 |
| Android API 版本 | Android SDK API 等级 21 及以上, Android 5.0 及以上版本 |
| CPU 架构 | 支持 ARM64、ARMV7 架构 |
| IDE | Android Studio |
| 其他 | 依赖 Androidx，不支持 support 库 |

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在 [网易云信控制台](https://app.yunxin.163.com/global/home) 创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA">创建应用并获取 AppKey</a>。
2. 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**。具体步骤可参考 [开通方案](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 集成 SDK

1. 新建 Android 工程。

    1. 运行 Android Studio，在顶部菜单依次选择 **File** > **New** > **New Project** 新建工程。再依次选择 **Phone and Tablet** > **Empty Activity**，单击 **Next**。

        <img style="width:50%" src="https://yx-web-nosdn.netease.im/common/5c330c3371274f9cb714a48f8865d3bc/android_create_project.png" alt="image" />

    2. 配置工程相关信息，请注意 **Minimum SDK** 的 Android API Level 为 API 21。

        <img style="width:50%" src="https://yx-web-nosdn.netease.im/common/ad36bb4ff0b2b1c45da31c4be11f701a/android_create_project_set.png" alt="image" />

    3. 单击 **Finish** 完成工程创建。

2. 添加 SDK 编译依赖。

    1. 修改工程目录下的 `app/build.gradle` 文件，添加网易会议组件的依赖。

        ```Groovy
        android {
          // 添加 packagingOptions，否则可能会造成资源文件冲突。
          packagingOptions {
            pickFirst 'lib/arm64-v8a/libc++_shared.so'
            pickFirst 'lib/armeabi-v7a/libc++_shared.so'
          }
        }

        dependencies {
          //声明 SDK 依赖，具体版本可根据您的实际需要修改。
          implementation 'com.netease.yunxin.kit.meeting:meeting:x.x.x'
        }
        ```

        ::: note note
        您可根据需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide/zk0MjM1OTg?platform=android)。
        :::

    2. 在顶部菜单依次选择 **Build** > **Make Project** 构建工程，触发依赖下载，完成后即可在代码中引入 SDK 中的类和方法。

## 初始化

### 调用时机

在调用 SDK 其他接口之前，您首先需要完成初始化操作。

### 注意事项

- 初始化是一个异步操作，您需要确保异步回调成功之后，再进行调用 API。
- 应用名称会显示在会议界面的顶部标题栏中。若不额外设置应用名称，则标题默认显示为 **会议**。
<!--默认情况下，这里不需要手动配置，sdk 会自动检测并配置
- 请在初始化网易会议组件 SDK 时配置前台服务，防止会议进程退到后台时被系统杀死，且保证使用 Android 系统可以正常共享屏幕。详细配置参考 [`NEForegroundServiceConfig`](https://doc.yunxin.163.com/meetingkit/references/android/dokka/Latest/zh/com/netease/yunxin/kit/meeting/sdk/config/NEForegroundServiceConfig.html)。
-->

### 调用步骤

1. 配置初始化相关参数，详情请参考 [`NEMeetingKitConfig`](https://doc.yunxin.163.com/meetingkit/references/android/dokka/Latest/zh/com/netease/yunxin/kit/meeting/sdk/NEMeetingKitConfig.html)。

   **示例代码如下：**

    ```Java
    NEMeetingKitConfig config = new NEMeetingKitConfig();
    config.appKey = Constants.APPKEY; //应用 AppKey
    config.appName = context.getString(R.string.app_name); //应用 AppName
    ```
2. 调用 [`initialize`](https://doc.yunxin.163.com/meetingkit/references/android/dokka/Latest/zh/com/netease/yunxin/kit/meeting/sdk/NEMeetingKit.html#initialize(android.content.Context,com.netease.yunxin.kit.meeting.sdk.NEMeetingKitConfig,com.netease.yunxin.kit.meeting.sdk.NECallback)) 方法完成初始化操作。该接口无额外回调结果数据。

   **示例代码如下：**

    ```Java
    NEMeetingKit.getInstance().initialize(getApplication(), config, new NECallback<Void>() {
        @Override
        public void onResult(int resultCode, String resultMsg, Void result) {
            if (resultCode == NEMeetingError.ERROR_CODE_SUCCESS) {
                //初始化成功
            } else {
                //初始化失败
            }
        }
    });
    ```
3. （可选）当您不确定是否已经初始化会议组件，可调用 [`isInitialized`](https://doc.yunxin.163.com/meetingkit/references/android/dokka/Latest/zh/com/netease/yunxin/kit/meeting/sdk/NEMeetingKit.html#isInitialized()) 添加查询是否已经初始化的调用。

   **示例代码如下：**

    ```Java
    // 检查会议组件是否已经初始化
    boolean isInitialized = NEMeetingKit.getInstance().isInitialized();
    if (isInitialized) {
        // 如果已经初始化，执行后续操作
        // ...
    } else {
        // 如果未初始化，可能需要重新初始化或者记录日志
        Log.e("MeetingKit", "Meeting component is not initialized.");
    }
    ```