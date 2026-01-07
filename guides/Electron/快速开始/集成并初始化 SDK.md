本文主要介绍如何集成 NEMeetingKit，并对会议组件进行初始化。

## 开发环境

在客户端实现音视频会议功能之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| Node 版本 | 14 及以上版本 |
| Electron 版本 | 22 及以上版本。<note type=note>如果需要支持 MacOS 系统，会议组件SDK在 Electron 25 及以上版本的存在兼容性问题，可能导致部分功能异常，请使用 24.8.8 或更低版本。</note> |

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在 [网易云信控制台](https://app.yunxin.163.com/global/home) 创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA?platform=console"> 创建应用并获取 AppKey</a>。

2. 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**。具体步骤可参考 [开通方案](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 集成 SDK

集成 [nemeeting-electron-sdk](https://www.npmjs.com/package/nemeeting-electron-sdk) 依赖。

```npm
npm install nemeeting-electron-sdk
```

::: note note
您可根据需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide?platform=electron)。
:::

## 初始化

### 调用时机

在调用 SDK 其他接口之前，您首先需要完成初始化操作。

### 注意事项

- 初始化是一个异步操作，您需要确保异步回调成功之后，再进行调用 API。
- 应用名称会显示在会议界面的顶部标题栏中。若不额外设置应用名称，则标题默认显示为 **会议**。

### 调用步骤

1. 配置初始化相关参数，详情请参考 [`NEMeetingKitConfig`](https://doc.yunxin.163.com/meetingkit/references/web/typedoc/Latest/zh/electron/types/NEMeetingKitConfig.html)。

   **示例代码如下：**

    ```TypeScript
    // 配置初始化相关参数
    const config = {
        appKey: 'your app key',
        appName: 'your app name',
    }
    ```
    
2. 调用 [`initialize`](https://doc.yunxin.163.com/meetingkit/references/web/typedoc/Latest/zh/electron/interfaces/NEMeetingKit.html#initialize) 方法完成初始化操作。该接口无额外回调结果数据。

   **示例代码如下：**

    ```TypeScript
    // 初始化会议组件
    import NEMeetingKit  from 'nemeeting-electron-sdk';

    const neMeetingKit = NEMeetingKit.getInstance();
    neMeetingKit.initialize(config).then((res) => {
        // 初始化成功
    }).catch((error) => {
        // 初始化失败
    });
    ```

3. （可选）当您不确定是否已经初始化会议组件，可调用 [`isInitialized`](https://doc.yunxin.163.com/meetingkit/references/web/typedoc/Latest/zh/electron/interfaces/NEMeetingKit.html#isInitialized) 添加查询是否已经初始化的调用。

   **示例代码如下：**

    ```TypeScript
    // 检查会议组件是否已经初始化
    import NEMeetingKit  from 'nemeeting-electron-sdk';


    const neMeetingKit = NEMeetingKit.getInstance();
    if (neMeetingKit.isInitialized) {
        // 如果已经初始化，执行后续操作
        // ...
    } else {
        // 如果未初始化，可能需要重新初始化
    }
    ```