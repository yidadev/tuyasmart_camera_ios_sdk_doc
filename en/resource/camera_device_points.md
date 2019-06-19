
# Camera function data-points





**【Description】**

Tuya smart camera supports various function data-points. The interaction between devices and APP is realized through standardized function settings. Certain return data of the device send notifications to APP by asynchronous messaging.



### Camera device standardized features

The details defined in ```TuyaSmartCameraDPManager.h```.


- Device basic features setting

| Data-point | Data type | value | Description |            Feature definition |
| ------ | ------ | :----: | ------ | ------ |
| TuyaSmartCameraBasicIndicatorDPName | BOOL |                                         YES： turn on indicator under normal state                                 NO: turn off indicator under normal state | Status indicator | Turn on/off device indicator under normal state: avoid light pollution at night. Indicator needs to be turned on during wifi configuration process or device is abnormal. |
| TuyaSmartCameraBasicFlipDPName | BOOL |                 YES: flip               NO: normal status                 | Screen flip |                          Turn on/off screen vertical flip: adjust screen direction in the case when the device is upside down. |
| TuyaSmartCameraBasicOSDDPName | BOOL | YES: OSD appears         NO: OSD disappears         |  Time watermark | Turn on/off time watermark in live view mode. |
| TuyaSmartCameraBasicPrivateDPName | BOOL | YES: private mode turned on             NO: private mode turned off | Private mode | Turn on/off private mode: no video&audio stream collecting, the device has no live view and playback. |
| TuyaSmartCameraBasicNightvisionDPName | enum | Auto, 1: Off, 2: On |  IR night vision | Select IR night vision status On/Off/Auto. |
| TuyaSmartCameraBasicPIRDPName | enum | 0：off, 1: low sensitivity, 2: medium sensitivity, 3: high sensitivity | PIR switch and sensitivity | Adjust PIR status: Off/low sensitivity/medium sensitivity/ high sensitivity. |

- Motion detection alarm

| Data-point     | Data type | value                                                        | Description                                              | Feature definition |
| ---------------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TuyaSmartCameraMotionDetectDPName | BOOL            | YES: motion detection alarm turned on               NO: motion detection alarm turned off | Motion detection alarm switch | Turn on/off motion detection alarm. After turning on: device needs to report messages to server once movement is detected under suitable conditions. |
| TuyaSmartCameraMotionSensitivityDPName | enum     | 0: low,1:medium, 2:high | motion detection sensitivity setting | Set motion detection alarm sensitivity. |


- Sound detection alarm

| Data-point       |  Data type | value                                                        |      Description                                        |                                Feature definition |
| ----------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TuyaSmartCameraDecibelDetectDPName | BOOL           | YES: sound detection alarm turned on              NO: sound detection alarm turned off |  Sound detection alarm switch               |  Turn on/off sound detection alarm. After turning on: device needs to report messages to server once sound is detected under suitable conditions. |
| TuyaSmartCameraDecibelSensitivityDPName | enum                  | low,1:medium, 2:high |  sound detection sensitivity setting | Set sound detection alarm sensitivity. |


- Memory card and local video recording management

|  Data-point  |   Data type | value                                                        |  Description                                            |              Feature definition                     |
| ------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| TuyaSmartCameraSDCardStatusDPName | enum(int)    |  1: normal, 2: abnormal (SD card is broken or under wrong format), 3: insufficient space, 4: formatting, 5: no SD card inserted |  Memory card status                              | Device reports current SD card status to the cloud, including SD card status: normal /abnormal /insufficient space etc. |
| TuyaSmartCameraSDCardStorageDPName | string                | JSON string                                              |   Memory card capacity info research     | Report memory card current capacity status/ capacity used/ capacity left. |
| TuyaSmartCameraSDCardFormatDPName | BOOL           |                                                       |       Memory card formatting                    | Initial formatting instruction, report current memory card formatting process. |
| TuyaSmartCameraSDCardFormatStateDPName | NSInteger        |              Positive number of formatting process 0-100 |   current memory card formatting process |     Positive number of formatting process: 100 means formatting success.  Error: -2000: SD card is formatting, -2001: SD card formatting. |
| TuyaSmartCameraSDCardRecordDPName | BOOL           | YES: local video recording turned on        NO: local video recording turned off |  local video recording switch                  | Trun on/off SD card record: device will not record video and save to SD card after turning off the switch. |
| TuyaSmartCameraRecordModeDPName | enum                  |                                1: event record (start to record once movement is detected)                             2: continuous record |   local video recording mode select    |   select local video recording mode, support event record and continuous record. |


- PTZ functions

| Data-point |  Data type | value                                                        |  Description                      |      Feature definition           |
| ----------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------------------ |
| TuyaSmartCameraPTZControlDPName | enum                |             0: left, 2: right, 4: down, 6: up |   PTZ direction  control |                                       Use to control the PTZ, and adjust direction. |
| TuyaSmartCameraPTZStopDPName | BOOL         |          | Stop PTZ         |   PTZ stop moving, no parameters. |


- Battery powered product features

