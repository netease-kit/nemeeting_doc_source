本文介绍如何在会议组件中开启 IM 复用功能，帮助您在应用中同时集成并使用 NEMeetingKit 和 IM SDK。

## 背景介绍

IM SDK 是网易云信提供的 IM 即时通讯 SDK，网易会议组件 NEMeetingKit 在底层实现上，依赖了 IM SDK 提供的 IM 长连接通道，用来完成会议中的会控操作。而为了能够建立起长连接通道，需要使用 IM 账号完成 IM SDK 的登录。因此，在 NEMeetingKit 中，一个会议账号都会对应唯一一个 IM 账号。

当用户调用 NEMeetingKit <a href="https://doc.yunxin.163.com/meeting/guide/TQxODU5Mjg?platform=android#%E7%99%BB%E5%BD%95%E9%89%B4%E6%9D%83" target="_blank">登录接口</a>传入会议账号进行登录时，NEMeetingKit 内部会查询并获取到该会议账号唯一绑定的 IM 账号，并自动登录 IM SDK 以建立起长连接通道。此时，如果上层业务同时也集成了 IM SDK，并已经进行了 IM SDK 登录操作，那么由于 IM SDK 同时只允许登入一个账号，因此会造成此次 NEMeetingKit 登录失败。

:::note note 
- 网易会议组件 NEMeetingKit 自 3.13.0 版本起，在登录时，会自动检测 IM SDK 是否已经登录。如果检测到此时 IM 已经登录，则会自动复用。如果检测到未登录，则不复用。用户无需手动开启 IM 的复用功能。
- 如果您使用的会议组件是 3.13.0 之前的版本，且需要在应用中同时集成 NEMeetingKit 和 IM SDK，那么您需要手动开启 NEMeetingKit 的 IM 复用功能。
:::

## 创建会议账号

1. 准备工作。

    * 若您已集成 IM SDK，需要再同时集成 NEMeetingKit，则需要在指定应用下开通 NERoom 房间组件，操作步骤请参考<a href="https://doc.yunxin.163.com/console/concept/zc3NDYzNzc?platform=console" target="_blank">开通 NERoom 房间组件</a>。

    * 若您已集成 NEMeetingKit，需要再同时集成 IM SDK，则不需要额外配置，当前的 NEMeeting App Key 可以直接作为 IM App Key 使用，用于完成 IM SDK 的登录。

2. <span id="创建一个与 IM 账号绑定的会议账号">创建一个与 IM 账号绑定的会议账号</span>。

    在登录 IM SDK 前，需要先完成 IM 账号的注册，其中每个 IM 账号对应唯一的 imAccid 和 imToken。

    假设当前已有一个 IM 账号，其中 imAccid 和 imToken 值分别为 "myAccid" 和 "myToken"。若现在需要复用该 IM 账号，则需要调用 <a href="https://doc.yunxin.163.com/meeting/server-apis/jU0MDAzNTg?platform=server" target="_blank">创建会议账号</a> 的服务端接口创建一个与该 IM 账号绑定的会议账号，请求时需要在请求参数中将 imAccid 和 imToken 分别设置为该 IM 账号的 imAccid 和 imToken，即 "myAccid" 和 "myToken" 。

    至此您就创建了一个与该 IM 账号绑定的会议账号。同时该服务端接口返回会议账号的 userUuid 和 userToken，可用来登录 NEMeetingKit。

## 复用 IM 账号

上一步完成后，则可以在客户端上开启 IM 复用功能，具体步骤如下：

1. 初始化 IM SDK 与 NEMeetingKit。
    
    1. 在 `Application` 中初始化 IM SDK。

        ::: note note
        开启 IM 复用功能后，NEMeetingKit 不会自动初始化 IM SDK，上层业务需要负责完成 IM SDK 的初始化工作。
        :::

    2. 初始化 NEMeetingKit。
    
        ::: note note
        如果您使用的是 3.13.0 之前的会议组件版本，则此时需要设置 `NEMeetingKitConfig#reuseIM` 字段为 `true` 手动开启 IM 复用功能。3.13.0 之后的版本无此参数，即 NEMeetingKit 会自动检测 IM SDK 是否已登录，登录则复用，未登录则不复用。
        :::

    **示例代码**如下：

    ```
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

        //1. 初始化 IM SDK (仅在复用时需要显示初始化)    
        NSString *appKey        = @"your app key";
        NIMSDKOption *option    = [NIMSDKOption optionWithAppKey:appKey];
        // 具体参数由应用层根据实际情况传入
        //...
        //...
        [[NIMSDK sharedSDK] registerWithOption:option];
    
        
        //2. 初始化 NEMeetingKit
        NEMeetingKitConfig *config = [[NEMeetingKitConfig alloc] init];
        config.appKey = @"your app key"; //应用 App Key
        // 打开 IM 复用开关
        config.reuseIM = YES;
        // 其他初始化参数根据实际情况传入
        // ...
        // ...
        [[NEMeetingKit getInstance] initialize:config
                            callback:^(NSInteger resultCode, NSString *resultMsg, id result) 
        {
        if (resultCode == ERROR_CODE_SUCCESS) {
            //初始化成功
            } else {
            //初始化失败
            }
        }];
    }
    ```

2. 登录 IM SDK。

    开启复用功能时，应用层需要确保已先使用与该会议账号绑定的 IM 账号登录 IM SDK。登录时机由业务决定，只需要确保在进行 NEMeetingKit 登录前完成 IM 登录即可。

    请通过 IM 账号的 imAccid 和 imToken 完成登录，**示例代码**如下：

    ```
    NSString *account = @"your account";
    NSString *token   = @"your token";
    [[[NIMSDK sharedSDK] loginManager] login:account
                                       token:token
                                    completion:^(NSError *error) {}];
    ```

3. 登录 NEMeetingKit。

    使用对应的会议账号登录 NEMeetingKit，请通过<a href="#创建一个与 IM 账号绑定的会议账号">创建会议账号</a>时返回的 userUuid 和 userToken 完成登录，**示例代码**如下：

    ```
    //以使用账号 ID 和 Token 登录为例
    [[NEMeetingKit getInstance] login:accountId
                                token:accountToken
                            callback:^(NSInteger resultCode, NSString *resultMsg, id result) {
        if (resultCode == ERROR_CODE_SUCCESS) {
            //登录成功
        } else {
    //登录失败
        }
    }];
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