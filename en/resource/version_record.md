# Change Log

## Document change log

| Version | Description | Author | Date       |
| ------- | ----------- | ------ | ---------- |
| v1.0.0  | Create      | FuLang | 2020-03-01 |



## SDK change log

### 3.15.0 (2020-02-21)

| Module              | Version |
| ------------------- | ------- |
| TuyaCameraSDK       | 3.15.3  |
| TuyaSmartCameraBase | 4.4.2   |
| TuyaSmartCameraM    | 4.3.4   |
| TuyaSmartCameraKit  | 4.4.4   |

**New Features**

* TuyaCamera is renamed TuyaCameraSDK. If you have specified the TuyaCamera version number in the podfile, you need to change the library name and version.
* Removed some classes and interfaces that have not been standardized (not described in the documentation).
* Alarm messages and pictures in cloud storage events are changed to unencrypted.
* Fixed some stability issues.

### 3.13.3 (2019-12-20)

| Module              | Version |
| ------------------- | ------- |
| TuyaSmartCameraBase | 4.2.6   |
| TuyaSmartCameraM    | 4.2.6   |
| TuyaCamera          | 3.13.4  |

**New Features**

* Add interface to get audio data of talk, provide to development process the audio.

### 3.13.0 (2019-12-05)

| Module              | Version |
| ------------------- | ------- |
| TuyaSmartCameraBase | 4.2.5   |
| TuyaSmartCameraM    | 4.2.5   |
| TuyaSmartCameraKit  | 4.3.2   |
| TuyaCamera          | 3.13.3  |

**New Features**

* The P2P and audio and video codec libraries are independent TuyaCamera components, and they are changed to dynamic library  to resolve the conflict of different library versions.
* Add interfaces for recording, screenshots, and saving to a specified file path.

### 3.10.0 (2019-10-25)

| Module                       | Version |
| ---------------------------- | ------- |
| TuyaSmartCameraBase          | 4.0.2   |
| TuyaSmartCameraM             | 4.0.2   |
| TuyaSmartCameraKit           | 4.0.1   |
| TYCameraCloudServicePanelSDK | 0.2.1   |
| TuyaSmartLogger              | 0.1.0   |

