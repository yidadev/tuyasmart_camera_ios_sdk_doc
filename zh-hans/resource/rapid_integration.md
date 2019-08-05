# 快速集成

使用 CocoaPods 集成，在```Podfile```文件中添加以下内容：

```ruby
platform :ios, '9.0'

target 'your_target_name' do

	pod "TuyaSmartCameraKit"
	# pod "TuyaSmartCameraT"

end
```

如果您只需要支持 p2p 2.0 的设备，则使用 ```pod "TuyaSmartCameraKit"``` 就足够了，如果您还需要支持 p2p 1.0 的设备，则需要增加 ```pod "TuyaSmartCameraT"```, 然后在项目根目录下执行```pod update```命令。

CocoaPods的使用请参考：[CocoaPods Guides](https://guides.cocoapods.org/)
