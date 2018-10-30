### 家庭管理类
ITuyaHomeManager 提供了创建家庭、获取家庭列表以及监听家庭相关的变更

可以通过TuyaHomeSdk.getHomeManagerInstance()获取
#### 获取家庭列表

```java

/**
     * @param callback
     */
    void queryHomeList(ITuyaGetHomeListCallback callback);
```
#### 创建家庭

```java
 /**
     *
     * @param name     家庭名称
     * @param lon      经度
     * @param lat      纬度
     * @param geoName  家庭地理位置名称
     * @param rooms    房间列表
     * @param callback
     */
    void createHome(String name, double lon, double lat, String geoName, List<String> rooms, ITuyaHomeResultCallback callback);


```

#### 家庭信息的变更

```java

	 /**
     * 注册家庭信息的变更
     * 有：家庭的增加、删除、信息变更、分享列表的变更和服务器连接成功的监听
     *
     * @param listener
     */
    void registerTuyaHomeChangeListener(ITuyaHomeChangeListener listener);
    
    
    /**
     * 注销家庭信息的变更
     *
     * @param listener
     */
    void unRegisterTuyaHomeChangeListener(ITuyaHomeChangeListener listener);
    
    
public interface ITuyaHomeChangeListener {
    /**
     * 家庭添加成功
     * 用于多设备数据同步
     *
     * @param homeId
     */
    void onHomeAdded(long homeId);

    /**
     * 家庭删除成功
     * 用于多设备数据同步
     *
     * @param homeId
     */
    void onHomeRemoved(long homeId);

    /**
     * 家庭信息变更
     * 用于多设备数据同步
     *
     * @param homeId
     */
    void onHomeInfoChanged(long homeId);

    /**
     * 分享设备列表变更
     * 用于多设备数据同步
     *
     * @param sharedDeviceList
     */
    void onSharedDeviceList(List<DeviceBean> sharedDeviceList);

    /**
     *
     * 手机连接涂鸦云服务器成功，特别注意接收到此通知，在一些情况下，本地数据与服务端数据可能会不一致，可以调用Home下面getHomeDetail接口重新刷新数据。
     *
     */
    void onServerConnectSuccess();
}
```



### 对家庭的缓存数据操作

```java

获取此数据前，应该调用家庭的初始化接口 getHomeDetail、或者getHomeLocalCache 之后才会有

public interface ITuyaHomeDataManager {

    /**
     * 家庭下面的设备、群组、房间列表
     */

    List<RoomBean> getHomeRoomList(long homeId);

    /**
     * 获取家庭下面的设备列表
     *
     * @param homeId 家庭ID
     * @return
     */
    List<DeviceBean> getHomeDeviceList(long homeId);

    /**
     * 获取家庭下面的群组列表
     *
     * @param homeId
     * @return
     */
    List<GroupBean> getHomeGroupList(long homeId);

    //获取群组
    GroupBean getGroupBean(long groupId);

    //获取设备
    DeviceBean getDeviceBean(String devId);

    //根据群组ID获取房间
    RoomBean getGroupRoomBean(long groupId);

    //获取房间
    RoomBean getRoomBean(long roomId);

    //根据设备获取房间信息
    RoomBean getDeviceRoomBean(String devId);

    //获取群组下面的设备列表
    List<DeviceBean> getGroupDeviceList(long groupId);

    //获取mesh下面的群组列表
    List<GroupBean> getMeshGroupList(String meshId);

    List<DeviceBean> getMeshDeviceList(String meshId);

    /**
     * 根据房间ID获取房间下面的设备列表
     *
     * @param roomId
     * @return
     */
    List<DeviceBean> getRoomDeviceList(long roomId);

    /**
     * 根据房间ID获取房间下面的群组列表
     *
     * @param roomId
     * @return
     */
    List<GroupBean> getRoomGroupList(long roomId);

    /**
     * 获取Home数据
     *
     * @param homeId
     * @return
     */
    HomeBean getHomeBean(long homeId);
}

```
