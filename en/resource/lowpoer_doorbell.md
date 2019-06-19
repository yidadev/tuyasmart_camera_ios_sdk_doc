# Low-power Doorbell

A low-power doorbell will sleep after a few seconds without a P2P connection. It cannot be directly connected to P2P after hibernation. It needs to wake up first, and then connect to the P2P channel after waking up. 

```objective-c
self.dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:self.devId];
self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];
// Add observer for DP 
[self.dpManager addObserver:self];
// if it is a low-power doorbell
- (BOOL)isDoorbell {
    return [self.dpManager isSupportDP:TuyaSmartCameraWirelessAwakeDPName];
}

- (BOOL)start {
    if ([self isDoorbell]) {
        // Get the status of the device, if it is awaking up.
        BOOL isAwaking = [[self.camera.dpManager valueForDP:TuyaSmartCameraWirelessAwakeDPName] boolValue];
		if (isAwaking) { // Is waking up, can connect.
			if (self.isConnected) {
                [self.videoContainer addSubview:self.camera.videoView];
                self.camera.videoView.frame = self.videoContainer.bounds;
                [self.camera startPreview];
            }else {
                [self.camera connect];
            }
		}else { // Is sleeping, send wake up command.
            [self.device awakeDeviceWithSuccess:nil failure:nil];
        }
    }
}
// TuyaSmartCameraDPObserver will call this method when dp is updated
- (void)cameraDPDidUpdate:(TuyaSmartCameraDPManager *)manager dps:(NSDictionary *)dpsData {
    // if the value of TuyaSmartCameraWirelessAwakeDPName is 'YES'，the device is waking up.
    if ([[dpsData objectForKey:TuyaSmartCameraWirelessAwakeDPName] boolValue]) {
        [self start];
    }
}

```

