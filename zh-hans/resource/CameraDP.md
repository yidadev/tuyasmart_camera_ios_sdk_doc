## 摄像头设备功能点

**【描述】**

涂鸦智能摄像头设备提供了丰富的功能点，并且通过标准化来实现设备与App之间的交互。设备相关的返回数据都采用异步消息都方式通知APP。

### 摄像头设备标准化功能

摄像头设备功能点是对 Data Point 的一层简单封装，主要的类为：```TuyaSmartCameraDPManager```。功能定义请查看 ```TuyaSmartCameraDPManager.h```.

- 设备基本设置功能

| 功能点 | 数据类型 | value | 描述 | 功能定义 |
| ------ | ------ | :----: | ------ | ------ |
| TuyaSmartCameraBasicIndicatorDPName | BOOL | “YES”为正常状态下开启指示灯，“NO”为正常状态下关闭指示灯 | 状态指示灯 | 开启/关闭设备正常状态下的指示灯，在晚间避免光污染。配网阶段、设备故障等情况下指示灯仍需要正常显示以提示用户 |
| TuyaSmartCameraBasicFlipDPName | BOOL |                 “YES”为翻转，“NO”为正常                 | 画面翻转 | 开启/关闭画面垂直翻转，用于设备倒装时调整画面方向 |
| TuyaSmartCameraBasicOSDDPName | BOOL |              “YES”为打开OSD，“NO”为关闭OSD              | 时间水印 | 开启/关闭直播画面的时间水印 |
| TuyaSmartCameraBasicPrivateDPName | BOOL | “YES”为开启隐私模式，“NO”为关闭隐私模式 | 隐私模式 | 开启/关闭隐私模式，休眠时设备的音视频采集不工作，即设备无法直播也无法保存录像 |
| TuyaSmartCameraBasicNightvisionDPName | enum | 0：自动, 1：关, 2：开 | 红外夜视功能 | 调整红外夜视功能的开/关/自动状态 |
| TuyaSmartCameraBasicPIRDPName | enum | 0：关闭，1：低灵敏度，2：中灵敏度，3：高灵敏度 | PIR开关及灵敏度 | 调整PIR功能的关闭/低灵敏度/中灵敏度/高灵敏度 |

- 移动侦测报警

| 功能点                                 | 数据类型 | value                                           | 描述                   | 功能定义                                                     |
| -------------------------------------- | -------- | ----------------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| TuyaSmartCameraMotionDetectDPName      | BOOL     | “YES”为开启移动侦测报警，“NO”为关闭移动侦测报警 | 移动侦测报警开关       | 开启/关闭移动侦测报警，开启后，设备需要在适当的条件下每次检测到移动均上报消息到服务器 |
| TuyaSmartCameraMotionSensitivityDPName | enum     | 0：低,1：中, 2：高                              | 移动侦测报警灵敏度设置 | 设置移动侦测报警的灵敏度                                     |

- 声音侦测报警

| 功能点                                  | 数据类 型 | value                                           | 描述                   | 功能定义                                                     |
| --------------------------------------- | --------- | ----------------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| TuyaSmartCameraDecibelDetectDPName      | BOOL      | “YES”为打开声音侦测报警，“NO”为关闭声音侦测报警 | 声音侦测报警开关       | 开启/关闭声音侦测报警，开启后，设备需要在适当的条件下每次检测到声音均上报消息到服务器 |
| TuyaSmartCameraDecibelSensitivityDPName | enum      | 0：低灵敏度,1：高灵敏度                         | 声音侦测报警灵敏度设置 | 设置声音侦测报警的灵敏度                                     |

- 存储卡及本地录像管理

