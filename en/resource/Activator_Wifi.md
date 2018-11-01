The TuyaActivator has three network configuration methods, namely, the Wifi, ZgigBee and Bluetooth mesh network configuration. 

## Wifi Network Configuration

**[Description]**

The Wifi network configuration mainly includes two network configuration modes, namely, EZ mode and AP mode. 

**[Initial Parameter Configuration]**

```java
TuyaHomeSdk.getActivatorInstance().newActivator(new ActivatorBuilder()
.setSsid(ssid)
.setContext(mContext)
.setPassword(password)
//EZ mode
.setActivatorModel(ActivatorModelEnum.TY_EZ)
//AP mode
.setActivatorModel(ActivatorModelEnum.TY_AP)
// Timeout
.setTimeOut(CONFIG_TIME_OUT) //unit is seconds
.setToken(token).setListener();
```
**[Parameter Description]**

**[Parameter Transfer]**

```java
/**
* @param token  The activation key required for network configuration. The TuyaActivator.getInstance() and the getActivatorToken () interface needs to be used to obtain the activation key. The validity period of Token is 10 minutes, and the Token will become invalid once the configuration succeeds and a new one will be applied for re-configuration. 
* @param ssid    Name of the Wifi used by devices in working after the network isconfigured.  (Home network)
* @param ssid    Password of the Wifi used by devices in working after the network is configured.  (Home network)
* @param activatorModel: 	Currently, the following two methods are used for network configuration of devices.
ActivatorModelEnum.TY_EZ: The EZ mode network configuration is enabled is this parameter is transfered. 
ActivatorModelEnum.TY_AP: The AP mode network configuration is enabled is this parameter is transfered.
* @param timeout     The timeout for the network configuration is set to 100s by default. 
* @param context:   	The context that is needed to be transferred to the activity. 
*/
```
**[Output Parameter]**

ITuyaSmartActivatorListener listener network configuration callback interface
```java
/**
* @method onError(String errorCode,String errorMsg);			
@param errorCode:
1001        Network error
1002        The invocation of device network configuration interface fails. 
1003        The activation of network configuration device fails, and device is not detected. 
1004        Obtaining token fails.
1005        The device is offline.
1006        Timeout in network configuration
@param errorMsg:
Temporarily, no data is available, please refer to the errorCode.
* @method onActiveSuccess(DeviceBean deviceBean);		The network configuration of device succeeds, and the device goes online (Mobile phone can control device directly).

* @method onStep(String step, Object o);	

|@param step         |@param o

|device_find         |devId (String)

|device_bind_success |dev (DeviceBean)

[Note]

Device_find Find device.

Device_bind_success Binding device succeeds, but the device is not online yet and cannot be controlled with your mobile phone.
**/
```
**[Method Invocation]**
```java
//getActivator Token
TuyaHomeSdk.getActivatorInstance().getActivatorToken(long homeId,ITuyaActivatorGetToken listener);
// Start configuration
mTuyaActivator.startConfig();
// Stop configuration
mTuyaActivator.stopConfig();
// Exit the page to destroy some cache data and monitoring data.
mTuyaActivator.onDestroy();
```
**[Example Codes]**
```java
// Initiate the callback interface.

ITuyaSmartActivatorListener mListener = new ITuyaSmartActivatorListener() {
	@Override
    public void onError(String errorCode, String errorMsg){
    }
    @Override
    public void onActiveSuccess(DeviceBean deviceBean) {
    }
    @Override
	public void onStep(String step, Object o) {
 	}
};



// Configure related parameter

mTuyaActivator = TuyaHomeSdk.getActivatorInstance().newMultiActivator(new 	ActivatorBuilder()
.setSsid("ssid")
.setContext(mContext)
.setPassword("password")
.setActivatorModel(ActivatorModelEnum.TY_EZ)
.setTimeOut(100)
.setToken(token).setListener(mListener)
// Start configuration
mTuyaActivator.start();
// Stop configuration
mTuyaActivator.stop();
// Destroy callback
mTuyaActivator.onDestroy();
```

## Summary of Problems for Network Configuration

**Timeout in network configuration. The device cannot be connected to the network. The causes may be:**

- The obtained WiFi Ssid is incorrect. The ssid obtained in the Android system usually has “” in the front or back of it.  It is recommended to use the WiFiUtil.getCurrentSSID() in the Tuya Sdk to obtain the Wifi Ssid. 
- The Wifi password has space. User may accidentally enter the space because of the predictive typing function. It is recommended that users enable the displaying password option when entering password. Or prop-up windows shall be displayed if the password entered includes spaces. 
- Users have not entered the Wifi password. User may forget to enter the password when they use the smart device for the first time. It is recommended that prop-up windows be displayed if no password is entered and the Wifi encryption type is not set to NONE.
- Users select the hotspot name of device in the AP mode network configuration. This problem is prevalent when users use the smart device for the first time. It is recommended that prop-up windows be displayed if users selected the hotspot name of devices in the AP mode network configuration. 
- The Ssid of the obtained Wifi is “0x” and <unknown ssid>. Currently, this problem may occur in Chinese mobile phones, and this problem is not caused by Wifi but the permission to locate. It is recommended that user enter the Ssid of Wifi manually or the prop-up windows be displayed to ask users to authorize permissions.

**Timeout in network configuration, but the device has been successfully activated. The possible cause is:**

- The App is not using a normal network, and the status of device cannot be obtained. 