# Functions Of Device

The ```dps``` (```NSDictionary``` type) attribute of the ```TuyaSmartDeviceModel``` class defines the state of the device, and the state is called data point (DP) or function point. Each ```key``` in the ```dps``` dictionary refers to a ```dpId``` of a function point, and ```Value``` refers to the ```dpValue``` of a function point. The ```dpValue``` is the value of the function point.  Click [there](https://tuyainc.github.io/tuyasmart_home_ios_sdk_doc/en/resource/Device.html#functions-of-device) view detail about DP.

If the ```dpId``` of control ptz is "119",  ```dpId``` of stop controlling ptz is "116". The ```dpValue``` of directions is "0"(up), "4"(down), "6"(left), "2"(right). The code is showed below:

```objective-c
//self.device = [TuyaSmartDevice deviceWithDeviceId:@"your_device_id"];

// Whether to support PTZ control
- (BOOL)isSupportPtz {
	if ([self.device.deviceModel.dps objectForKey:@"119"]) {
		return YES;
	} else {
		return NO;
	}
}

// start PTZ control
- (void)startPtz:(UISwipeGestureRecognizerDirection)direction {
	NSString *d = @"0";
	if (direction == UISwipeGestureRecognizerDirectionRight) {
		d = @"2";
	} else if (direction == UISwipeGestureRecognizerDirectionLeft) {
		d = @"6";
	} else if (direction == UISwipeGestureRecognizerDirectionUp) {
		d = @"0";
	} else if (direction == UISwipeGestureRecognizerDirectionDown) {
		d = @"4";
	}
	NSDictionary *dps = @{@"119": d};
	// publish dp data
	[self.device publishDps:dps success:^{
		// publish success, when receive new dp data, will call 'deviceDpsUpdate' method
		NSLog(@"publishDps success");
	} failure:^(NSError *error) {
		NSLog(@"publishDps failure: %@", error);
	}];
}

// stop ptz control
- (void)stopPtz {
	NSDictionary *dps = @{@"116": @(YES)};
	// publish dp data
	[self.device publishDps:dps success:^{
        //publish success
		NSLog(@"publishDps success");
	} failure:^(NSError *error) {
		NSLog(@"publishDps failure: %@", error);
	}];
}
```
