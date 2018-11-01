## Network Configuration for ZigBee Sub-device
The network configuration for ZigBee sub-device can be initiated only when the cloud for ZigBee gateway device is online, and the sub-device has been configured.
[Method Invocation]
```java
TuyaHomeSdk.getActivatorInstance().newGwSubDevActivator(TuyaGwSubDevActivatorBuilder builder)
// Start configuration
mTuyaActivator.start();
// Stop configuration
mTuyaActivator.stop();
// Exit the page to destroy some caches and listeners
mTuyaActivator.onDestroy();
```
[Example Codes]
```java
TuyaGwSubDevActivatorBuilder builder = new TuyaGwSubDevActivatorBuilder()
					// Setting the gateway ID
                .setDevId(mDevId)
                // Setting the time-out period for network configuration
                .setTimeOut(100)
                .setListener(new ITuyaSmartActivatorListener() {
                    @Override
                    public void onError(String s, String s1) {
                    }

                    @Override
                    public void onActiveSuccess(DeviceBean deviceBean) {
                    }

                    @Override
                    public void onStep(String s, Object o) {
                    }
                });

        mTuyaGWActivator = TuyaHomeSdk.getActivatorInstance(). newGwSubDevActivator(builder);
        // Start network configuration
        mTuyaGWActivator.start();
        // Stop network configuration
        mTuyaGWActivator.stop();
        // Destroy
        mTuyaGWActivator.onDestory();
```