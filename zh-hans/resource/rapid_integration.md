# 快速集成

使用 CocoaPods 集成，在```Podfile```文件中添加以下内容：

```ruby
platform :ios, '8.0'

target 'your_target_name' do

pod "TuyaSmartCamera", "3.1.0"

end
```

然后在项目根目录下执行```pod update```命令。

CocoaPods的使用请参考：[CocoaPods Guides](https://guides.cocoapods.org/)