| 功能点                                 | 数据类型  | value                                                        | 描述                         | 功能定义                                                     |
| -------------------------------------- | --------- | ------------------------------------------------------------ | ---------------------------- | ------------------------------------------------------------ |
| TuyaSmartCameraSDCardStatusDPName      | enum      | 1：正常,2：异常（SD卡损坏或格式不对）,3：空间不足, 4：正在格式化,5：无SD卡 | 存储卡状态                   | 设备向云端上报当前SD卡的状态，包括SD卡正常、异常、空间不足等 |
| TuyaSmartCameraSDCardStorageDPName     | string    | “总容量\|已使用\|空闲”，单位是 kB。                          | 存储卡容量信息查询           | 上报存储卡当前的容量状态，包括总容量、已使用容量、剩余容量   |
| TuyaSmartCameraSDCardFormatDPName      | BOOL      | 下发“YES”以格式化存储卡                                      | 存储卡格式化                 | 发起格式化存储卡的指令、上报当前存储卡格式化的进度等状态     |
| TuyaSmartCameraSDCardFormatStateDPName | NSInteger | 正数为格式化进度0-100                                        | 当前存储卡格式化的进度等状态 | 正数为格式化进度，100即格式化成功.错误码：-2000：SD卡已在格式化状态，-2001：SD卡格式化异常 |
| TuyaSmartCameraSDCardRecordDPName      | BOOL      | “YES”为打开本地录像，“NO”为关闭本地录像                      | 本地录像开关                 | 开启/关闭SD卡录像，关闭后设备不会将实时录像记录在SD卡        |
| TuyaSmartCameraRecordModeDPName        | enum      | 1：事件录像（检测到移动再录像到SD卡）2：连续录像             | 本地录像模式选择             | 在本地录像开启的状态下，选择录像的模式，支持“事件录像”和“连续录像” |

- PTZ(云台方向控制)功能

| 功能点                          | 数据类型 | value                   | 描述             | 功能定义                             |
| ------------------------------- | -------- | ----------------------- | ---------------- | ------------------------------------ |
| TuyaSmartCameraPTZControlDPName | enum     | 0：上,2：右,4：下,6：左 | 云台方向控制     | 用于控制云台开始转动以及转动的方向。 |
| TuyaSmartCameraPTZStopDPName    | BOOL     | “YES”(云台停止转动命令) | 控制云台停止转动 | 云台停止转动命令                     |

- 电池供电产品功能

| 功能点                                   | 数据类型  | value                                             | 描述               | 功能定义                                                     |
| ---------------------------------------- | --------- | ------------------------------------------------- | ------------------ | ------------------------------------------------------------ |
| TuyaSmartCameraWirelessElectricityDPName | NSInteger | 设备电量，上报0-100的整数                         | 电池及设备状态信息 | 设备主动上报当前电量，电量发生变化时上报，同时设备及时通知云端当前休眠/唤醒状态 |
| TuyaSmartCameraWirelessLowpowerDPName    | NSInteger | 0-100的整数                                       | 低电量报警阈值     | 用户可以在App上设置阈值，报警规则为dp145≤dp147则报警         |
| TuyaSmartCameraWirelessBatteryLockDPName | BOOL      | “YES”为上锁，“NO”为解锁                           | 电池锁             | 控制打开/关闭电池锁                                          |
| TuyaSmartCameraWirelessPowerModeDPName   | enum      | 0：电池供电,1：插电供电                           | 设备供电方式查看   | 设备主动上报当前供电状态，供电状态发生变化时上报             |
| TuyaSmartCameraWirelessAwakeDPName       | BOOL      | “YES”表示已唤醒，可以直接连接。“NO”表示设备在休眠 | 设备唤醒状态       | 设备主动上报当前是否处于唤醒状态。设备休眠前上报“NO”，设备唤醒后上报一次“YES” |

### 设备标准化功能方法调用

#### 获取对象

```TuyaSmartCameraDPManager```类提供与设备信息通信的能力，提供了控制指令下发、获取当前功能点的数据。

```objective-c
//根据设备id初始化设备控制类
TuyaSmartCameraDPManager *dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:devId];
```



#### 监听代理

 ```TuyaSmartCameraDPObserver```协议提供设备主动上报DP变化的监听能力。

```objective-c
// dpsData 为设备上报的DP。通过 [dpsData objectForKey:TuyaSmartCameraDPKey]; 获取对应的DP功能的值。
- (void)cameraDPDidUpdate:(TuyaSmartCameraDPManager *)manager dps:(NSDictionary *)dpsData;
```



#### 功能点是否支持

**【描述】**

通过该方法可以判断设备是否支持该功能点，如果设备不支持该功能，调用**数据下发**和**数据查询**的方法是会失败的，因此做数据下发和查询之前要先判断该功能是否支持。**入参**为功能点的ID

```objective-c
- (BOOL)isSupportDP:(TuyaSmartCameraDPKey)dpName;
```

**【方法调用】**

```objective-c
BOOL isSupportDpBasicFlip = [self.dpManager isSupportDP:TuyaSmartCameraBasicFlipDPName];
```

#### 数据下发

**【描述】**

通过局域网或者云端这两种方式发送指令，接口是同一个，SDK 内部会自动根据网络状态选择下发方式。

**【方法调用】**

