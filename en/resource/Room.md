## Room Management Class

The ITuyaRoom provides the room management class, and adding devices, removing devices or groups are handled in this class. The TuyaHomeSdk.newRoomInstance("homeId") can be used to build the room management class. 

**RoomBean Field Information:**
```java
    private long roomId;//The id of the room
    private String name;//The name of the room
    private List<DeviceBean> deviceList; //The device in the room
    private List<GroupBean> groupList;   //The group in the room
    private int displayOrder;//Room order
```

**Interface:**
```java
/**
* Update room name
*
* @param name     New name of room
* @param callback
*/
void updateRoom(String name, IResultCallback callback);
/**
* Add device
*
* @param devId
* @param callback
*/
void addDevice(String devId, IResultCallback callback);

/**
* Add group
*
* @param groupId
* @param callback
*/
void addGroup(long groupId, IResultCallback callback);
/**
* Remove device
*
* @param devId
* @param callback
*/
void removeDevice(String devId, IResultCallback callback);
/**
* Remove group
*
* @param groupId
* @param resultCallback
*/
void removeGroup(Long groupId, IResultCallback resultCallback);

/**
* Remove groups or devices from a room.
* @param list
* @param callback
*/
void moveDevGroupListFromRoom(List<DeviceAndGroupInRoomBean> list, IResultCallback callback);

/**
* Sort groups or devices in a room.
* @param list
* @param callback
*/
void sortDevInRoom(List<DeviceAndGroupInRoomBean> list, IResultCallback callback);
```