## ZigBee子设备配网
ZigBee子设备配网需要ZigBee网关设备云在线的情况下才能发起,且子设备处于配网状态。

【方法调用】
```java
TuyaHomeSdk.getActivatorInstance().newGwSubDevActivator(TuyaGwSubDevActivatorBuilder builder)
//开始配置
mTuyaActivator.start();
//停止配置
mTuyaActivator.stop();
//退出页面销毁一些缓存和监听
mTuyaActivator.onDestroy();
```
【代码范例】
```java
TuyaGwSubDevActivatorBuilder builder = new TuyaGwSubDevActivatorBuilder()
					//设置网关ID
                .setDevId(mDevId)
                //设置配网超时时间
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
        //开始配网
        mTuyaGWActivator.start();
        //停止配网
        mTuyaGWActivator.stop();
        //销毁
        mTuyaGWActivator.onDestory();

```