```objective-c
// 下发数据为 id 类型，即如果是 int 或者 BOOL 类型的功能，则需要包装成 NSNumber
[self.dpManager setValue:@(YES) forDP:TuyaSmartCameraBasicFlipDPName success:^(id result) {
        // 下发成功，result 为下发后，设备返回的值，通常与下发的数据是一样的
} failure:^(NSError *error) {
        // 下发失败
}];
```

#### 数据查询

**【描述】**

通过缓存获取对应功能点的数据，存储卡状态，容量，格式化进度三个DP点则需要下发命令到设备查询。

```objective-c
// 通过缓存取DP数据。大多数DP点都可以通过缓存来获取，设备会主动上报。
- (id)valueFortDP:(TuyaSmartCameraDPKey)dpName;
// 下发查询命令
- (void)valueForDP:(TuyaSmartCameraDPKey)dpName success:(TYSuccessID)success failure:(TYFailureError)failure;
```

**【方法调用】**

```objective-c
TuyaSmartCameraRecordMode mode = [self.dpManager valueForDP:TuyaSmartCameraRecordModeDPName];

[self.dpManager valueForDP:TuyaSmartCameraSDCardStorageDPName success:^(id result) {
	// 查询成功，result 为相应DP的值。
} failure:^(NSError *error) {
    // 查询失败
}];
```



### 其他

在开头功能表中的enum类型，实则是字符串类型的枚举，在```TuyaSmartCameraDPManager.h```头文件中定义了相应的常量，方便使用，有以下几个类型：

```objective-c
typedef NSString * TuyaSmartCameraNightvision NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCaemraPIR NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraMotion NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraDecibel NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraRecordMode NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraPTZDirection NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraPowerMode NS_EXTENSIBLE_STRING_ENUM;
```

**方法调用**

```objective-c
/// nightvision状态有三个值，表示自动，关闭，打开.
/// TuyaSmartCameraNightvisionAuto,TuyaSmartCameraNightvisionOff,TuyaSmartCameraNightvisionOn
[self.dpManager setValue:TuyaSmartCameraNightvisionOn forDP:TuyaSmartCameraBasicNightvisionDPName success:^(id result) {
        
} failure:^(NSError *error) {
        
}];
```

**存储卡**

设备如果支持存储卡，则可以获取存储卡的状态，以及格式化存储卡。

```objective-c
// 存储卡状态
typedef NS_ENUM(NSUInteger, TuyaSmartCameraSDCardStatus) {
    TuyaSmartCameraSDCardStatusNormal = 1,			// 存储卡正常使用中
    TuyaSmartCameraSDCardStatusException = 2,		// 存储卡异常，无法获取信息
    TuyaSmartCameraSDCardStatusMemoryLow = 3,       // 存储卡容量较低
    TuyaSmartCameraSDCardStatusFormatting = 4,	    // 存储卡正在格式化
    TuyaSmartCameraSDCardStatusNone = 5             // 未检测到存储卡
};

// 获取存储卡状态
TuyaSmartCameraSDCardStatus status = [[self.dpManager valueForDP:TuyaSmartCameraSDCardStatusDPName] integerValue];

// 获取存储卡容量
[self.dpManager valueForDP:TuyaSmartCameraSDCardStorageDPName success:^(id result) {
	// result 是字符串，总容量，已使用，空闲三个数值以‘|’隔开，单位是kB。
    NSArray *components = [result componentsSeparatedByString:@"|"];
	if (components.count < 3) {
		// exception
		return;
	}
	NSString *total = [components objectAtIndex:0];
	NSString *used = [components objectAtIndex:1];
	NSString *free = [components objectAtIndex:2];
    // other code
} failure:^(NSError *error) {
	// failed
}];

// 格式化存储卡
[self.dpManager setValue:@(YES) forDP:TuyaSmartCameraSDCardFormatDPName success:^(id result) {
	// 存储卡正在格式化
} failure:^(NSError *error) {
	// failed
}];

// 获取存储卡格式化进度
[self.dpManager setValue:@(YES) forDP:TuyaSmartCameraSDCardFormatStateDPName success:^(id result) {
	// 存储卡格式化进度
	NSInteger progress = [result integerValue];
} failure:^(NSError *error) {
	// failed
}];
```

老版本的固件，在格式化存储卡的过程中，需要APP主动去查询格式化进度。新版本的设备固件，会自动上报存储卡的格式化进度，可以通过```TuyaSmartCameraDPObserver``` 的代理方法监听格式化进度，不用主动获取。

