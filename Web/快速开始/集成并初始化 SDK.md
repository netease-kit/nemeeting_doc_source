本文主要介绍如何集成 NEMeetingKit，并对会议组件进行初始化。

## 开发环境

Web NEMeetingKit 接入不区分开发框架，但在客户端实现音视频会议功能之前，请您准备以下浏览器环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| Chrome | 74 及以上版本 |
| Safari | 12 及以上版本 |
| Node | 14 及以上版本 |

## 注意事项

- 请使用 HTTPS 协议访问网易会议组件的 Domain。
- 不建议通过 require 方式引入网易会议组件，可能存在选择设备无效等问题。

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在 [网易云信控制台](https://app.yunxin.163.com/global/home) 创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA">创建应用并获取 AppKey</a>。
2. 开通 **视频会议** 解决方案或者开通 **PaaS 会议组件服务**。具体步骤可参考 [开通方案](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 集成 SDK

下载 [网易会议组件](https://yx-nosdn.chatnos.com/package/NEMeetingKit_v4.18.0.js?download=NEMeetingKit_v4.18.0.js) 到本地。

::: note note
您可根据业务需要自行选择组件版本号，建议使用最新版本，详情可查看会议组件 [更新日志](https://doc.yunxin.163.com/meeting/guide/zEwOTYyODc?platform=web)。
:::

### 通过 script 标签引入

1. 在本地文件夹中创建网易会议组件的项目文件夹，并将下载并解压后的网易会议组件拷贝到项目文件夹中。文件目录如下：

    ```Bash
    ├── index.html
    ├── NEMeetingKit_V4.x.x.js
    ```

2. 将以下代码添加到 `index.html` 页面的 body 底部中。

    ```JavaScript
    <script src="./NEMeetingKit_V4.x.x.js"></script>//将文件路径替换为网易会议组件在本地的相对路径，版本号请替换为对应的版本号
    ```

3. 在页面添加 ID 为 `ne-web-meeting` 的元素（用于挂载网易会议组件）。

    ```HTML
    <div id="ne-web-meeting"></div>
    ```

    ::: note note
    该 ID 为固定值，请不要修改。
    :::

    **示例代码如下：**

    ```HTML
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
    <div id="ne-web-meeting"></div>
    <script src="./NEMeetingKit_v4.x.x.js"></script> //将文件路径替换为网易会议组件在本地的相对路径，版本号请替换为对应的版本号
    <script>
        const neMeetingKit = NEMeetingKit.default.getInstance()
    </script>
    </body>
    </html>
    ```

### 通过模块化引入

Web 应用可通过 esmodule 方式引入网易会议组件。

1. 将网易会议组件拷贝到项目文件夹中。文件目录如下：

    ```Bash
    ├── index.js
    ├── NEMeetingKit_V4.x.x.js
    ```

2. 将以下代码添加到 `index.js` 中。

    **示例代码如下：**

    ```JavaScript
    import NEMeetingKit from './NEMeetingKit_V4.x.x.js' //将文件路径替换为网易会议组件在本地的相对路径，版本号请替换为对应的版本号
    const neMeetingKit = NEMeetingKit.getInstance()
    const appKey = ''
    neMeetingKit.initialize({
        appKey,
        /** 用于设置会议显示区域宽度，(设置为 0 则根据容器自适应) */
        width: 0,
        /** 用于设置会议显示区域高度，(设置为 0 则根据容器自适应) */
        heith: 0,
    })
    ```

## 初始化

### 调用时机

在调用 SDK 其他接口之前，您首先需要完成初始化操作。

### 注意事项

- 初始化是一个异步操作，您需要确保异步回调成功之后，再进行调用 API。
- 应用名称会显示在会议界面的顶部标题栏中。若不额外设置应用名称，则标题默认显示为 **会议**。

### 调用步骤

1. 配置初始化相关参数，详情请参考 [`NEMeetingKitConfig`](https://doc.yunxin.163.com/meetingkit/references/web/typedoc/Latest/zh/types/NEMeetingKitConfig.html)。

   **示例代码如下：**

    ```JavaScript
    import NEMeetingKit from './NEMeetingKit_V4.x.x.js' //将文件路径替换为网易会议组件在本地的相对路径，版本号请替换为对应的版本号
    const neMeetingKit = NEMeetingKit.getInstance()
    const appKey = ''
    const config = {
        appKey,
        /** 用于设置会议显示区域宽度，(设置为 0 则根据容器自适应) */
        width: 0,
        /** 用于设置会议显示区域高度，(设置为 0 则根据容器自适应) */
        heith: 0,
    }
    ```
    
2. 调用 [`initialize`](https://doc.yunxin.163.com/meetingkit/references/web/typedoc/Latest/zh/interfaces/NEMeetingKit.html#initialize) 方法完成初始化操作。该接口无额外回调结果数据。

   **示例代码如下：**

    ```JavaScript
    neMeetingKit.initialize(config).then(() => {
        // 初始化成功
    }).catch((error) => {
        // 初始化失败
    })
    ```
    
3. （可选）当您不确定是否已经初始化会议组件，可调用 [`isInitialized`](https://doc.yunxin.163.com/meetingkit/references/web/typedoc/Latest/zh/interfaces/NEMeetingKit.html#isInitialized) 添加查询是否已经初始化的调用。

   **示例代码如下：**

    ```JavaScript
    // 检查会议组件是否已经初始化
    const isInitialized = NEMeetingKit.getInstance().isInitialized;
    if (isInitialized) {
        // 如果已经初始化，执行后续操作
        // ...
    } else {
        // 如果未初始化，可能需要重新初始化或者记录日志
       
    }
    ```