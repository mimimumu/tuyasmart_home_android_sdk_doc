## Upload the user account picture

**[Description]**

It is for user to upload his/her self-defined account image. 

**[Method Prototype]**
```java
/**
 * Upload user account picture
 * @param file     user account picture file
 * @param callback
 */
void uploadUserAvatar(File file, IBooleanCallback callback)
```

**[Example Codes]**
```java
TuyaHomeSdk.getUserInstance().uploadUserAvatar(
    file, 
    new IBooleanCallback() {
        @Override
        public void onSuccess() {
        }
        @Override
        public void onError(String code, String error) {

        }
});
```
## Set the Unit of Temperature

Select the centigrade or Fahrenheit.
```java
/**
 * TempUnitEnum.Celsius and TempUnitEnum.Fahrenheit *
 * @param unit
 * @param callback
 */
void setTempUnit(TempUnitEnum unit, IResultCallback callback);
```
## Log Out of the Login Interface

The logout interface has to be invoked for account switching. 
```java
TuyaHomeSdk.getUserInstance().logout(new ILogoutCallback() {
    @Override
    public void onSuccess() {
        // Logout succeeds
    }
    @Override
    public void onError(String errorCode, String errorMsg) {

    }
});
```


## Deregister Account

After one week, the account will be permanently disabled, and all information in the account will be deleted. If you log in to the account again before it is permanently disabled, your deregistration will be canceled.
```java
/**
* Deregister account
* Account cancellation
* @param callback
*/

void cancelAccount(IResultCallback callback);
```
**[Example Codes]**
```java
TuyaHomeSdk.getUserInstance().cancelAccount(new IResultCallback() {
    @Override
    public void onError(String code, String error) {
    }
    @Override
    public void onSuccess() {
    }
});
```