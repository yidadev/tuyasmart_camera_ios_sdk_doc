

* After calling startPreview, there is no video frame and no error feedback.
  - Check if the camera object was created successfully.  The TuyaSmartCameraSDK was split, when importing with pod 'TuyaSmartCamera', the default is not supported with p2p 1.0. So check the p2pType when creating the camera. If it is 1, use pod 'TuyaSmartCamera/ALL' to import the SDK.
  - Check if the p2p connection is successful. ```startPreivew``` must be called after receiving the ```cameraDidConnected:``` callback.
* Crash after called ```- (void)destory```
  - In the old version, there is a bug, when the p2pType is 1, the call to the destroy method does crash. It has been fixed in versions later than 3.1.1.
  - Stop the preview or playback before calling the destory method and call disConnect to disconnect the p2p connection.
* When debugging with the simulator, crash when previewing.
  - Some cameras use hardware decoding, the simulator does not support, please use real machine debugging.

