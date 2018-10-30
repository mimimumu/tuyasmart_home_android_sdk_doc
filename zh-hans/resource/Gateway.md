## 网关

网关类封装了ZigBee网关的相关操作，包括控制，查询子设备，监听子设备状态等。
可以通过TuyaHomeSdk.newGatewayInstance()去初始化网关。

```
public interface ITuyaGateway {
    /**
     * 发送命令
     *
     * @param dps
     * @param callback
     */
    void publishDps(String localId, String dps, IResultCallback callback);

    /**
     * 广播控制设备
     *
     * @param dps
     * @param callback
     */
    void broadcastDps(String dps, IResultCallback callback);

    /**
     * 组播控制设备
     *
     * @param localId
     * @param dps
     * @param callback
     */
    void multicastDps(String localId, String dps, IResultCallback callback);

    /**
     * 获取网关子设备
     *
     * @param callback
     */
    void getSubDevList(ITuyaDataCallback<List<DeviceBean>> callback);

    /**
     * 注册子设备信息变更
     *
     * @param listener
     */
    void registerSubDevListener(ISubDevListener listener);

    /**
     * 注销子设备信息变更
     */
    void unRegisterSubDevListener();
}

```

------

