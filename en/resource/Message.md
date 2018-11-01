# Message center

## Obtain message list

**[Description]**

It is used to obtain lists of all messages.

**[Method Prototype]**
```java
/**
 \* Obtain message list
 *
 \* @param callback
 */
void getMessageList(ITuyaDataCallback<List<MessageBean>> callback);

Among,  MessageBeanprovides the following interfaces:

/**
 \* Obtain date and time  Format: 2017-09-08 17:12:45
 *
 \* @return date and time
 */
public String getDateTime() {
     return dateTime;
}
/**
 \* Obtain message icon URL
 *
 \* @return Message icon URL
 */
public String getIcon() {
     return icon;
}
/**
 \* Obtain the message type name (if it is alarm type message, then dp points name are available.)
 *
 \* @return message class name 
 */
public String getMsgTypeContent() {
     return msgTypeContent;
}

/**
 \* Obtain the message content for interface display.
 *
 \* @return Message content 
 */
public String getMsgContent() {

     return msgContent;

}

/**
 \* obtain message type
 \* 0: System Messages
 \* 1: With new device
 \* 2: With new friends
 \* 4: Device alarm
 *
 \* @return message class 
 */

public int getMsgType() {
     return msgType;
}

/**
 \*  Obtain id of device
 Note: This field is only available for alarm type messages.
 *
 \* @return device ID
 */
public String getMsgSrcId() {
     return msgSrcId;
}

/**
 \* obtain message id
 *
 \* @return message id
 */
public String getId() {
     return id;
}
```
**[Example Codes]**
```java
TuyaHomeSdk.getMessageInstance().getMessageList(new ITuyaDataCallback<List<MessageBean>>() {
     @Override
     public void onSuccess(List<MessageBean> result) {
     }
     @Override
     public void onError(String errorCode, String errorMessage) {
     }
});
```
## Delete messages

**[Description]**

It is used to delete messages in bulk.

**[Method Prototype]**
```java
/**
 \*  Delete messages
 *
 \* @param mIds to be deleted message id list
 \* @param callback
 */
void deleteMessage(List<String> mIds, IBooleanCallback callback);
```
**[Example Codes]**
```java
List<String> deleteList = new ArrayList<>();  
deleteList.add(messageBean.getId());  //add the id of the targeted message into the list

TuyaMessage.getInstance().deleteMessages(
     deleteList, 
     new IBooleanCallback() {
         @Override
         public void onSuccess() {       
         }
         @Override
         public void onError(String errorCode, String errorMessage) {

         }
});
```