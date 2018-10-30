### 修改备注名

#### (1)修改发出的分享人的备注名

##### 【描述】

分享者修改发出的分享人的备注名.即你收到其他用户分享给你的设备，可以修改他们的备注名.(发出的分享)

##### 【代码调用】

```java
* @param memberId     用户成员Id 从SharedUserInfoBean中获取
* @param name   			  要修改的备注名
void renameShareNickname(long memberId, String name, IResultCallback callback);
```

##### 【代码范例】

```java
TuyaHomeSdk.getDeviceShareInstance().renameShareNickname(memberId, name, new IResultCallback() {
    @Override
    public void onError(String code, String error) {
        
    }

    @Override
    public void onSuccess() {

    }
})
```

#### (2)修改接收到的分享人的备注名

##### 【描述】

分享者修改接收到的分享人的备注名.即你分享设备给其他人，可以修改他们的备注名.（接收到的分享）

##### 【方法调用】

```java
* @param memberId     用户成员Id 从SharedUserInfoBean中获取
* @param name    		 要修改的备注名 
void renameReceivedShareNickname(long memberId, String name, IResultCallback callback);
```

##### 【代码范例】

```java
TuyaHomeSdk.getDeviceShareInstance().renameReceivedShareNickname(memberId, name, new IResultCallback() {
    @OverrideTuyaHomeSdk.getDeviceShareInstance
    public void onError(String code, String error) {
        
    }

    @Override
    public void onSuccess() {

    }
})
```

------

## 