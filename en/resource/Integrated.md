# Integrate SDK

# Preparation for Integration

### (1) Create Project

Build your project in the Android Studio.

### (2) Introduce the aar Package

![pastedGraphic.png](./images/android_import_aar.jpg)

### (3) Configure the build.gradle

Add the following codes in the build.gradle file.

```groovy
defaultConfig {
    ndk {
        abiFilters "armeabi-v7a", "x86"
    }

    gradle
    repositories {
        flatDir {
            dirs 'libs'
        }
    }
    dependencies {

        compile 'com.alibaba:fastjson:1.1.67.android'
        compile 'com.squareup.okhttp3:okhttp-urlconnection:3.6.0'

// compile 'de.greenrobot:eventbus:2.4.0'  Deprecated in 2.7.2

        compile 'io.reactivex.rxjava2:rxandroid:2.0.1'
        compile 'io.reactivex.rxjava2:rxjava:2.1.7'
        compile 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
// compile 'org.eclipse.paho:org.eclipse.paho.android.service:1.1.1' Deprecated in 2.7.2
        compile(name: 'tuyasmart-x.x.x', ext: 'aar')
    }


    android {
        lintOptions {
            abortOnError false
            disable 'InvalidPackage'
        }

        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_7
            targetCompatibility JavaVersion.VERSION_1_7
        }
        packagingOptions {
            exclude 'META-INF/LICENSE.txt'
            exclude 'META-INF/NOTICE'
            exclude 'META-INF/LICENSE'
            exclude 'META-INF/NOTICE.txt'
            exclude 'META-INF/INDEX.LIST'
            exclude 'META-INF/services/javax.annotation.processing.Processor'
        }
    }
```

**[Note]**

The Tuya Smart sdk solely supports the platform of armeabi-v7a and x86 architecture by default. Developer may refer to the [GitHub](https://github.com/TuyaInc/tuyasmart_android_sdk/tree/master/library) if you need other platforms. 

### (4) Set the AndroidManifest.xml

Set appkey and appSecret in the AndroidManifest.xml file, and configure corresponding permissions, etc.

```xml

<meta-data
	android:name="TUYA_SMART_APPKEY"
	android:value="App id" />
<meta-data
	android:name="TUYA_SMART_SECRET"
	android:value="App key" />

Add essential permission support.

<!-- sdcard -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" android:required="false"/>

<!-- Network -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.WAKE_LOCK"android:required="false" />

Add necessary service and receiver.

//<service android:name="org.eclipse.paho.android.service.MqttService" />
//from 2.6.3 com.tuya.smart.mqtt.MqttService replace the org.eclipse.paho.android.service.mqttservice
    <service android:name="com.tuya.smart.mqtt.MqttService" android:stopWithTask="true"/>
<receiver android:name="com.tuya.smart.android.base.broadcast.NetworkBroadcastReceiver">
<intent-filter>
<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
</intent-filter>
</receiver>
<service

android:name="com.tuya.smart.android.hardware.service.GwBroadcastMonitorService"
android:exported="true"
android:label="UDPService"
android:process=":monitor">
<intent-filter>
<action android:name="tuya.intent.action.udp" />


<category android:name="tuya" />
</intent-filter>
</service>
<service
   android:name="com.tuya.smart.android.hardware.service.DevTransferService"
   android:label="TCPService"
    <intent-filter>
    	<action android:name="tuya.intent.action.tcp" />
           <category android:name="tuya" />
        </intent-filter>
</service>

```
### (5) Aliasing Configuration

Arrange aliasing configuration in corresponding proguard-rules.pro files. 

```bash 
#fastJson
-keep class com.alibaba.fastjson.**{*;}
-dontwarn com.alibaba.fastjson.**

#netty
-keep class io.netty.** { *; }
-dontwarn io.netty.**

#mqtt
-keep class org.eclipse.paho.client.mqttv3.** { *; }
-dontwarn org.eclipse.paho.client.mqttv3.**

-dontwarn okio.**
-dontwarn rx.**
-dontwarn javax.annotation.**
-keep class com.squareup.okhttp.** { *; }
-keep interface com.squareup.okhttp.** { *; }
-keep class okio.** { *; }
-dontwarn com.squareup.okhttp.**

-keep class com.tuya.**{*;}
-dontwarn com.tuya.**

```



## Use the ADK function in codes

The TuyaHomeSdk is the outbound interface of the Smart Home, and the operations include network configuration, initiation, control, room, group and ZigBee.

### (1) Initiate Tuya Smart sdk in the Application

**[Description]**

It is used to initiate components of communication services, etc.

**[Example Codes]**
```java
public class TuyaSmartApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        TuyaHomeSdk.init(this);
    }

}
```

**Note:**

The appId and appSecret need to be configured in the AndroidManifest.xml file or the build environment or the codes. 

### (2) Lou out of the Tuya Smart Cloud connection

The following interface needs to be invoked to log out of the App. 
```java
TuyaHomeSdk.onDestroy();
```

### (3) Monitor the invalidity of registered session 

**[Description]**

Because of abnormality or long-time absence of operation for 45 or more days, the session will become invalid, and user has to log out of the App and log in again to obtain the session. 

**[Method Invocation]**

```java
// Monitor the invalidity of session.
TuyaHomeSdk.setOnNeedLoginListener(INeedLoginListener needLoginListener);
```
**[Callback]**

```java
needLoginListener.onNeedLogin(Context context);
```
**[Example Codes]**
```java
public class TuyaSmartApp extends Application {

        @Override
        public void onCreate() {
            super.onCreate();
            // Register in the App.
  			  TuyaHomeSdk.setOnNeedLoginListener(new INeedLoginListener(){
     		  @Override
      		  public void onNeedLogin(Context context) {

      		  }
    });
```
[**Note**]

- In case of this kind of callback, please go to the login page and require the user to log in again. 
---