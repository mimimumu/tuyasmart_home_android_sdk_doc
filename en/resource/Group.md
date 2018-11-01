# Group Management

The Tuya Cloud supports the group management system. User can create group, change group name, manage devices of group, manage multiple devices via the group and dismiss group.

# Creating a Group

**[Description]**

Tuya Smart provides some interfaces for device group control. The group control herein refers to the WiFi group, and currently, only the group control function is available. The group function is disabled by default. If necessary, please contact our staff.

**[Method Invocation]**

**[Example Codes]**
```java
// Creating a group

TuyaHomeSdk.newHomeInstance("homeId").createNewGroup("productId", "name", devIds, new ICreateGroupCallback() {
    @Override
    public void onSuccess(long groupId) {
// Returnjing groupId
    }
    @Override
    public void onError(String errorCode, String errorMsg) {
    }
});
```
**[Notes]**

Groups cannot be created by default. If this function is required, please contact us to enable it.

## Group List Acquisition

**[Description]**

This interface is mainly to obtain the latest group list from the cloud.

**[Method Invocation]**
```java
\* @param productId    product id (obtained from DeviceBean.getProductId)
\* @param 	 callback
TuyaHomeSdk.newHomeInstance("homeId").getGroupDevList(String productId,IGetDevsFromGroupByPidCallback callback)
```
**[Example Codes]**
```java
//Obtaning the group list from the cloud
TuyaHomeSdk.newHomeInstance("homeId").queryDeviceListToAddGroup("productId", new IGetDevsFromGroupByPidCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> bizResult) {
    }
    @Override

   public void onError(String errorCode, String errorMsg) {

    }

});
```
## Group Operation

**[Description]**

The group operation of Tuya Smart is mainly based on the operation of the main device, and the main device refers to the first device in the current group online. Online/offline status, and data reporting rely on changes of the main device. Sending control commands is applicable to all devices of the group.

**[Instantiation]**
```java
\* Instantiation of groups
\* @param groupId	group Id
ITuyaGroup mITuyaGroup= TuyaHomeSdk.newGroupInstance(groupId);
```
### (1) Modifying group name**

**[Description]**

Modify group name.

**[Method Invocation]**
```java
\* Modifying group name
\* @param groupId    group Id
\* @param 	 callback
mITuyaGroup.updateGroupName(IControlCallback callback)
```
**[Example Codes]**
```java
// Renaming group

mITuyaGroup.updateGroupName(new IControlCallback() {
    @Override
    public void onSuccess() {
    }
    @Override
    public void onError(String s, String s1) {
    }

});
```
### (2) Modifying group device list

**[Description]**

Modify group device list.

**[Method Invocation]**
```java
\* Modifying group device list
\* @param devIds   devIds of device
\* @param 	 callback
mITuyaGroup.updateGroupRelations(List<String> devIds,IControlCallback callback);
```
**[Example Codes]**
```java
//Modifying group device list

mITuyaGroup.updateGroupRelations(getSelectedDeviceIds, new IControlCallback() {
    @Override
    public void onError(String errorCode, String errorMsg) {

    }
    @Override
    public void onSuccess() {
    }
});
```
### (3) Dismissing group

**[Description]**

Dismiss group.

**[Method Invocation]**
```java
\* Dismissing group
\* @param groupId    group Id
\* @param 	 callback
mITuyaGroup.dismissGroup(IControlCallback callback)
```
**[Example Codes]**
```java
// Deleting group.

mITuyaGroup.dismissGroup(new IControlCallback() {
    @Override
    public void onSuccess() {
    }

    @Override
    public void onError(String s, String s1) {
    }
});
```
### (4) Group callback event

**[Description]**

Group callback event

**[Method Invocation]**
```java
\* Registering group callback event
\* @param listener callback
mITuyaGroup.registerGroupListener(IGroupListener listener);
\* Cancel group callback event
mITuyaGroup.unRegisterGroupListener();
```
**[Example Codes]**
```java
\* Registering group callback event

mITuyaGroup.registerGroupListener(new IGroupListener() {
    // Updating data points of master device
    @Override
    public void onDpUpdate(String dps) {
    }
    // Change of online status of master device
    @Override
    public void onStatusChanged(boolean status) {
    }
    // Change of network status
    @Override
    public void onNetworkStatusChanged(boolean isNetworkOk) {
    }

});

// Cancelling group callback event

mITuyaGroup.unRegisterGroupListener();
```
### (5) Sending group control command

**[Description]**

Send group control command.

**[Method Invocation]**
```java
\* Sending group control command
\* @param command	control command
\* @param listener callback
mTuyaGroup.publishDps(String command,IControlCallback listener);
```
**[Example Codes]**
```java
//Code segment for switching on the light in a group
LampBean bean = new LampBean();
bean.setOpen(true);
HashMap<String, Object> hashMap = new HashMap<>();
hashMap.put(STHEME_LAMP_DPID_1, bean.isOpen());
mTuyaGroup.publishDps(JSONObject.toJSONString(hashMap),callback)；
```
**[Notes]**

The returned result of a command sent from a group means that the command is sent to the cloud successfully, and it does not mean that the device has been actually controlled.

### (6) Obtaining device list of a group

**[Description]**

Obtain the device list of a group from the cloud.

**[Method Invocation]**
```java
\* @param listener callback
mTuyaGroup.getGroupDevList(IGetDevicesInGroupCallback listener)
```
**[Example Codes]**
```java
// Obtaining device list of a group

mITuyaGroup.getGroupDevList(new IGetDevicesInGroupCallback() {
    @Override
    public void onSuccess(List<GroupDeviceBean> list) {
    }

    @Override
    public void onError(String errorCode, String errorMsg) {

    }

});
```
### (7) Group data acquisition

**[Description]**

For obtaining the group data locally, the data cannot be obtained before the initialization of Home (invoking getHomeDetail() or getHomeLocalCache).

**[Method Invocation]**
```java
\* Obtaining group data bean locally
\* @param groupId    group Id
\* @return GroupBean  class of group data
TuyaHomeDataManager.getInstance().getGroupBean(long groupId);

\* Obtaining a list of group data locally
\* @return  List<GroupBean>  group list
TuyaHomeDataManager.getInstance().getGroupDeviceList(long groupId);
```
### (8) Group data destruction

\* It is recommended to invoke the group data destruction function when exiting the group control page.
```java
mITuyaGroup.onDestroy();
```