# Rapid Integration

Using cocapods, add below detail in pod file.

```ruby
platform :ios, '9.0'

target 'your_target_name' do

	pod "TuyaSmartCameraKit"
    # pod "TuyaSmartCameraT"

end
```

If you need support p2p 1.0, you should att the line ```pod "TuyaSmartCameraT"```. Excute pod update in project root file.

How to use cocoapods：[CocoaPods Guides](https://guides.cocoapods.org/)
