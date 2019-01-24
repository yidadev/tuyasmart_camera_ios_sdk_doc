# 初始化

## TuyaSmartHomeKit 通用接口介绍

在接入TuyaSmartHomeKit，并初始化其SDK后，需要使用其通用接口调用服务端的API接口来获取摄像头的配置参数。

```objective-c
- (void)load {
//self.request = [TuyaSmartRequest alloc] init];

/**
*  请求服务端接口
*
*  @param apiName    接口名称
*  @param postData 业务入参
*  @param version  接口版本号
*  @param success  操作成功回调
*  @param failure  操作失败回调
*/
[self.request requestWithApiName:@"api_name" postData:@"post_data" version:@"api_version" success:^(id result) {

} failure:^(NSError *error) {

}];    
}
```

### 摄像头配置参数接口

* 接口说明

| 名称                  | 版本 | 描述       |
| --------------------- | ---- | ---------- |
| tuya.m.ipc.config.get | 1.0  | 摄像头配置 |


* 请求参数

| 名称  | 类型   | 描述               |
| ----- | ------ | ------------------ |
| devId | 字符串 | 摄像头对应的设备id |

```objective-c
NSDictionary *params = @{
@"devId": @"XXXXXXXX" 
};
```

* 响应参数

| 名称              | 类型   | 描述                          |
| ----------------- | ------ | ----------------------------- |
| result            | 字典   | 返回结果                      |
| result.id         | 字符串 | 设备id                        |
| result.p2pConfig  | 字典   | 扩展配置                      |
| result.p2pId      | 字符串 | p2p连接id                     |
| result.password   | 字符串 | p2p连接密码                   |
| result.timeZoneId | 字符串 | 设备所在时区                  |
| status            | 字符串 | 状态（成功：ok；失败：error） |
| success           | 布尔值 | 是否成功                      |

```json
{
result =     {
password = "xxxxxx",
id = "xxxxxxxxxxxxxxxxxx",
p2pConfig =     {
},
p2pId = "XXXXXXXXXXXXXXXXXXX",
timeZoneId = "Asia/Shanghai",
},
success = 1,
status = "ok",
}
```

## 创建摄像头对象

* 摄像头配置参数类属性介绍

| 名称      | 类型   | 描述              |
| --------- | ------ | ----------------- |
| p2pId     | 字符串 | p2p连接id         |
| password  | 字符串 | p2p连接密码       |
| localKey  | 字符串 | 局域网通讯密钥    |
| p2pConfig | 字典   | 摄像头p2p通道配置 |

* 在创建摄像头的文件中，添加以下代码导入头文件：

```
#import <TuyaSmartCamera/TuyaSmartCameraFactory.h>
```


* 添加以下代码创建配置参数对象并创建摄像头对象：

```objective-c
//self.devId = @"your_device_id";
//self.request = [TuyaSmartRequest alloc] init];
//self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];

//p2p通道类型，SDK会根据p2pType创建不同版本的摄像头对象。
id p2pType = [self.device.deviceModel.skills objectForKey:@"p2pType"];

__weak typeof(self) weakSelf = self;
[self.request requestWithApiName:@"tuya.m.ipc.config.get"   
postData:@{@"devId":self.devId} 
version:@"1.0" 
success:^(id result) {
TuyaSmartCameraConfig *config = [TuyaSmartCameraConfig new];
config.p2pId = [result objectForKey:@"p2pId"];
config.password = [result objectForKey:@"password"];
config.localKey = self.device.deviceModel.localKey;
config.p2pConfig = [result objectForKey:@"p2pConfig"];
weakSelf.camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType 
config:config
delegate:self];
} failure:failure];
```

至此，已经成功创建出了摄像头对象，接下来可以开始使用摄像头的功能了。
