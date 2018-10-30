### 用户手机验证码登陆

涂鸦智能提供手机验证码登陆体系。
#### (1)手机验证码登陆
##### 【描述】

手机验证码登陆，支持国内手机验证码登陆。
##### 【代码调用】

```java
/**获取手机验证码
* @param countryCode   国家区号
* @param phoneNumber   手机号码
*/
TuyaHomeSdk.getUserInstance().getValidateCode(String countryCode, String phoneNumber, final IValidateCallback callback);
/** 手机验证码登陆
* @param countryCode 国家区号
* @param phone       电话
* @param code        验证码
* @param callback    登陆回调接口
*/
TuyaHomeSdk.getUserInstance().loginWithPhone(String countryCode, String phone, String code, final ILoginCallback callback)
```

##### 【代码范例】

```java
//获取手机验证码
TuyaHomeSdk.getUserInstance().getValidateCode("86","13666666666", new IValidateCallback(){
    @Override
    public void onSuccess() {
        Toast.makeText(mContext, "获取验证码成功", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, "code: " + code + "error:" + error, Toast.LENGTH_SHORT).show();
    }
});
//手机验证码登陆
TuyaHomeSdk.getUserInstance().loginWithPhone("86", "13355555555", "123456", new ILoginCallback() {
    @Override
    public void onSuccess(User user) {
        Toast.makeText(mContext, "登录成功，用户名：" +TuyaHomeSdk.getUserInstance().getUser().getUsername(), Toast.LENGTH_SHORT).show();
    }
    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, error, Toast.LENGTH_SHORT).show();
    }
});
```
### 用户手机密码登陆

涂鸦智能提供手机密码登陆体系。
#### (1)手机密码注册
##### 【描述】

手机密码注册。支持国内外手机注册。
##### 【方法调用】

```java
//获取手机验证码
* @param countryCode   国家区号
* @param phoneNumber   手机号码
TuyaHomeSdk.getUserInstance().getValidateCode(String countryCode, String phoneNumber, final IValidateCallback callback);

//注册手机密码账户
* @param countryCode 国家区号
* @param phone       手机密码
* @param passwd      登陆密码
* @param callback    登陆回调接口
TuyaHomeSdk.getUserInstance().registerAccountWithPhone(final String countryCode, final String phone, final String passwd, final String code, final IRegisterCallback callback);
```
##### 【代码范例】

```java

//获取手机验证码
TuyaHomeSdk.getUserInstance().getValidateCode("86","13666666666", new IValidateCallback(){
    @Override
    public void onSuccess() {
        Toast.makeText(mContext, "获取验证码成功", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, "code: " + code + "error:" + error, Toast.LENGTH_SHORT).show();
    }
 });
//注册手机密码账户
TuyaHomeSdk.getUserInstance().registerAccountWithPhone("86","13666666666","123456","124332", new IRegisterCallback() {
    @Override
    public void onSuccess(User user) {
        Toast.makeText(mContext, "注册成功", Toast.LENGTH_SHORT).show();
    }
    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, "code: " + code + "error:" + error, Toast.LENGTH_SHORT).show();
    }
});
```
#### (2)手机密码登陆
##### 【描述】

手机密码登陆。
##### 【方法调用】

```java
//手机密码登陆
* @param countryCode 国家区号
* @param phone       手机密码
* @param passwd      登陆密码
* @param callback    登陆回调接口
TuyaHomeSdk.getUserInstance().loginWithPhonePassword(String countryCode, String phone, String passwd, final ILoginCallback callback);
```
##### 【代码范例】

```java
//手机密码登陆
TuyaHomeSdk.getUserInstance().loginWithPhonePassword("86", "13666666666", "123456", new ILoginCallback() {
    @Override
    public void onSuccess(User user) {
        Toast.makeText(mContext, "登录成功，用户名：" +TuyaHomeSdk.getUserInstance().getUser().getUsername(), Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, "code: " + code + "error:" + error, Toast.LENGTH_SHORT).show();
    }
});
```
#### (3)手机重置密码
##### 【描述】

手机密码登陆 重置密码。
##### 【方法调用】

```java
//获取手机验证码
* @param countryCode   国家区号
* @param phoneNumber   手机号码
TuyaHomeSdk.getUserInstance().getValidateCode(String countryCode, String phoneNumber, final IValidateCallback callback);

//重置密码
* @param countryCode 国家区号
* @param phone       手机号码
* @param code        手机验证码
* @param newPasswd   新密码
* @param callback    重置手机密码回调接口
TuyaHomeSdk.getUserInstance().resetPhonePassword(final String countryCode, final String phone, final String code, final String newPasswd, final IResetPasswordCallback callback);

```
##### 【代码范例】

```java
//手机获取验证码
TuyaHomeSdk.getUserInstance().getValidateCode("86", "13555555555", new IValidateCallback() {
    @Override
    public void onSuccess() {
        Toast.makeText(mContext, "获取验证码成功", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, "code: " + code + "error:" + error, Toast.LENGTH_SHORT).show();
    }
});
//重置手机密码
TuyaHomeSdk.getUserInstance().resetPhonePassword("86", "13555555555", "123456", "123123", new IResetPasswordCallback(){
    @Override
    public void onSuccess() {
        Toast.makeText(mContext, "找回密码成功", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onError(String code, String error) {
        Toast.makeText(mContext, "code: " + code + "error:" + error, Toast.LENGTH_SHORT).show();
    }
});
```