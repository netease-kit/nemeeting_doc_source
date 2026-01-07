本文介绍如何在会议组件中开启 IM 复用功能，帮助您在应用中同时集成并使用 NEMeetingKit 和 IM SDK。

## 背景介绍

IM SDK 是网易云信提供的 IM 即时通讯 SDK，网易会议组件 NEMeetingKit 在底层实现上，依赖了 IM SDK 提供的 IM 长连接通道，用来完成会议中的会控操作。而为了能够建立起长连接通道，需要使用 IM 账号完成 IM SDK 的登录。因此，在 NEMeetingKit 中，一个会议账号都会对应唯一一个 IM 账号。

当用户调用 NEMeetingKit <a href="https://doc.yunxin.163.com/meeting/guide/DM0OTEwMjQ?platform=web" target="_blank">登录接口</a> 传入会议账号进行登录时，NEMeetingKit 内部会查询并获取到该会议账号唯一绑定的 IM 账号，并自动登录 IM SDK 以建立起长连接通道。此时，如果上层业务同时也集成了 IM SDK，并已经进行了 IM SDK 登录操作，那么由于 IM SDK 同时只允许登入一个账号，因此会造成此次 NEMeetingKit 登录失败。

**因此，若您需要在应用中同时集成 NEMeetingKit 和 IM SDK，您需要开启 NEMeetingKit 的 IM 复用功能**。

## 创建会议账号

1. 准备工作。

    - 若您已集成 IM SDK，需要再同时集成 NEMeetingKit，则需要在指定应用下开通 NERoom 房间组件，操作步骤请参考 <a href="https://doc.yunxin.163.com/console/concept/zc3NDYzNzc?platform=console" target="_blank">开通 NERoom 房间组件</a>。

    - 若您已集成 NEMeetingKit，需要再同时集成 IM SDK，则不需要额外配置，当前的 NEMeeting App Key 可以直接作为 IM App Key 使用，用于完成 IM SDK 的登录。

2. <span id="创建一个与 IM 账号绑定的会议账号">创建一个与 IM 账号绑定的会议账号</span>。

    在登录 IM SDK 前，需要先完成 IM 账号的注册，其中每个 IM 账号对应唯一的 imAccid 和 imToken。

    假设当前已有一个 IM 账号，其中 imAccid 和 imToken 值分别为 "myAccid" 和 "myToken"。若现在需要复用该 IM 账号，则需要调用 <a href="https://doc.yunxin.163.com/meeting/server-apis/DU3NTczMTg?platform=server" target="_blank">创建会议账号</a> 的服务端接口创建一个与该 IM 账号绑定的会议账号，请求时需要在请求参数中将 imAccid 和 imToken 分别设置为该 IM 账号的 imAccid 和 imToken，即 "myAccid" 和 "myToken" 。

    至此您就创建了一个与该 IM 账号绑定的会议账号。同时该服务端接口返回会议账号的 userUuid 和 userToken，可用来登录 NEMeetingKit。

## 复用 IM 账号

上一步完成后，则可以在客户端上开启 IM 复用功能，具体步骤如下：

1. 初始化 IM SDK 之前，需要通过会议组件的 `NEMeetingKit.reuseIM()` 方法重新包装 IM SDK 的 `getInstance` 方法，里面会做一些复用的逻辑处理。该方法返回新的 IM SDK。

    ::: note note
    开启复用功能后，NEMeetingKit 不会自动初始化 IM SDK，需要在应用层单独提示进行 IM SDK 初始化。
    :::

    **示例代码**如下：

    ```
    // 先调用NEMeetingKit.reuseIM对IM SDK进行处理
    const NIM = NEMeetingKit.reuseIM(NIM)
    // 用新返回的NIM初始化(仅在复用时需要显示初始化)
    const nim = NIM.getInstance({
        appKey: 'appKey',
        account: 'account',
        token: 'token',
        // 具体参数由应用层根据实际情况传入
        //...
        //...
    });        
    ```

 2. 初始化会议组件的时候把上一步的实例传入 im 字段，即可开启复用功能。
 
    **示例代码**如下：
    ```
    //初始化 NEMeetingKit
    const config = {
        appKey,
        /** 用于设置会议显示区域宽度，(设置为 0 则根据容器自适应) */
        width: 0,
        /** 用于设置会议显示区域高度，(设置为 0 则根据容器自适应) */
        heith: 0,
        // 需要把生成的nim实例传入会议组件sdk
        im: nim, 
        // 其他初始化参数根据实际情况传入
        // ...
        // ...
    }
    const neMeetingKit = NEMeetingKit.getInstance()
    neMeetingKit.initialize(config)
    ```

3. 登录 NEMeetingKit。

    使用对应的会议账号登录 NEMeetingKit，请通过<a href="#创建一个与 IM 账号绑定的会议账号">创建会议账号</a>时返回的 userUuid（对应 accountId） 和 userToken（对应 accountToken） 完成登录，**示例代码**如下：

    ```
    //使用 账号密码 登录
    const neMeetingKit = NEMeetingKit.getInstance()
    const accountService = neMeetingKit.getAccountService()
    // 用户账户
    const userUuid = ''
    // 用户 token
    const token = ''
    accountService.loginByToken(userUuid, token).then(res => {
        // 登录成功
    }).catch(error => {
        // 登录失败
    })
    ```

    ::: note note
    您可以通过此接口回调中返回的错误码判断是否登录成功；若登录成功则说明 IM 复用功能启用成功，否则启用失败。
    :::

## 相关参考

### **复用 IM 前后 NEMeetingKit 的行为差异**

启用或关闭 IM 复用功能时，NEMeetingKit 的行为表现存在不一致的情况，具体请参考如下表格。

| <div style="width:80px">IM 复用功能状态</div>	| <div style="width:50px">初始化</div> 	| <div style="width:30px">登录</div> 	| <div style="width:60px">是否支持匿名入会</div>|
|---	|---	|---	|---	|
| 关闭 	| NEMeetingKit 会自动初始化 IM SDK 	| 会获取会议账号对应的 IM 账号自动登录 IM SDK 	| 支持 	|
| 启用 	| NEMeetingKit 不会自动初始化 IM SDK，由应用层负责初始化 	| 不会自动登录 IM SDK，会判断会议账号与当前已登录的 IM 账号是否是绑定关系，以决定 NEMeetingKit 是否登录成功 	| 不支持 	|

### **错误码**

| 错误码 	| 说明 	| 解决办法 	|
|---	|---	|---	|
| 112001 	| IM 复用不支持匿名入会 	| 选择其他方法入会或者关闭 IM 复用功能 	|
| 100004 	| 开启了 IM 复用，请先登录 IM 	| 确保 IM SDK 已经登录后再登录 NEMeetingKit 	|
| 100005 	| 开启了 IM 复用，IM 账号不匹配 	| 重新创建会议账号，并传入对应需绑定 IM 账号的 imAccid与 imToken 	|