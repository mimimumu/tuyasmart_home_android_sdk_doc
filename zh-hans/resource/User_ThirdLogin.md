###  第三方登陆

#### Twitter登陆

##### 【方法调用】

```java
/**
* @param countryCode 国家区号
* @param key         twitter授权登录获取的key
* @param secret      twitter授权登录获取的secret
*/

TuyaHomeSdk.getUserInstance().loginByTwitter(String countryCode, String key, String secret, ILoginCallback callback);
```


#### QQ登陆

##### 【方法调用】

```java
/**
* @param countryCode 国家区号
* @param userId         QQ授权登录获取的userId
* @param accessToken    QQ授权登录获取的accessToken
*/

TuyaHomeSdk.getUserInstance().loginByQQ(String countryCode, String userId, String accessToken, ILoginCallback callback);
```

#### 微信登陆

##### 【方法调用】

```java
/**
 *  @param countryCode 国家区号
 *  @param code 微信授权登录获取的code
 */
TuyaHomeSdk.getUserInstance().loginByWechat(String countryCode, String code, ILoginCallback callback);

```

#### Facebook 登陆

```java
/**
 *  @param countryCode 国家区号
 *  @param token facebook授权登录获取的token
 */
TuyaHomeSdk.getUserInstance().loginByFacebook(String countryCode, String token, ILoginCallback callback);
```