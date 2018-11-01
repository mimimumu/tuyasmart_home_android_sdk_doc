## Gateway

The gateway class encapsulates the operations of the ZigBee gateway, including control, querying sub-devices and monitoring sub-device status. The gateway can be initialized via TuyaHomeSdk.newGatewayInstance().

```java
public interface ITuyaGateway {
     /**
      \* Send command
      *
      \* @param dps
      \* @param callback
      */
     void publishDps(String localId, String dps, IResultCallback callback);
    /**
      \* Broadcast control device
      *
      \* @param dps
      \* @param callback
      */
     void broadcastDps(String dps, IResultCallback callback);
     /**
      \* Multicast control device
      *
      \* @param localId
      \* @param dps
      \* @param callback
      */
     void multicastDps(String localId, String dps, IResultCallback callback);
     /**
      \* Obtain the sub-device of a gateway
      *
      \* @param callback
      */
     void getSubDevList(ITuyaDataCallback<List<DeviceBean>> callback);
     /**
      \* Change of registered sub-device information 
      *
      \* @param listener
      */
     void registerSubDevListener(ISubDevListener listener);
     /**
     \* Change of logout sub-device information
      */
     void unRegisterSubDevListener();
}
```
