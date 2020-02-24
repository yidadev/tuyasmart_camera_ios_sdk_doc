## 视频直播

在创建后 camera 对象后，就可以开始播放实时视频了。这里会介绍到相关的接口，接口详细描述可以在 `TuyaSmartCameraType.h`头文件中查看接口的注释，或者查看`TuyaSmartCameraBase`模块的接口文档，所有 camera 操作的结果将通过`TuyaSmartCameraDelegate`的代理方法反馈。

### P2P 连接

在开始播放视频之前，需要先连接 p2p 通道。p2p 的连接状态需要开发者自己维护，SDK 只负责下发命令和接收响应结果。p2p 通道连接相关的接口如下：

```objc
#pragma mark - TuyaSmartCameraType
/**
 开始连接 p2p 通道
 */
- (void)connect;
/**
 断开 p2p 通道
 */
- (void)disConnect;

#pragma mark - TuyaSmartCameraDelegate
/**
 camera 的 p2p 通道已连接成功
 */
- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera;

/**
 camera 的 p2p 通道已断开
 */
- (void)cameraDisconnected:(id<TuyaSmartCameraType>)camera;

```

> connect 方法可能会阻塞线程，建议在子线程调用。

### 实时视频播放

p2p 通道连接成功以后，就可以开始播放实时视频了，相关的接口如下：

```objc
#pragma mark - TuyaSmartCameraType
/**
 开始视频实时预览
 */
- (void)startPreview;

/**
 停止视频实时预览
 */
- (void)stopPreview;

/**
 视频帧渲染视图
 */
- (UIView<TuyaSmartVideoViewType> *)videoView;

#pragma mark - TuyaSmartCameraDelegate
/**
 camera 成功开始播放实时视频
 */
- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera;

/**
 camera 停止实时视频播放
 */
- (void)cameraDidStopPreview:(id<TuyaSmartCameraType>)camera;

```

### 视频渲染

视频成功播放后，SDK 收到视频流将会自动渲染，开发者可以通过 camera 对象的 `- (UIView<TuyaSmartVideoViewType> *)videoView` 方法来获取渲染视图，将其添加到屏幕上，并设置布局属性。

视频渲染视图提供下面几个属性和方法：

```objc
/**
是否缩放图像已铺满整个视图，默认是 NO ，如果视图的宽高比和图像的宽高比不一样，则会在图像的上下或者左右两侧留有黑边
 */
@property (nonatomic, assign) BOOL scaleToFill;

/**
设置图像缩放比例
 */
- (void)tuya_setScaled:(float)scaled;

/**
设置图像偏移量
 */
- (void)tuya_setOffset:(CGPoint)offset;

/**
清除当前的图像，会显示黑色
 */
- (void)tuya_clear;

/**
对镶嵌渲染的图像截图
 */
- (UIImage *)screenshot;
```

> TuyaSmartCameraType 对象还提供了一个 autoRender 属性，默认是 YES，如果开发者不希望 SDK 自动渲染视频，可将此属性设置为 NO，然后可以从代理方法中获取到每一帧视频的 YUV 数据，并自主开发视频渲染，详细的接口在“裸流数据”章节中介绍。

#### 全屏播放

SDK 并不提供全屏播放的能力，全屏播放只需要修改视频渲染视图的布局即可，若需要视频图像铺满视图，可以将`scaleToFill`属性设置为 YES，但是此时图像可能会被拉伸变形。全屏模式时，横竖屏的旋转，布局的变化都需要开发者自行维护。

### 流程图

```flow
st=>start: 初始化Camera
conn=>operation: 连接p2p
isconned=>condition: 是否已经连接？
success=>condition: 是否成功？
error=>operation: 错误提示
play=>operation: 开始直播
playSuccess=>condition: 直播成功？
stop=>operation: 停止直播
e=>end: 断开链接
st->isconned
isconned(yes)->play
isconned(no)->conn->success
success(yes)->play
success(no)->error
play->playSuccess
playSuccess(no)->error
playSuccess(yes)->stop
stop->e
```

### 错误回调

摄像头操作的所有错误回调都将通过下面的代理方法反馈：

```objc
/**
 camera 操作失败回调

 @param camera 发生错误的 camera
 @param errStepCode 发生错误的操作，具体参考 TYCameraErrorCode 
 @param errorCode 错误原因，具体参考TuyaCameraSDK.framework/TYDefines
 */
- (void)camera:(id<TuyaSmartCameraType>)camera didOccurredErrorAtStep:(TYCameraErrorCode)errStepCode specificErrorCode:(NSInteger)errorCode;
```

`errStepCode`参数表明什么操作失败了，定义于`TuyaSmartCameraDefines.h`中的 `TYCameraErrorCode`枚举，errorCode 是错误，表示具体的错误原因，定义于`TuyaCameraSDK.framework/TYDefines.h`中的`TuyaErrCode`枚举。例如`TY_ERROR_CONNECT_FAILED`表示 p2p 连接失败，`kTuyaErrCode_ParamsInvalid`表示无效的参数。

