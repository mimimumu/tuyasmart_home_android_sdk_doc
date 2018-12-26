### 家庭管理类
ITuyaHomeManager 提供了创建家庭、获取家庭列表以及监听家庭相关的变更

可以通过TuyaHomeSdk.getHomeManagerInstance()获取

####HomeBean字段信息
```java
    private String name;//家庭名称
    private double lon;//经度
    private double lat;//纬度
    private String geoName;//家庭地理位置名称
    private long homeId;//家庭Id
    private boolean admin;//管理员身份
    private List<RoomBean> rooms = new ArrayList<>();//所有房间列表
    private List<DeviceBean> deviceList = new ArrayList<>();//所有设备列表
    private List<GroupBean> groupList = new ArrayList<>();//所有群组
    private List<BlueMeshBean> meshList = new ArrayList<>();//网关设备
    private List<DeviceBean> sharedDeviceList = new ArrayList<>();//收到的共享设备
    private List<GroupBean> sharedGroupList = new ArrayList<>();//收到的共享群组

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


#### 获取家庭列表

```java

/**
     * @param callback
     */
    void queryHomeList(ITuyaGetHomeListCallback callback);
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
     * 手机连接涂鸦云服务器成功，特别注意接收到此通知，本地数据与服务端数据可能会不一致或者无法控制设备，可以调用Home下面getHomeDetail接口重新初始化数据。
     *
     */
    void onServerConnectSuccess();
}
```



### 对家庭的缓存数据操作
**注意**: 获取此数据前，应该调用家庭的初始化接口 TuyaHomeSdk.newHomeInstance("homeId").getHomeDetail()或者TuyaHomeSdk.newHomeInstance("homeId").getHomeLocalCache 之后下面的接口才会有缓存数据。  
接口入口:TuyaHomeSdk.getDataInstance()

```java

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

