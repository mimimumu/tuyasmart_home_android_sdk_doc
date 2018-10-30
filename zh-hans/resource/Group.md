## 群组管理

涂鸦云支持群组管理体系：可以创建群组，修改群组名称，管理群组设备，通过群组管理多个设备，解散群组。

### 创建群组

##### 【描述】

涂鸦智能提供一些设备群组控制的接口。这里的群组控制是指WiFi群组，目前只有群控的功能。群组功能默认关闭，如果需要开通群组功能，联系我们业务人员

##### 【方法调用】

```java

```

##### 【代码范例】

```java
//创建群组
TuyaHomeSdk.newHomeInstance("homeId").createNewGroup("productId", "name", devIds, new ICreateGroupCallback() {
    @Override
    public void onSuccess(long groupId) {
//返回groupId
    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```

##### 【注意事项】

群组默认不支持创建，如果你的产品需要这个功能，那么请联系我们对产品进行开启这项功能。

### 群组列表获取

##### 【描述】

此接口主要是从云端拉取最新群组列表。

##### 【方法调用】

```java
*
* @param productId    产品id 可从 DeviceBean.getProductId获取
* @param callback 回调
TuyaHomeSdk.newHomeInstance("homeId").getGroupDevList(String productId,IGetDevsFromGroupByPidCallback callback)
```

##### 【代码范例】

```java
//云端获取群组列表
TuyaHomeSdk.newHomeInstance("homeId").queryDeviceListToAddGroup("productId", new IGetDevsFromGroupByPidCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> bizResult) {

    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```

### 群组操作

##### 【描述】

涂鸦智能群组操作，主要是基于对主设备的操作，主设备是指当前群组在线的第一个设备。在线和离线状态、数据上报都是依赖于主设备的变更。发送控制命令是面对群组的所有设备。

##### 【实例化】

```java
* 群组实例化
* @param groupId 群组Id
ITuyaGroup mITuyaGroup= TuyaHomeSdk.newGroupInstance(groupId);
```

#### (1) 群组修改名称

##### 【描述】

群组修改名称

##### 【方法调用】

```java
* 群组修改名称
* @param groupId    群组id
* @param callback 回调
mITuyaGroup.updateGroupName(IControlCallback callback)
```

##### 【代码范例】

```java
//群组重命名
mITuyaGroup.updateGroupName(new IControlCallback() {
    @Override
    public void onSuccess() {
    }

    @Override
    public void onError(String s, String s1) {
    }
});
```

#### (2) 修改群组设备列表

##### 【描述】

修改群组设备列表

##### 【方法调用】

```java
* 修改群组设备列表
* @param devIds   设备devIds
* @param callback 回调
mITuyaGroup.updateGroupRelations(List<String> devIds,IControlCallback callback);
```

##### 【代码范例】

```java
//修改群组设备列表
mITuyaGroup.updateGroupRelations(getSelectedDeviceIds, new IControlCallback() {
    @Override
    public void onError(String errorCode, String errorMsg) {
    }

    @Override
    public void onSuccess() {
    }
});
```

#### (3) 解散群组

##### 【描述】

解散群组

##### 【方法调用】

```java
* 解散群组
* @param groupId    群组id
* @param callback 回调
mITuyaGroup.dismissGroup(IControlCallback callback)
```

##### 【代码范例】

```java
//删除群组
mITuyaGroup.dismissGroup(new IControlCallback() {
    @Override
    public void onSuccess() {
    }

    @Override
    public void onError(String s, String s1) {
    }
});
```

#### (4) 群组回调事件

##### 【描述】

群组回调事件

##### 【方法调用】

```java
* 注册群组回调事件
* @param listener 回调
mITuyaGroup.registerGroupListener(IGroupListener listener);

* 注销群组回调事件
mITuyaGroup.unRegisterGroupListener();
```

##### 【代码范例】

```java
//注册群组回调事件
mITuyaGroup.registerGroupListener(new IGroupListener() {
    //主设备数据点更新
    @Override
    public void onDpUpdate(String dps) {
    }
    //主设备在线状态变更
    @Override
    public void onStatusChanged(boolean status) {
    }
    //网络状态变更
    @Override
    public void onNetworkStatusChanged(boolean isNetworkOk) {
    }
});
//注销群组回调事件
mITuyaGroup.unRegisterGroupListener();

```

#### （5）发送群组控制命令

##### 【描述】

发送群组控制命令

##### 【方法调用】

```java
* 发送群组控制命令
* @param command 控制命令
* @param listener 回调
mTuyaGroup.publishDps(String command,IControlCallback listener);

```

##### 【代码范例】

```java
//群组开灯代码片段
LampBean bean = new LampBean();
bean.setOpen(true);
HashMap<String, Object> hashMap = new HashMap<>();
hashMap.put(STHEME_LAMP_DPID_1, bean.isOpen());
mTuyaGroup.publishDps(JSONObject.toJSONString(hashMap),callback)；

```

##### 【注意事项】

群组的发送命令返回结果，是指发送给云端成功，并不是指实际控制设备成功。 

#### （6）获取群组下设备列表

##### 【描述】

云端获取获取群组下设备列表

##### 【方法调用】

```java
* @param listener 回调
mTuyaGroup.getGroupDevList(IGetDevicesInGroupCallback listener)
```

##### 【代码范例】

```java
//获取群组列表
mITuyaGroup.getGroupDevList(new IGetDevicesInGroupCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> list) {
    }

    @Override
    public void onError(String errorCode, String errorMsg) {
    }
});
```

#### （7）群组数据获取

##### 【描述】

本地获取群组数据，需要初始化Home（调用getHomeDetail()或者getHomeLocalCache）之后,才能取到数据

##### 【方法调用】

```java
/**
* 本地获取群组数据bean
* @param groupId 群组id
* @return GroupBean  群组数据类
*/
TuyaHomeSdk.getDataInstance().getGroupBean(long groupId);

/**
* 本地获取群组数据列表
* @return  List<GroupBean>  群组列表
*/
TuyaHomeSdk.getDataInstance().getGroupDeviceList(long groupId);
```

#### （8）群组数据销毁

```java
//群组数据销毁，建议退出群组控制页面的时候调用。
mITuyaGroup.onDestroy();
```

## 