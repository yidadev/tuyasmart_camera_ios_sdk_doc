# Manual Integration

Click [HERE](https://airtake-public-data.oss-cn-hangzhou.aliyuncs.com/smart/app/package/sdk/ios/TuyaSmartCamera-V3.1.0.zip) to download SDK. After adding ```TuyaSmartCamera.framework``` in project category, the porject category like below:

![Project Category](./images/camera_sdk_import_1.png)

Then choose project related target, under general setting, add system dependencies to "Linked Frameworks and Libraries", the required dependencies are: "Photos.framework", "libresolv.tbd", "libbz2.tbd", "libiconv.tbd", "libz.tbd", "libc++.tbd". like below:

![Adding Procedure](./images/camera_sdk_import_2.png)

Then switch to "Build Settings", enter "other linker" in the search box, and add "-ObjC" in "Other Linker Flags", like below：

![Adding Finish](./images/camera_sdk_import_3.png)
