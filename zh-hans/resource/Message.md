## 消息中心

### 获取消息列表

##### 【描述】

用于获取全部消息列表。

##### 【方法原型】

```java
/**
 *  获取消息列表
 *
 * @param callback 回调
 */
void getMessageList(ITuyaDataCallback<List<MessageBean>> callback);
```

其中， `MessageBean`提供以下接口：

```java
/**
 * 获取日期和时间 格式: 2017-09-08 17:12:45
 *
 * @return 日期和时间
 */
public String getDateTime() {
    return dateTime;
}

/**
 * 获取消息图标URL
 *
 * @return 消息图标URL
 */
public String getIcon() {
    return icon;
}

/**
 * 获取消息类型名称(如果是告警类型消息则为dp点名称)
 *
 * @return 消息类型名称
 */
public String getMsgTypeContent() {
    return msgTypeContent;
}

/**
 *  获取消息内容, 可用于界面展示
 *
 * @return 消息内容
 */
public String getMsgContent() {
    return msgContent;
}

/**
 * 获取消息类型
 * 0: 系统消息
 * 1: 有新的设备
 * 2: 有新的好友
 * 4: 设备告警
 *
 * @return 消息类型
 */
public int getMsgType() {
    return msgType;
}

/**
 * 获取设备id
 * 注： 只有告警类型消息才会有该字段
 *
 * @return 设备ID
 */
public String getMsgSrcId() {
    return msgSrcId;
}

/**
 *  获取消息id
 *
 * @return 消息id
 */
public String getId() {
    return id;
}
```

##### 【代码范例】

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

### 删除消息

##### 【描述】

用于批量删除消息。

##### 【方法原型】

```java
/**
 *  删除消息
 *
 * @param mIds 要删除的消息的id列表
 * @param callback 回调
 */
void deleteMessage(List<String> mIds, IBooleanCallback callback);
```

##### 【代码范例】

```java
List<String> deleteList = new ArrayList<>();  
deleteList.add(messageBean.getId());  //取要删除的消息的id添加到列表中
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

## 