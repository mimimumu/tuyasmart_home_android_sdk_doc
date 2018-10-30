ITuyaActivator 集成了WiFi配网、ZigBee配网、蓝牙mesh配网等。

### WiFi配网

##### 【描述】

```
WiFi配网主要有EZ模式和AP模式两种
```

##### 【初始化参数配置】

```java
TuyaHomeSdk.getActivatorInstance().newActivator(new ActivatorBuilder()
.setSsid(ssid)
.setContext(mContext)
.setPassword(password)
//EZ模式
.setActivatorModel(ActivatorModelEnum.TY_EZ)
//AP模式
.setActivatorModel(ActivatorModelEnum.TY_AP)
//超时时间
.setTimeOut(CONFIG_TIME_OUT) //unit is seconds
.setToken(token).setListener();
```

##### 【参数说明】

###### 【入参】

```java
/**
* @param token  配网所需要的激活key。需要通过方法TuyaHomeSdk.getActivatorInstance().getActivatorToken ()接口获取;Token的有效期为10分钟，且配置成功后就会失效（再次配网需要重新获取）
* @param ssid   配网之后，设备工作WiFi的名称。（家庭网络）
* @param password   配网之后，设备工作WiFi的密码。（家庭网络）
* @param activatorModel:	现在给设备配网有以下两种方式:
ActivatorModelEnum.TY_EZ: 传入该参数则进行EZ配网
ActivatorModelEnum.TY_AP: 传入该参数则进行AP配网
* @param timeout     配网的超时时间设置，默认是100s.
* @param context:  	需要传入activity 的context.
*/
```

###### 【出参】

ITuyaSmartActivatorListener listener 配网回调接口

```java
* @method onError(String errorCode,String errorMsg);			

@param errorCode:
1001        网络错误
1002        配网设备激活接口调用失败，接口调用不成功
1003        配网设备激活失败，设备找不到。
1004        token 获取失败
1005        设备没有上线
1006        配网超时

@param errorMsg:
暂时没有数据，请参考errorCode。

* @method onActiveSuccess(DeviceBean deviceBean);		设备配网成功,且设备上线（手机可以直接控制），可以通过
* @method onStep(String step, Object o);	
|@param step         |@param o
|device_find         |devId (String)
|device_bind_success |dev (DeviceBean)
【备注】
device_find 发现设备
device_bind_success 设备绑定成功，但还未上线，此时设备处于离线状态，无法控制设备。
```

##### 【方法调用】

```java
//getActivator Token
TuyaHomeSdk.getActivatorInstance().getActivatorToken(long homeId,ITuyaActivatorGetToken listener);
//开始配置
mTuyaActivator.startConfig();
//停止配置
mTuyaActivator.stopConfig();
//退出页面销毁一些缓存和监听
mTuyaActivator.onDestroy();
```

##### 【代码范例】

```java
//初始化回调接口
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

//配置相应参数
mTuyaActivator = TuyaHomeSdk.getActivatorInstance().newMultiActivator(new 	ActivatorBuilder()
.setSsid("ssid")
.setContext(mContext)
.setPassword("password")
.setActivatorModel(ActivatorModelEnum.TY_EZ)
.setTimeOut(100)
.setToken(token).setListener(mListener)
//开始配置
mTuyaActivator.start();
//停止配置
mTuyaActivator.stop();
//回调销毁
mTuyaActivator.onDestroy();
```

#### 【配网问题汇总】

##### 配网超时，此时设备一直处于连不上网络的状态。有以下几种原因。

- 获取WiFi Ssid 错误，导致配网失败
	安卓系统API里面获取到ssid，通常前后会有“”。
	建议使用Tuya Sdk里面自带的WiFiUtil.getCurrentSSID()去获取
- WiFi密码包含空格
	用户在输入密码的时候，由于输入法联想的功能很容易在密码中输入空格。建议密码输入的时候直接显示出来，另外在判断密码含有空格的时候，弹窗提醒用户。
- 用户不输入WiFi密码
	用户在首次使用智能设备产品的过程中，很容易不输入密码就进行后续操作
	建议判断密码输入为空且WiFi加密类型不为NONE时，弹窗提醒用户。
- 用户在AP配网时选择了设备的热点名称，用户首次使用智能产品的过程中，很容易出现此问题。
	建议在判别AP配网时用户选择了设备的热点名称，弹窗提醒给用户。
- 获取WiFi的Ssid为"0x","\<unknown ssid\>"
	目前发现在一些国产手机会出现此问题。并不是用户选择的WiFi名称。这是由于定位权限没开启导致的，建议用户可以手动输入WiFi的Ssid，或者给出弹窗提醒，让用户开启相应权限。

##### 配网超时，此时设备已经激活成功。可能原因有：

- APP没有连接到正常的网络，导致无法获取设备的状态。