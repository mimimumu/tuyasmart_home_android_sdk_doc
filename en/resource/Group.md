## Group Management

The Tuya Cloud supports the group management system. User can create group, change group name, manage devices of group, manage multiple devices via the group and dismiss group.
Tuya Smart provides some interfaces for device group control. The group control herein refers to the WiFi group, and currently, only the group control function is available. The group function is disabled by default. If necessary, please contact our staff.

### Group List Acquisition

Obtain the device list of product when the group is not created.

```java
//the group is not created, parameter groupId must be an integer 0
TuyaHomeSdk.newHomeInstance("homeId").queryDeviceListToAddGroup(groupId, "productId", new IGetDevsFromGroupByPidCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> bizResult) {

    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```
Obtain the device list of a group when the group is created.

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

### Creating a Group

```java
TuyaHomeSdk.newHomeInstance("homeId").createNewGroup("productId", "name", devIds, new ICreateGroupCallback() {
    @Override
    public void onSuccess(long groupId) {
			//return groupId
    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```

### Edit Group

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

### Dismiss Group

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

### Modifying group name

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

### Group Operation

#### Instantiation

```java
* Instantiation of groups
* @param groupId	group Id
ITuyaGroup mITuyaGroup= TuyaHomeSdk.newGroupInstance(groupId);
```

#### Group callback event

```java
* Registering group callback event
* @param listener callback
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

* Cancel group callback event
mITuyaGroup.unRegisterGroupListener();
```

#### Sending group control command

```java
* Sending group control command
* @param command	control command
* @param listener callback
mTuyaGroup.publishDps(String command,IControlCallback listener);
```
Example Codes

```java
//Code segment for switching on the light in a group
LampBean bean = new LampBean();
bean.setOpen(true);
HashMap<String, Object> hashMap = new HashMap<>();
hashMap.put(STHEME_LAMP_DPID_1, bean.isOpen());
mTuyaGroup.publishDps(JSONObject.toJSONString(hashMap),callback)；
```
Notes

The returned result of a command sent from a group means that the command is sent to the cloud successfully, and it does not mean that the device has been actually controlled.

#### Group data acquisition

For obtaining the group data locally, the data cannot be obtained before the initialization of Home (invoking getHomeDetail() or getHomeLocalCache).


```java
* Obtaining group data bean locally
* @param groupId    group Id
* @return GroupBean  class of group data
TuyaHomeDataManager.getInstance().getGroupBean(long groupId);

* Obtaining a list of group data locally
* @return  List<GroupBean>  group list
TuyaHomeDataManager.getInstance().getGroupDeviceList(long groupId);
```
#### Group data destruction

```java
//It is recommended to invoke the group data destruction function when exiting the group control page.
mITuyaGroup.onDestroy();
```
