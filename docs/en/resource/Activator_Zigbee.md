## Network Configuration for ZigBee Gateway

It refers to the wired network configuration herein.

### Wired Network Configuration for ZigBee Gateway

Before the network configuration for ZigBee gateway, please ensure that the ZigBee gateway device is connected to the router of Internet and the network of the ZigBee gateway device has been configured.

**[Method Invocation]**
```java
// Uploading of HomeId getActivator Token is required. 
TuyaHomeSdk.getActivatorInstance().getActivatorToken(long homeId,ITuyaActivatorGetToken listener);
// Start configuration
mTuyaActivator.startConfig();
// Stop configuration
mTuyaActivator.stopConfig();
// Exit the page to destroy some caches and listeners
mTuyaActivator.onDestroy();
```
**[Example Codes]**
```java
// Initialize listener

ITuyaSmartActivatorListener  listener =new ITuyaSmartActivatorListener() {
                @Override
                public void onError(String errorCode, String errorMsg) {
						
                }

                @Override
                public void onActiveSuccess(DeviceBean deviceBean) {
                	
                }
                @Override
                public void onStep(String step, Object data) {

                }
            })

ITuyaActivator mITuyaActivator = TuyaHomeSdk.getActivatorInstance().newGwActivator(new TuyaGwActivatorBuilder()
            .setToken(token)
            .setTimeOut(100)
            .setContext(this)
            .setListener(listener);
// Start network configuration
mITuyaActivator.start()
// Stop network configuration
mITuyaActivator.stop()
// Exit the page to clean up
mITuyaActivator.onDestroy()
```