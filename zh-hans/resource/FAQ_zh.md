

* 调用 startPreview 后，没有出现视频画面，也没有任何错误反馈
  - 检查 camera 对象是否创建成功。现在 TuyaSmartCameraSDK 拆分后，使用 pod 'TuyaSmartCamera' 导入时，默认是不会带有 p2p 1.0 的支持的。所以检查创建摄像头时的 p2pType，如果是 1，请使用 pod 'TuyaSmartCamera/ALL' 导入 SDK。
  - 检查 p2p 连接是否成功。startPreivew 必须在收到 ```cameraDidConnected:``` 回调以后调用。
* 调用 ```- (void)destory```方法后崩溃
  - 在老的版本中，有一个BUG，p2pType 为 1 的时候，调用 destory 方法确实会崩溃。在 3.1.1 以后的版本中已经修复。
  - 请在调用 destory 方法之前，停止预览或者回放操作，并调用 disConnect 断开 p2p 连接。
* 在使用模拟器调试的时候，某些摄像头预览时崩溃。
  - 有些摄像头采用的硬件解码，模拟器不支持，请使用真机调试。