本文主要介绍如何快速跑通网易会议组件的 [Android Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/Android)，体验在线的多人视频会议功能。

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
| JDK 版本 | 17 及以上版本 |
| Android API 版本 | Android SDK API 等级 21 及以上、Android 5.0 及以上版本 |
| CPU 架构 | 支持 ARM64、ARMV7 架构 |
| IDE | Android Studio |
| 其他 | 依赖 Androidx，不支持 support 库 |

## 前提条件

- 在网易云信控制台创建至少一个应用，获取应用的 AppKey。若无应用，请参考 [创建应用并获取 AppKey](https://doc.yunxin.163.com/console/concept/TIzMDE4NTA)。

- 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**，具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 下载 Sample Code

1. 从 github 下载网易会议组件 Sample Code 源码，或者直接在命令行运行以下命令：

    ```
    git clone https://github.com/netease-kit/NEMeeting.git
    ```

2. 通过 Android Studio 打开 `NEMeeting/SampleCode/Android` 项目。

## 配置 Sample Code

### 配置 SDK

修改工程目录下的 `app/build.gradle` 文件，添加网易会议组件的依赖。

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
您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide/zk0MjM1OTg?platform=android)。
:::

### 配置 AppKey

打开工程，在工程的 `NEMeeting/SampleCode/Android/app/src/main/res/values/appkey.xml` 文件中填写您的会议组件 AppKey。

```XML
<?xml version="1.0" encoding="utf-8"?>
  <resources>
    <!--TODO-->
    <!--Replace With Your AppKey Here-->
    <string name="appkey">Your AppKey</string>
</resources>
```

## 运行 Sample Code

您在申请并声明 AppKey 后，运行并编译示例项目，即可体验 **登录**、**预约会议**、**加入会议**、**创建会议** 功能。

::: note note
请使用 Android 真机设备进行调试和运行，暂不支持模拟器设备。
:::

效果图如下所示：

<img style="width:30%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/b6cb6b4600a32d1bdb36c8a8017042aa/meetingkit_demo_main.png" alt="meetingkit_demo_main" />