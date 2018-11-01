# Smart

## Brief Introduction

Smart divides into scene or automation actions. Scene is a condition that users add actions and it is triggered manually; automation action is a condition set by the user, and the set action is automatically executed when the condition is triggered.

In the Tuya smart Android SDK, smart includes the unified management interface of scene or automation actions, TuyaHomeSdk.TuyaHomeSdk.getSceneManagerInstance() and the independent operation interface TuyaHomeSdk.newSceneInstance(String sceneId). The independent operation interface needs to be initialized with the scene id. The scene id can be obtained in the result of [obtaining the scene list interface.](https://github.com/TuyaInc/tuyasmart_home_android_sdk/blob/master/TuyaSmartHomeSdkDemo/doc/tuyahome.md#%23%2310.1)

**In the following documents, manual scene and automated scene are simply referred to as scene.**

## Obtain scene list

**[Description]**

Get the scene list, which can be used when the scene home page is initialized. Determine the manual scene and automation actions through SceneBean's getConditions(); the value is empty when it is a manual scene.

**[Method Prototype]**
```java
/**
 \* Obtain scene list
 \* @param callback
 */
public void getSceneList(ITuyaDataCallback<List<SceneBean>> callback)
Among, the interface of SceneBean is defined as follows

/**
 \* Obtain scene id
 \* @returnScene id
 */
public String getId()

/**
 \*  *  Obtain scene name
 \* @returnScene name
 */
public String getName()

/**
 \*  Obtain scene conditions 
 \* @returnScene condition
 */
public List<SceneCondition> getConditions()

/**
 \*  Obtain scene task
 \* @returnScene task
 */
public List<SceneTask> getActions()

```

**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getSceneList(new ITuyaResultCallback<List<SceneBean>>() {
      @Override
      public void onSuccess(List<SceneBean> result) {
      }

      @Override
      public void onError(String errorCode, String errorMessage) {
      }
});
```
**Automation actions**

User-settable conditions include weather conditions, device status and timer.

- Weather type

Weather conditions include temperature, humidity, weather, PM2.5, air quality, sunrise and sunset, and cities can be freely selected. The weather conditions that can be selected vary depending on the device in the user account.
```java
/**
 \* Create weather conditions
 *
 \* @param place weather city 
 \* @param type condition type
 \* @param rule  condition regulations
 \* @return
 */
public static SceneCondition createWeatherCondition(
      PlaceFacadeBean place, 
      String type,
      Rule rule)
```
Note: The PlaceFacadeBean class object is obtained from the [Obtain City List](https://github.com/TuyaInc/tuyasmart_home_android_sdk/blob/master/TuyaSmartHomeSdkDemo/doc/tuyahome.md#%23%23%2310.2.4), [Obtain City by Latitude and Longitude](https://github.com/TuyaInc/tuyasmart_home_android_sdk/blob/master/TuyaSmartHomeSdkDemo/doc/tuyahome.md#%23%23%2310.2.6) and [obtain City by City ID](https://github.com/TuyaInc/tuyasmart_home_android_sdk/blob/master/TuyaSmartHomeSdkDemo/doc/tuyahome.md#%23%23%2310.2.5) interface. Currently, the acquired urban interface only supports domestic cities.

- Device type

A device condition is a scheduled task for another device or devices triggered by a device, when it is in a certain state. **The same device cannot be used as both a condition and a task to avoid cycle control.**
```java
/**
 \* Create device type conditions
 *
 \* @param devBean  condition device
 \* @param dpId    condition dpId
 \* @param rule    condition regulations
 \* @return
 */
public static SceneCondition createDevCondition(
      SceneDevBean devBean,
      String dpId,
      Rule rule) 
```
Note: SceneDevBean class object can be obtained from the interface of [Obtain the condition device list](https://github.com/TuyaInc/tuyasmart_home_android_sdk/blob/master/TuyaSmartHomeSdkDemo/doc/tuyahome.md#%23%23%2310.2.2).

- Timer

Timer refers that when the specified time is reached, the scheduled task is executed.
```java
/**
  \* Create timer conditions
  \* @param display is used to display the user-selected time conditions.
  \* @param name  Name of Timer condition
  \* @param type condition type
  \* @param rule  condition regulations
  \* @return
  */
  public static SceneCondition createTimerCondition(String display,String name,String type,Rule rule)
```
There are four rules for rule-condition rules:

- Numerical type

Taking temperature as an example, the final expression of the numerical type condition is formatted as "temp > 20". You can get the currently supported temperature maximum, minimum and granularity (stepping) from the Obtain Condition List interface; you can get the supported temperature from the Obtain Condition List. After the configuration is completed on the user interface, the ValueRule.newInstance method is invoked to build the rules, and the rules are used to form the conditions.

For example:

 ValueProperty tempProperty = (ValueProperty) conditionListBean.getProperty();       //Numeric Property

```java
 int max = tempProperty.getMax();       //Maximum value
 int min = tempProperty.getMin();       //Minimum value
 int step = tempProperty.getStep();     //Granularity
 String unit = tempProperty.getUnit();  //Unit
 //The temperature is greater than 20 ℃
 ValueRule tempRule = ValueRule.newInstance(
       "temp",  //Category
       ">",     //operational rule (">", "==", "<")
       20       //Critical value
 ); 
 SceneCondition tempCondition = SceneCondition.createWeatherCondition(
       placeFacadeBean,   //City
       “temp",            //Category
       tempRule           //Rule 
 );
```
- Enumeration type

Taking the weather condition as an example, the final expression of the enumeration type condition is formatted as "condition == rainy". You can get the currently supported weather conditions from the Obtain Condition List interface, including the code and name of each weather condition. After the configuration is completed on the user interface, the EnumRule.newInstancemethod is invoked to build the rules, and the rules are used to form the conditions.

For example:

 EnumProperty weatherProperty = (EnumProperty) conditionListBean.getProperty();  //Enumeration type Property
 ```java
/** 
 {
      {"sunny", " fine day"},
      {"rainy", "rainy day"}
 } 
 */
 HashMap<Object, String> enums = weatherProperty.getEnums();
 //It's rainy.
 EnumRule enumRule = EnumRule.newInstance(
               "condition",  //Category 
               "rainy"        // Enumerated value selected
 );
 SceneCondition weatherCondition = SceneCondition.createWeatherCondition(
               placeFacadeBean,    //City
               "condition",        //Category
               enumRule            //Rule 
 );
 ```
- Boolean type

Boolean types are common in device type conditions, and the final expression is formatted as "dp1 == true". You need to invoke the Obtain Condition Device List interface to obtain a device that supports the configuration of the smart scene, and then query the operations that the device can support based on the device id. See the Obtain Operations Supported by Device for details. After the configuration is completed on the user interface, the BoolRule.newInstancemethod is invoked to build the rules, and the rules are used to form the conditions.

For example:
```java
BoolProperty devProperty = (BoolProperty) conditionListBean.getProperty();  //Boolean type Property
/**
{
  {true, “on"},
  {false, "off"}
}
*/
HashMap<Boolean, String> boolMap = devProperty.getBoolMap(); 

//When the device starts.
BoolRule boolRule = BoolRule.newInstance(
      "dp1",    //"dp" + dpId
      true    //bool of triggering conditions
);
SceneCondition devCondition = SceneCondition.createDevCondition(
      devBean,    //Device 
      "1",        //dpId
      boolRule    //Rule 
);
```
- Timer type

Timer is expressed as a Map type, that is Key: Value. After the user completes the timer configuration, the TimerRule.newInstance is invoked to complete the Map data assembly by the SDK to form the rule condition.

For example:

```java

TimerProperty timerProperty = (TimerProperty)conditionListBean.
 getProperty();	//Timer-type property


 //TimerRule.newInstance provides two construction methods, the difference is whether it will pass the time zone.
 //If it does not pass the time zone, read default time zone.
 /**
  \* 
  \* @param timeZoneId time zone, the format like "Asia/Shanghai”
  \* @param loops 7-digit character string; each digit represents what day of the week; the first digit represents Sunday, and the second digit represents Monday.
  \* By analogy, the character indicates on which days the timer is enabled. 0 indicates not selected; 1 indicates selected, with format like only Monday Selected.
  \* Tuesday: "0110000"。 If they are not selected, it means that the timer is only executed once, with the format: "0000000"
  \* @param time 24-hour system With format like "08:00", 
  \* If the user uses the 12-hour system, the developer needs to convert it to a 24-hour system and upload it.
  \* @param date, format like "20180310"
  \* @return
  */

public static TimerRule newInstance(String timeZoneId,String loops,String time,String date);

 //Create timer rule
 TimerRule timerRule = TimerRule.newInstance("Asia/Shanghai","0111110","16:00","20180310"

 /**

  \* The required parameters have the same meaning as the parameters of the above method, and the default time zone is read.
  \* @param loops
  \* @param time
  \* @param date
  \* @return
  */
 public static TimerRule newInstance(String loops,String time,String date);

 

 /**

  \* Create timer conditions

  \* @param display  is used to display the user-selected time conditions.

  \* @param name  Name of Timer condition

  \* @param type condition type

  \* @param rule  condition regulations

  \* @return

  */

 public static SceneCondition createTimerCondition(String display,String name,String type,Rule rule);

 

 //Create timer conditions, taking the Timer rule constructed above as an example.

 SceneCondition.createTimerCondition(

 "Monday, Tuesday, Wednesday, Thursday & Friday",

 “Timer in work day",

 "timer",

 timerRule

 )

```

### Obtain condition list

**[Interface Description]**

It can be used to get a list of conditions that are currently supported by the user, as a first step in adding or modifying conditions usually.

**[Method Prototype]**
```java
/**
 \* Obtain condition list
 \* @param showFahrenheit shows Fahrenheit degree or not
 \* @param callback
 */
void getConditionList(boolean showFahrenheit,ITuyaResultCallback<List<ConditionListBean>> callback);
Among,  the interface provided by ConditionListBeanis
/**
 \* Obtain condition category
 *
 \* @return category [1]
 */
public String getType() {
      return type;
}·

/**
 \* Obtain condition name
 *
 \* @return name 
 */
public String getName() {
      return name;
}
/**
 \* Obtain Property [2]
 *
 \* @return Property 
 */
public IProperty getProperty() {
      return property;
} 
```
**Note:**

1. Currently supported weather condition categories and their names and Property types

| **Description**  | **Type**   | **Property Type** |
| ---------------- | ---------- | ----------------- |
| Temperature      | Temp       | ValueProperty     |
| Humidity         | humidity   | EnumProperty      |
| Weather          | condition  | EnumProperty      |
| PM2.5            | pm25       | EnumProperty      |
| Air quality      | aqi        | EnumProperty      |
| Sunrise & sunset | sunsetrise | EnumProperty      |
| Timer            | timer      | TimerProperty     |

1. Property is a commonly used data structure in Tuya smart to control devices and other functions. Currently, four properties are available: Regarding Numerical Type, Enumeration Type, Boolean Type, and Timer Type (corresponding to Numeric Type, Enumeration Type and Boolean Type in the condition), each Property is provided with a different access interface. See the rules introduction section above for details.

**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getConditionList(new ITuyaDataCallback<List<ConditionListBean>>() {
      @Override
      public void onSuccess(List<ConditionListBean> conditionActionBeans) {
      }
      @Override
      public void onError(String errorCode, String errorMessage) {
      }
});
```
### Obtain the condition device list

**[Description]**

Obtain a list of devices that can be used for conditional settings.

**[Method Prototype]**
```java
/**

 \* Obtain a list of optional devices in the condition

 \* @param homeId Home id

 \* @param callback

 */

void getConditionDevList(long homeId, ITuyaResultCallback<List<DeviceBean>> callback);
```
**[Example Codes]** 
```java
TuyaHomeSdk.getSceneManagerInstance().getConditionDevList(homeId ,new ITuyaResultCallback<List<DeviceBean>>() {
      @Override
      public void onSuccess(List<DeviceBean> deviceBeans) {
      }
      @Override
      public void onError(String errorCode, String errorMessage) {
      }
});
```
### Obtain device tasks based on device id 

**[Description]**

It is used to obtain the tasks can be selected when selecting the specific trigger conditions of the device.
```java
**[Method Prototype]**

  /**
       \* Obtain a list of task conditions supported by the device
       *
       \* @param devId    device id
       \* @param callback
       */
      void getDeviceConditionOperationList(String devId, ITuyaResultCallback<List<TaskListBean>> callback);
Among, TaskListBean provides interfaces as follows:

/**
 \*  obtain the name of dp points for display interface.
 *
 \* @return name of dp points
 */
public String getName() {
      return name;
}

/**
 \*  Obtain dpId
 *
 \* @return dpId
 */
public long getDpId() {
      return dpId;
}

/**
 \* Obtain the operation configured by the dp point
 *
 \*   Format:
 \*     {
 \*       {true, “on"},
 \*       {false, "off"}
 \*     {
 *
 \* @return 

 */
public HashMap<Object, String> getTasks() {
      return tasks;
}
/**
 \* Obtain the type bool, value, enum, etc. of the condition. 
 */
public String getType() {
      return type;
}
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getDeviceConditionOperationList(
      devId, //Device id
      new ITuyaDataCallback<List<TaskListBean>>() {
          @Override
          public void onSuccess(List<TaskListBean> conditionActionBeans) {
          }
          @Override
          public void onError(String errorCode, String errorMessage) {
          }
});
}
```
### Obtain the city list

**[Description]**

It is used to select a city when creating weather conditions. Note: Only Chinese cities are supported in the current city list for the time being.

**[Method Prototype]**
```java
/**
 User can obtain the city list according to the country code.
 *
 \* @param countryCode Country code
 \* @param callback    callback
 */
void getCityListByCountryCode(String countryCode, ITuyaResultCallback<List<PlaceFacadeBean>> callback);
Among, PlaceFacadeBean category provides interfaces as follows:
/**
 \* Obtain area name
 *
 \* @return Area name
 */
public String getArea() {
      return area;
}

/**
 \* Obtain name of province
 *
 \* @return province name 
 */
public String getProvince() {
      return province;
}
/**
 \* Obtain the city name
 *
 \* @return city name 
 */
public String getCity() {
      return city;
}

/**
 \* Obtain the city id
 *
 \* @return city id
 */
public long getCityId() {
      return cityId;
}
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getCityListByCountryCode(
  	"cn",  //China 
  	new ITuyaResultCallback<List<PlaceFacadeBean>>() {
      	@Override
      	public void onSuccess(List<PlaceFacadeBean> placeFacadeBeans) {

      	}
      	@Override
      	public void onError(String errorCode, String errorMessage) {

      	}
});
```
### Obtain the city information according to the city id.

**[Description]**

Obtain the city information according to the city id to display existing weather conditions. City id can be obtained from the Obtain City List interface.

**[Method Prototype]**
```java
/**
 \* Obtain the city information according to the city id.
 *
 \* @param cityId   cityid{@link PlaceFacadeBean}
 \* @param callback
 */
void getCityByCityIndex(long cityId, ITuyaResultCallback<PlaceFacadeBean> callback);
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getCityByCityIndex(

  	cityId, //City id

  	new ITuyaResultCallback<PlaceFacadeBean>() {

  		@Override

  		public void onSuccess(PlaceFacadeBean placeFacadeBean) {

  		}



  		@Override

  		public void onError(String errorCode, String errorMessage) {

  		}

});
```
### Obtain the city information according to the altitude and longitude of city

**[Description]**

Obtain the city information according to the longitude and latitude to display existing weather conditions.

**[Method Prototype]**
```java
/**
 \* Obtain the city information according to the altitude and longitude of city
 *
 \* @param lon      Longitude
 \* @param lat      Latitude
 \* @param callback
 */
void getCityByLatLng(String lon, String lat, ITuyaResultCallback<PlaceFacadeBean> callback);
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getCityByLatLng(
      String.valueOf(longitude), //Longitude 
      String.valueOf(latitude),   //Latitude 
      new ITuyaResultCallback<PlaceFacadeBean>() {
          @Override
          public void onSuccess(PlaceFacadeBean placeFacadeBean) {
          }
          @Override
          public void onError(String errorCode, String errorMessage) {
          }

});
```
### Scene action

A scene action refers to a control device action executed when a condition is triggered. Actions that can be executed by manual scene include automated scene and smart device, and actions that can be executed by automated scene include manual scene, other automated scene and smart devices. The tasks that users can set vary depending on the user’s device. Please note that not every product supports the scene.

**Create action**

**[Description]**

It is used to create scene actions.

**[Method Prototype]**
```java
/**
 \* It is used to create scene actions.
 *
 \* @param devId device id
 \* @param tasks to be implemented task format: { dpId: dp point value}
 \*                          For example:
 \*                          {
 \*                              "1": true,
 \*                          }
 \* @returnScene action
 */
public static SceneTask createDpTask(@NonNull String devId, HashMap<String, Object> tasks)
```
**[Example Codes]**
```java
HashMap<String, Object> taskMap = new HashMap<>();
taskMap.put("1", true); //Turn on the device
SceneTask task = SceneTask.createDpTask(
      devId,      //Device id
      taskMap     //Device action
);
```
### A list of devices supported by obtaining execution action

**[Description]**

Obtain a list of devices supporting scene actions for selecting to add to the action to be executed.

**[Method Prototype]**
```java
/**
 \* Obtain a list of optional devices in the action
 \* @param homeId Home id
 \* @param callback
 */
void getTaskDevList(long homeId, ITuyaResultCallback<List<DeviceBean>> callback);
Among, **DeviceBean****provides interfaces as follows:**
/**
 \*  Obtain name of Device
 \* 
 \* @return Device name 
 */
public String getName() {

      return name;

}



/**

 \*  Product ID

 \* 

 \* @return product id

 */

public String getProductId() {

      return productId;

}



/**

 \*  Obtain id of device

 \* 

 \* @return device id

 */

public String getDevId() {

      return devId;

}



/**

 \*  Obtain icon of device

 \* 

 \* @return icon address

 */

public String getIconUrl() {

       return iconUrl;

 }
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getTaskDevList(new ITuyaResultCallback<List<DeviceBean>>() {
      @Override
      public void onSuccess(List<DeviceBean> deviceBeans) {
      }
      @Override
      public void onError(String errorCode, String errorMessage) {
      }

});
```
### Obtain executable actions based on device id

**[Description]**

It is used to obtain the tasks executed by the device when creating an action. The device id can be obtained from the [a list of devices supported by obtaining execution action](https://github.com/TuyaInc/tuyasmart_home_android_sdk/blob/master/TuyaSmartHomeSdkDemo/doc/tuyahome.md#%23%23%2310.3.1)

**[Method Prototype]**
```java
/**
\* Obtain what the device can execute
\* @param devId    device id
\* @param callback
*/
void getDeviceTaskOperationList(String devId, ITuyaResultCallback<List<TaskListBean>> callback);
Among, TaskListBean provides interfaces as follows:
/**
 \*  obtain the name of dp points for display interface.
 *
 \* @return name of dp points
 */
public String getName() {
    return name;
}



/**

 \*  Obtain dpId

 *

 \* @return dpId

 */

public long getDpId() {

    return dpId;

}



/**
 \* Obtain the operation configured by the dp point
 *
 \*   Format:
 \*     {
 \*       {true, “on"},
 \*       {false, "off"}
 \*     }
 *
 \* @return 
 */

public HashMap<Object, String> getTasks() {
    return tasks;
}
/**
 \* Obtain the type bool, value, enum, etc. of the condition.
*/
public String getType() {
    return type;

}
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().getTaskList(
    devId,      //Device id
    new ITuyaResultCallback<List<TaskListBean>>() {
        @Override
        public void onSuccess(List<TaskListBean> conditionActionBeans) {
        }
       @Override
        public void onError(String errorCode, String errorMessage) {
       }

});
```
**Create Scene**

**[Description]**

It is used to assemble conditions and actions into a scene and create a new scene, and then returns the scene data after success. There are two creation methods, and the only difference is whether there are stickyOnTop parameters.

**[Method Prototype]**
```java
/**
 \* Create Scene
 *
 \* @param homeId	  Home id
 \* @param name      Scene name 
 \* @param stickyOnTop shows in the home page or not
 \* @param conditions Scene triggering conditions {@link SceneCondition}
 \* @param tasks     Scene task {@link SceneTask}
 \* @param matchType Scenario condition and/or relationship  SceneBean.MATCH_TYPE_OR represents that it satisfies any conditions, default value; SceneBean.MATCH_TYPE_AND represents that it satisfies all conditions
 \* @param callback   callback

 */

public void createScene(long homeId, String name, boolean stickyOnTop, String background, List<SceneCondition> conditions, List<SceneTask> tasks,int matchType, final ITuyaResultCallback<SceneBean> callback) ;

/**
 \* Create Scene
 *
 \* @param homeId	  Home id
 \* @param name      Scene name 
 \* @param conditions Scene triggering conditions {@link SceneCondition}
 \* @param tasks     Scene task {@link SceneTask}
 \* @param matchType Scenario condition and/or relationship  SceneBean.MATCH_TYPE_OR represents that it satisfies any conditions, default value; SceneBean.MATCH_TYPE_AND represents that it satisfies all conditions
 \* @param callback   callback
 */

public void createScene(long homeId, String name, String background, List<SceneCondition> conditions, List<SceneTask> tasks,int matchType, final ITuyaResultCallback<SceneBean> callback) ;
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().createScene(
	 "100001"
    “Morning", // Scene name
    true,  //Show it in the home page or not
    conditions, //condition
    tasks,     //task
    SceneBean.MATCH_TYPE_AND, //Implement condition type
    new ITuyaResultCallback<SceneBean>() {
        @Override
        public void onSuccess(SceneBean sceneBean) {
            Log.d(TAG, "createScene Success");
        }
        @Override
        public void onError(String errorCode, String errorMessage) {
        }
});
```
### 10.5 Modify the scene

**[Description]**

It is used to modify the scene. After it succeeded, it will return to new scene data.
```java
**[Method Prototype]**

/**
 \* Modify the scene
 \* 
 \* @param sceneReqBean Scene data class
 \* @param callback
 */

void modifyScene(SceneBean sceneReqBean, ITuyaResultCallback<SceneBean> callback);
```
Note: The interface can only be used to modify the scene. Do not transmit into the newly created SceneBean object.

**[Example Codes]**
```java
sceneBean.setName("New name");  //Change Scene name 
sceneBean.setConditions(Collections.singletonList(condition)); //Change Scene condition
sceneBean.setActions(tasks); //Change Scene action
String sceneId = sceneBean.getId();  //Get the Scene id to initialize
TuyaHomeSdk.newSceneInstance(sceneId).modifyScene(
    sceneBean,  //Modified Scene data class
    new ITuyaResultCallback<SceneBean>() {
        @Override
        public void onSuccess(SceneBean sceneBean) {
            Log.d(TAG, "Modify Scene Success");
        }
        @Override
        public void onError(String errorCode, String errorMessage) {
        }

});
```
### Execute scene

**[Description]**

It is used to execute manual scene.

**[Method Prototype]**
```java
/**
 \* Perform scene actions
 *
 \* @param callback
 */
void executeScene(IResultCallback callback);
```
**[Example Codes]**
```java
String sceneId = sceneBean.getId();  
TuyaHomeSdk.newSceneInstance(sceneId).executeScene(new IResultCallback() {
    @Override
    public void onSuccess() {
        Log.d(TAG, "Excute Scene Success");
    }
    @Override
    public void onError(String errorCode, String errorMessage) {
    }
});
```
###  Delete scene

**[Description]**

For deleting scene

**[Method Prototype]**
```java
/**
 \* Delete Scene
 *
 \* @param callback
 */
void deleteScene(IResultCallback callback);
```
**[Example Codes]**
```java
String sceneId = sceneBean.getId();  
TuyaHomeSdk.newSceneInstance(sceneId).deleteScene(new 
IResultCallback() {
    @Override
    public void onSuccess() {
        Log.d(TAG, "Delete Scene Success");
    }
    @Override
    public void onError(String errorCode, String errorMessage) {
    }

});
```
### Turn on and turn off the automation scene

**[Description]**

It is used to turn on or off the automatic scene.

**[Method Prototype]**
```java
/**
 \* Turn on automation scene
 \* @param sceneId  
 \* @param callback
 */
void enableScene(String sceneId, final IResultCallback callback);

/**
 \* Turn off the automation scene
 \* @param sceneId  
 \* @param callback
 */
void disableScene(String sceneId, final IResultCallback callback);
```
**[Example Codes]**
```java
String sceneId = sceneBean.getId();  
TuyaHomeSdk.newSceneInstance(sceneId).enableScene(sceneId,new 
IResultCallback() {
    @Override
    public void onSuccess() {
       Log.d(TAG, "enable Scene Success");
    }
    @Override
    public void onError(String errorCode, String errorMessage) {
    }

});



TuyaHomeSdk.newSceneInstance(sceneId).disableScene(sceneId,new 

IResultCallback() {
    @Override
    public void onSuccess() {
        Log.d(TAG, "disable Scene Success");
    }
    @Override
    public void onError(String errorCode, String errorMessage) {
    }

});
```
### Sort scene

**[Description]**

Manual scene or automation scene sorting. Note: Manual or automation scenes can only be sorted separately and cannot be shuffled.

**[Method Prototype]**
```java
/**
 \* Scene sorting
 \* @param homeId family id
 \* @param sceneIds  Sorted id list for manual or automation scenes 
 \* @param callback    callback
 */
void sortSceneList(long homeId, List<String> sceneIds, IResultCallback callback)
```
**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().sortSceneList(
    homeId, //Home list
    sceneIds,//Scene id list
    new IResultCallback() {
        @Override
        public void onSuccess() {
        }
        @Override
        public void onError(String errorCode, String errorMessage) {
        }

});
```
### Erase

**[Description]**

If the user exits the activity of the scene, the scene destruction method should be invoked to reclaim the memory and enhance the experience.

**[Example Codes]**
```java
TuyaHomeSdk.getSceneManagerInstance().onDestroy();
TuyaHomeSdk.newSceneInstance(sceneId).onDestroy();
```