|  Data-point       |  Data type | value                                                        | Description                                      |                          Feature definition       |
| ------------------------ | -------------------- | ------------------------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------ |
| TuyaSmartCameraWirelessElectricityDPName | NSInteger       |     Device power, report 0-100 integer |    Battery and device status info |   Device reports current battery power, and reports when power changes. Device sends messages to cloud current device sleep/wake status. |
| TuyaSmartCameraWirelessLowpowerDPName | NSInteger       | value                                                        |          Low power alarm value       | Users can set the value in App. Alarm regulation: dp145≤dp147. |
| TuyaSmartCameraWirelessBatteryLockDPName | BOOL          |  YES: locked    NO: unlocked |    Battery lock                           |                 Turn on/off battery lock control. |
| TuyaSmartCameraWirelessPowerModeDPName | enum                 |   0: Battery power supply, 1: Plug power supply |      Device power supply method    |      Device reports current power supply status. Report once power supply method changes. |
| TuyaSmartCameraWirelessAwakeDPName | BOOL | YES: the device is awaking. NO: the device is sleeping. | device awake state | Device reports current state is awaking, report NO when sleeping, report YES when awaking. |



### Device standardized feature method call


TuyaSmartCameraDPManager provides device info communication ability, control orders issue, reach current data-points info

```objective-c
// init TuyaSmartCameraDPManager instance with device id
TuyaSmartCameraDPManager *dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:devId];
```



#### Listener


 TuyaSmartCameraDPObserver protocol provides listener device info data receiving.

```objective-c
// dpsData is the data that device reports。get the value of DP though [dpsData objectForKey:TuyaSmartCameraDPKey]。
- (void)cameraDPDidUpdate:(TuyaSmartCameraDPManager *)manager dps:(NSDictionary *)dpsData;
```




#### Data-point support or not


**【Description】**


Judge whether the device supports the data-point. Call data issues/data research will fail if the device dose not support certain data-point. 

```objective-c
- (BOOL)isSupportDP:(TuyaSmartCameraDPKey)dpName;
```



【Method call】

```objective-c
BOOL isSupportDpBasicFlip = [self.dpManager isSupportDP:TuyaSmartCameraBasicFlipDPName];
```



#### Data publish


**【Description】**

Send message through LAN or cloud



**【Method call】**

```objective-c
// the type of data is id, if the dp value is int or BOOL, need packed as NSNumber
[self.dpManager setValue:@(YES) forDP:TuyaSmartCameraBasicFlipDPName success:^(id result) {
        // success, result is the reponse of device, usually as same as the publish data
} failure:^(NSError *error) {
        // failed
}];
```



#### Data research


**【Description】**

Obtain data-point info through cache, if has no cache, will push command to device for query the value.



```objective-c
// get the value of dp from app cache, most of the dps could get the latest value, device will auto report
- (id)valueFortDP:(TuyaSmartCameraDPKey)dpName;
// publish the query command for dp
- (void)valueForDP:(TuyaSmartCameraDPKey)dpName success:(TYSuccessID)success failure:(TYFailureError)failure;
```



**【Method call】**


Value data-point research

```objective-c
TuyaSmartCameraRecordMode mode = [self.dpManager valueForDP:TuyaSmartCameraRecordModeDPName];

[self.dpManager valueForDP:TuyaSmartCameraSDCardStorageDPName success:^(id result) {
	// succes, result is the value of the dp。
} failure:^(NSError *error) {
    // failed
}];
```




### Others


the ```enum``` of the dp list in the header, actually is the NSString enum, there defines the constant reference in ```TuyaSmartCameraDPManager.h```.

```objective-c
typedef NSString * TuyaSmartCameraNightvision NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCaemraPIR NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraMotion NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraDecibel NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraRecordMode NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraPTZDirection NS_EXTENSIBLE_STRING_ENUM;
typedef NSString * TuyaSmartCameraPowerMode NS_EXTENSIBLE_STRING_ENUM;
```


How to use:

```objective-c
/// nightvision state has three values.
/// TuyaSmartCameraNightvisionAuto,TuyaSmartCameraNightvisionOff,TuyaSmartCameraNightvisionOn
[self.dpManager setValue:TuyaSmartCameraNightvisionOn forDP:TuyaSmartCameraBasicNightvisionDPName success:^(id result) {
        
} failure:^(NSError *error) {
        
}];
```




#### Memory card 


If device support sd card, could get the sd card status and format sd card.

```objective-c
// sd card state
typedef NS_ENUM(NSUInteger, TuyaSmartCameraSDCardStatus) {
    TuyaSmartCameraSDCardStatusNormal = 1,			// sd card working
    TuyaSmartCameraSDCardStatusException = 2,		// exception, can not get info
    TuyaSmartCameraSDCardStatusMemoryLow = 3,       // low memory
    TuyaSmartCameraSDCardStatusFormatting = 4,	    // sd card is formatting
    TuyaSmartCameraSDCardStatusNone = 5             // not detected sd card
};

// get sd card status
TuyaSmartCameraSDCardStatus status = [[self.dpManager valueForDP:TuyaSmartCameraSDCardStatusDPName] integerValue];

// get sd card storage
[self.dpManager valueForDP:TuyaSmartCameraSDCardStorageDPName success:^(id result) {
	// result is string, total space，used space，free space separated by "|", the unit is kB.
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

// format sd card
[self.dpManager setValue:@(YES) forDP:TuyaSmartCameraSDCardFormatDPName success:^(id result) {
	// sd card is formatting
} failure:^(NSError *error) {
	// failed
}];

// get the progress of sd card formatting
[self.dpManager setValue:@(YES) forDP:TuyaSmartCameraSDCardFormatStateDPName success:^(id result) {
	NSInteger progress = [result integerValue];
} failure:^(NSError *error) {
	// failed
}];
```


