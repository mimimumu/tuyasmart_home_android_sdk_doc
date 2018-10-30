### ZigBee网关配网

```
这里的ZigBee网关配网是指有线配网
```

#### ZigBee网关有线配网

ZigBee网关配网前，请确保ZigBee网关设备连接上外网联通的路由器，并使ZigBee网关设备处于配网状态

##### 【方法调用】

```java
//需要传入HomeId getActivator Token 
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
//初始化监听
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
            
//开始配网
mITuyaActivator.start()
//停止配网
mITuyaActivator.stop()
//退出页面清理
mITuyaActivator.onDestroy()
```


