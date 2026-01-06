本文主要介绍如何快速跑通网易会议组件的 [H5 Sample Code](https://github.com/netease-kit/NEMeeting/tree/main/SampleCode/H5)，体验在线的多人视频会议功能。

该示例项目包含了详细的 API 调用场景、参数封装以及回调处理。包含的功能如下：

- 通过账号、密码完成会议组件的登录鉴权、注销登录。
- 预约会议、创建会议、加入会议。
- 会议内提供的其他功能（如会议控制、屏幕共享等）。

## 开发环境

在开始运行示例项目之前，请您准备以下开发环境：

| 环境类型 | 具体要求 |
| ---- | ---- |
| 移动版 Chrome | 74 及以上版本 |
| 移动版 Safari | 13 及以上版本 <note type="notice">13.0 ～ 14.3 的版本仅支持播放音视频，不支持发送。</note> |
| Node | 14 及以上版本 |

## 前提条件

在根据本文操作前，请确保您已在网易云信控制台上，完成以下设置：

1. 在控制台创建至少一个应用。若无应用，请参考 <a href="https://doc.yunxin.163.com/console/concept/TIzMDE4NTA">创建应用并获取 AppKey</a>。
2. 开通 **视频会议** 解决方案。具体步骤可参考 [方案开通](https://doc.yunxin.163.com/meeting/concept/TkzMjExNDY?platform=client)。

## 下载 Sample Code

1. 从 github 下载网易会议组件 Sample Code 源码，或者直接在命令行运行以下命令：

    ```
    git clone https://github.com/netease-kit/NEMeeting.git
    ```

2. 通过 VSCode 打开 `NEMeeting/SampleCode/H5` 项目。

## 配置 Sample Code

1. 配置示例项目。具体步骤如下。

    1. 进入项目 `SampleCode/H5` 目录，运行以下命令进行依赖安装。

        ```NPM
        npm install
        ```

    2. 依赖安装完成后，编辑 `index.html` 页面。
    
        - 在 `init` 方法中填入您在 [网易云信控制台](https://app.yunxin.163.com/global/home) 上创建应用时获取的密钥（AppKey）。

            ```JavaScript
            function init() {
                const neMeetingKit = NEMeetingKit.default.getInstance()
                const appKey = '' // 此处填入对应appkey
                neMeetingKit.initialize({
                    appKey,
                    width: 0,
                    height: 0,
                }).then(() => {
                    const meetingService = neMeetingKit.getMeetingService()

                    meetingService.addMeetingStatusListener({
                        onMeetingStatusChanged: function ({status}){
                        // status: 会议状态
                        }
                    })
                })

            }
            ```

        - 在 `login` 方法中写入对应账号 ID（`accountId`）和账号 Token（`accountToken`）。有关账号 Token 的获取，请参考 [创建会议账号](https://doc.yunxin.163.com/meeting/server-apis/jU0MDAzNTg?platform=server)。

            ```JavaScript
            function login() {
                const neMeetingKit = NEMeetingKit.default.getInstance()
                const accountService = neMeetingKit.getAccountService()
                // 此处填入用户账户
                const userUuid = ''
                // 此处提填入用户 token
                const token = ''
                accountService.loginByToken(userUuid, token)
            }
            ```

## 运行 Sample Code

1. 在项目根目录运行以下命令，启动静态资源服务。

    ```npm
    node app
    ```

2. 服务启动后在浏览器窗口地址输入：`http://localhost:3001/index.html`，依次单击 **初始化** > **登录** > **加入会议** 按钮即可进入会议主页。

    效果图如下所示：

    <img style="width:30%;border: 1px solid #BFBFBF;" src="https://yx-web-nosdn.netease.im/common/eeddbb8e819a7b7790ac52e0f2e69661/meeting_h5_demo.jpg" alt="image" />
