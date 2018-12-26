### Network Configuration for ZigBee Gateway

#### Description

It refers to the wired network configuration herein. Before the network configuration for ZigBee gateway,
please ensure that the ZigBee gateway device is connected to the router of Internet and the network of the ZigBee gateway device has been configured.

```sequence
Title: Zigbee Gateway configuration

participant APP
participant SDK
participant Zigbee Gateway
participant Service

Note over Zigbee Gateway: Reset zigbee gateway
APP->SDK: Get Token
SDK->Service: Get Token
Service-->SDK: Response Token
SDK-->APP: Response Token

APP -> APP: connect mobile phone to the same hotspot of the gateway

APP->SDK: sends the activation instruction
SDK->Zigbee Gateway: sends the activation instruction
Note over Zigbee Gateway: device receives the activation data

Zigbee Gateway->Service: activate the device
Service-->Zigbee Gateway: network configuration succeeds

Zigbee Gateway-->SDK: network configuration succeeds
SDK-->APP: network configuration succeeds

```

#### Get Network Configuration Token

Before the wifi network configuration, the SDK needs to obtain the network configuration Token from the Tuya Cloud.
The term of validity of Token is 5 minutes, and the Token become invalid once the network configuration succeeds.
A new Token has to be obtained if you have to reconfigure network.

```java
/**
* @param homeId(Reference family management section)
* @param callback
*/
TuyaHomeSdk.getActivatorInstance().getActivatorToken(homeId, new ITuyaActivatorGetToken() {
@Override
public void onSuccess(String token) {

}

@Override
public void onFailure(String s, String s1) {

}
});
```

#### Configuration Method Invocation
```java
// Initialize listener
ITuyaSmartActivatorListener listener = new ITuyaSmartActivatorListener() {
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