### 示例代码

更加完整的代码可以参考 Demo 程序中`TuyaSmartCamera.m`类中对涂鸦 IPC SDK 的二次封装代码。

__Objective-C__

```objc
#define kTuyaSmartIPCConfigAPI @"tuya.m.ipc.config.get"
#define kTuyaSmartIPCConfigAPIVersion @"2.0"

- (void)startStream {
    if (self.connected) {
        [self.camera startPreview];
        return;
    }
    id p2pType = [self.deviceModel.skills objectForKey:@"p2pType"];
    [[TuyaSmartRequest new] requestWithApiName:kTuyaSmartIPCConfigAPI postData:@{@"devId": self.devId} version:kTuyaSmartIPCConfigAPIVersion success:^(id result) {
        dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
            TuyaSmartCameraConfig *config = [TuyaSmartCameraFactory ipcConfigWithUid:[TuyaSmartUser sharedInstance].uid localKey:self.deviceModel.localKey configData:result];
            self.camera = [TuyaSmartCameraFactory cameraWithP2PType:p2pType config:config delegate:self];
            [self.camera connect];
        });
    } failure:^(NSError *error) {
				// 获取配置信息失败
    }];
}

#pragma mark - TuyaSmartCameraDelegate

- (void)cameraDidConnected:(id<TuyaSmartCameraType>)camera {
    self.connected = YES;
  	// 需要 p2p 连接成功后再开始预览
		[camera startPreview];
}

- (void)cameraDisconnected:(id<TuyaSmartCameraType>)camera {
  	// p2p 连接被动断开，一般为网络波动导致
    self.connected = NO;
    self.previewing = NO;
}

- (void)cameraDidBeginPreview:(id<TuyaSmartCameraType>)camera {
		// 实时视频开始播放
    self.previewing = YES;
  	// 将视频渲染视图添加到屏幕上
		[self.view addSubview:camera.videoView];
}

- (void)cameraDidStopPreview:(id<TuyaSmartCameraType>)camera {
		// 实时视频停止播放
    self.previewing = NO;
}

// 错误回调
- (void)camera:(id<TuyaSmartCameraType>)camera didOccurredErrorAtStep:(TYCameraErrorCode)errStepCode specificErrorCode:(NSInteger)errorCode {
		if (errStepCode == TY_ERROR_CONNECT_FAILED) {
      	// p2p 连接失败
        self.connected = NO;
    }
    else if (errStepCode == TY_ERROR_START_PREVIEW_FAILED) {
      	// 实时视频播放失败
        self.previewing = NO;
    }
}
```

__Swift__

```swift
func startStream() {
    if self.isConnected {
        self.camera.startPreview()
        return
    }
    let p2pType = self.deviceModel.skills["p2pType"]!
    TuyaSmartRequest().request(withApiName: kTuyaSmartIPCConfigAPI, postData: ["devId": self.devId], version: kTuyaSmartIPCConfigAPIVersion, success: { result in
        guard let responder = result as? [AnyHashable:Any] else {
            return;
        }
        DispatchQueue.global().async {
            let config = TuyaSmartCameraFactory.ipcConfig(withUid: TuyaSmartUser.sharedInstance().uid, localKey: self.deviceModel.localKey, configData: responder)
            self.camera = TuyaSmartCameraFactory.camera(withP2PType: p2pType, config: config, delegate: self)
            self.camera.connect()
        }
    }) { _ in
        // 获取配置信息失败
    }
}

func cameraDidConnected(_ camera: TuyaSmartCameraType!) {
    self.isConnected = true
    // 需要 p2p 连接成功后再开始预览
    camera.startPreview()
}

func cameraDisconnected(_ camera: TuyaSmartCameraType!) {
    // p2p 连接被动断开，一般为网络波动导致
    self.isConnected = false
    self.isPreviewing = false
}

func cameraDidBeginPreview(_ camera: TuyaSmartCameraType!) {
    // 实时视频开始播放
    self.isPreviewing = true;
    // 将视频渲染视图添加到屏幕上
    self.view.addSubview(camera.videoView())
}

func cameraDidStopPreview(_ camera: TuyaSmartCameraType!) {
    // 实时视频停止播放
    self.isPreviewing = false
}

func camera(_ camera: TuyaSmartCameraType!, didOccurredErrorAtStep errStepCode: TYCameraErrorCode, specificErrorCode errorCode: Int) {
    if errStepCode == TY_ERROR_CONNECT_FAILED  {
        // p2p 连接失败
        self.isConnected = false
    }else if errStepCode == TY_ERROR_START_PREVIEW_FAILED {
        // 实时视频播放失败
        self.isPreviewing = false
    }
}
```

> App 进入后台的时候，要需要停止视频播放，视频数据使用硬件解码，OpenGL 渲染，在后台的时候，继续播放，可能会造成 App 崩溃。另外，涂鸦智能摄像机一般最多支持同时连接 5 路 p2p 通道（即支持 5 个手机同时连接），建议 App 在后台停留一段时间后，主动断开 p2p 连接，以释放资源。



