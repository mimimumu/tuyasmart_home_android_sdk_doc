## Home Management Class

The ITuyaHomeManager provides changes related to creating home, obtaining the home list and monitoring home, 

and it can be obtained by invoking the TuyaHomeSdk.getHomeManagerInstance().
## Create Home
```java
/**
*
* @param name     Name of home
* @param lon      Longitude
* @param lat      Latitude
* @param geoName  Name of geographic position
* @param rooms    Room list
* @param callback
*/

void createHome(String name, double lon, double lat, String geoName, List<String> rooms, ITuyaHomeResultCallback callback);
```

## Obtain Home List
```java
/**
* @param callback
*/

void queryHomeList(ITuyaGetHomeListCallback callback);
```

## Change of home information
```java
/**
* Change of registered home information includes:
* adding and removing homes, information change, change of shared lists and monitoring of successfully server connection.
*
* @param listener
*/

void registerTuyaHomeChangeListener(ITuyaHomeChangeListener listener);

/**
* Deregistered the information Change of a home.
*
* @param listener
*/
void unRegisterTuyaHomeChangeListener(ITuyaHomeChangeListener listener);

public interface ITuyaHomeChangeListener {
/**
* Adding home succeeds Used for data synchronization of multiple devices
*
* @param homeId
*/
void onHomeAdded(long homeId);
/**
* Removing home succeeds Used for data decarbonization of multiple devices
*
* @param homeId
*/
void onHomeRemoved(long homeId);
/**
* Change of home information
Used for data decarbonization of multiple devices
*
* @param homeId
*/
void onHomeInfoChanged(long homeId);
/**
* Release the change of device list Used for data decarbonization of multiple devices
*
* @param sharedDeviceList
*/
void onSharedDeviceList(List<DeviceBean> sharedDeviceList);
/**
*
* The mobile phone has been connected successfully to the Tuya Cloud Server. Special attention shall be paid to this message. In some cases, the local data may be inconsistent with the data of servers, the getHomeDetail interface of Home can be invoked to update data. 
*
*/
 void onServerConnectSuccess();
}
```

## Operate Cache Data of a Home

Before obtaining the cache data, the initiation interface of home, getHomeDetail or getHomeLocalCache, shall be invoked first.

```java
public interface ITuyaHomeDataManager {
/**
* Devices, groups and room list of a home
*/
List<RoomBean> getHomeRoomList(long homeId);
/**
* Obtain the device list of a home.
*
* @param homeId home ID
* @return
*/
List<DeviceBean> getHomeDeviceList(long homeId);
/**
* Obtain the group list of a home.
*
* @param homeId
* @return
*/
List<GroupBean> getHomeGroupList(long homeId);
// Obtain group
GroupBean getGroupBean(long groupId);
// Obtain devices
DeviceBean getDeviceBean(String devId);
// Obtain room according to the group ID.
RoomBean getGroupRoomBean(long groupId);
// Obtain room
RoomBean getRoomBean(long roomId);
// Obtain room information based on devices
RoomBean getDeviceRoomBean(String devId);
// Obtain device list of a group
List<DeviceBean> getGroupDeviceList(long groupId);
// Obtain the group list of a mesh
List<GroupBean> getMeshGroupList(String meshId);
List<DeviceBean> getMeshDeviceList(String meshId);
/**
* Obtain the following device list of a room according to the room ID.
*
* @param roomId
* @return
*/
List<DeviceBean> getRoomDeviceList(long roomId);

/**
* Obtain the following group list of a room according to the room ID.
*
* @param roomId
* @return
*/
List<GroupBean> getRoomGroupList(long roomId);
/**
* Obtain the data of home.
*
* @param homeId
* @return
*/
HomeBean getHomeBean(long homeId);
}
```