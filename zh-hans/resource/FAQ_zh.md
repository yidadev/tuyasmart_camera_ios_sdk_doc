

1. 调用 startPreview 后，没有出现视频画面，也没有任何错误反馈
   * 检查 camera 对象是否创建成功。现在 TuyaSmartCameraSDK 拆分后，使用 pod 'TuyaSmartCamera' 导入时，默认是不会带有 p2p 1.0 的支持的。所以检查创建摄像头时的 p2pType，如果是 1，请使用 pod 'TuyaSmartCamera/ALL' 导入 SDK。
   * 检查 p2p 连接是否成功。startPreivew 必须在收到 ```cameraDidConnected:``` 回调以后调用。
2. 调用 ```- (void)destory```方法后崩溃
   * 在老的版本中，有一个BUG，p2pType 为 1 的时候，调用 destory 方法确实会崩溃。在 3.1.1 以后的版本中已经修复。
   * 请在调用 destory 方法之前，停止预览或者回放操作，并调用 disConnect 断开 p2p 连接。
3. 在使用模拟器调试的时候，画面绿屏，或者某些摄像头预览时崩溃。
   * 有些摄像头采用的硬件解码，模拟器不支持，请使用真机调试。
4. 音视频不同步
   * 可以先让设备厂商确认一下，音频帧的时间戳和视频帧的时间戳是否一样。
5. 存储卡录像回放时，播放失败
   * 请确认在开始播放前有请求获取当天的存储卡录像视频片段。
   * 请检查参数是否正确，playTime 需要大于等于 startTime，小于 endTime。
6. 已经接入过其他视频方案，导入 Tuya IPC SDK 后，编译报错或者运行时崩溃
   * 这是由于 FFmpeg 库版本冲突，可以将已集成的视频播放库打包成动态库来解决冲突。