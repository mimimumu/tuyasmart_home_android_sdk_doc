## 群组管理

### 描述

涂鸦云支持群组管理体系：可以创建群组，修改群组名称，管理群组设备，通过群组管理多个设备，解散群组。
涂鸦智能提供一些设备群组控制的接口。这里的群组控制是指WiFi群组，目前只有群控的功能。群组功能默认关闭，如果需要开通群组功能，联系我们业务人员

### 群组列表获取

群组未创建，获取可创建群组的设备列表

```java
//群组未创建，入参groupId传0
TuyaHomeSdk.newHomeInstance("homeId").queryDeviceListToAddGroup(groupId, "productId", new IGetDevsFromGroupByPidCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> bizResult) {

    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```
群组已经创建，获取群组的设备列表

```java
TuyaHomeSdk.newHomeInstance("homeId").queryDeviceListToAddGroup(groupId, "productId", new IGetDevsFromGroupByPidCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> bizResult) {

    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```

### 创建群组

```java
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

### 编辑群组

```java
TuyaHomeSdk.newGroupInstance(groupId).updateDeviceList(deviceIds, new IResultCallback() {
                @Override
                public void onError(String s, String s1) {

                }

                @Override
                public void onSuccess() {

                }
            });
```

### 解散群组

```java
TuyaHomeSdk.newGroupInstance(groupId).dismissGroup(new IResultCallback() {
                @Override
                public void onError(String s, String s1) {
	
                }
	
                @Override
                public void onSuccess() {
	
                }
            });
```

### 修改群组名称

```java
TuyaHomeSdk.newGroupInstance(groupId).renameGroup(titleName, new IResultCallback() {
            @Override
            public void onError(String s, String s1) {

            }

            @Override
            public void onSuccess() {
                           }
        });
```

### 群组操控

#### 实例化

```java
* 群组实例化
* @param groupId 群组Id
ITuyaGroup mITuyaGroup= TuyaHomeSdk.newGroupInstance(groupId);
```

#### 群组dp控制回调

```java
* 注册群组回调事件
* @param listener 回调
mITuyaGroup.registerGroupListener(new IGroupListener() {
            @Override
            public void onDpUpdate(long l, String s) {

            }

            @Override
            public void onGroupInfoUpdate(long l) {

            }

            @Override
            public void onGroupRemoved(long l) {

            }
        });

* 注销群组回调事件
mITuyaGroup.unRegisterGroupListener();
```

#### 发送群组控制命令

```java
* 发送群组控制命令
* @param command 控制命令
* @param listener 回调
mTuyaGroup.publishDps(String command,IControlCallback listener);

```

代码范例

```java
//群组开灯代码片段
LampBean bean = new LampBean();
bean.setOpen(true);
HashMap<String, Object> hashMap = new HashMap<>();
hashMap.put(STHEME_LAMP_DPID_1, bean.isOpen());
mTuyaGroup.publishDps(JSONObject.toJSONString(hashMap),callback)；

```

注意事项

群组的发送命令返回结果，是指发送给云端成功，并不是指实际控制设备成功。 

#### 群组数据获取

本地获取群组数据，需要初始化Home（调用getHomeDetail()或者getHomeLocalCache）之后,才能取到数据

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

#### 群组数据销毁

```java
//群组数据销毁，建议退出群组控制页面的时候调用。
mITuyaGroup.onDestroy();
```