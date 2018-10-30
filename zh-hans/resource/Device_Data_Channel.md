### 设备数据流通道

主要用于扫地机地图数据等大量实时上报的场景

```java
ITuyaSingleTransfer singleTransfer = TuyaHomeSdk.getTransferInstance();
```

```java
public interface ITuyaSingleTransfer {
	/**
	 * 开始连接
	 */
    void startConnect();

	/**
	 * 是否在线
	 */
    boolean isOnline();

	/**
	 * 订阅设备数据，订阅设备之后，设备如果有数据上报上来，便可以通过 registerTransferDataListener 回调上来。需要注意的是，每次通道连接成功都需要重新订阅设备数据
	 */
    void subscribeDevice(String devId);

	/**
	 * 取消订阅设备信息，则设备数据不在收到
	 *
	 */
    void unSubscribeDevice(String devId);

	/**
	 * 注册设备数据流，SDK不做数据解析，具体格式需要和硬件上报方协商一致，然后解析。
	 */
    void registerTransferDataListener(ITuyaDataCallback<TransferDataBean> callback);
	/**
	 * 取消订阅设备数据流
	 */
    void unRegisterTransferDataListener(ITuyaDataCallback<TransferDataBean> callback);

    /**
     * 注册通道状态变化，在网络波动的情况下会导致通道断开重连的情况 
     */
    void registerTransferCallback(ITuyaTransferCallback callback);

    /** 取消注册
    */
    void unRegisterTransferCallback(ITuyaTransferCallback callback);

    /**
    	断开数据流通道
    */
    void stopConnect();
}

```



### 数据模型

#### DeviceBean

- iconUrl 设备图标链接地址
- devId 设备唯一标示id
- isOnline 设备是否在线（局域网或者云端在线） 
- name 设备名称
- schema 用来描述设备dp点属性
- productId 产品唯一标示id
- pv  网关通信协议版本 用x.x来表示 。
- bv  网关通用固件版本 用x.x来表示。
- time 设备添加时间
- isShare 设备是否被分享
- schemaMap Map类型 key 表示dpId， value 表示Schema 数据。
- dps  设备当前数据信息。key 是 dpId ，value 是值。
- lon、lat用来标示经纬度信息，需要用户使用sdk前，调用TuyaSdk.setLatAndLong 设置经纬度信息。
- isZigBeeWifi 是否是ZigBee网关设备
- hasZigBee 是否包含ZigBee能力（网关设备或者子设备）