## 一、上传用户头像
**接口描述**

用于上传用户自定义的头像。

```java
/**
 * 上传用户头像
 * @param file   用户头像图片文件
 * @param callback 回调
 */
void uploadUserAvatar(File file, IBooleanCallback callback)
```
**代码范例**

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
## 二、设置温度单位
**接口描述**

设置温度单位是摄氏度还是华氏度

```java
/**
 * TempUnitEnum.Celsius是摄氏度,TempUnitEnum.Fahrenheit是华摄度
 * @param unit
 * @param callback
 */
void setTempUnit(TempUnitEnum unit, IResultCallback callback);

```


## 三、退出登录接口

用户账号切换的时候需要调用退出登录接口

**代码范例**

```java
       TuyaHomeSdk.getUserInstance().logout(new ILogoutCallback() {
            @Override
            public void onSuccess() {
            	//退出登录成功
            }

            @Override
            public void onError(String errorCode, String errorMsg) {
            }
        });

```
## 四、注销账户

**接口描述**

一周后账号才会永久停用并删除以下你账户中的所有信息，在此之前重新登录，则你的停用请求将被取消

```java

/**
 * 注销账户
 * Account cancellation
 * @param callback
 */
void cancelAccount(IResultCallback callback);
    
```
**代码范例**

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