## Mesh设备操作
ITuyaBlueMeshDevice类提供了对Mesh设备的操作

### Mesh设备判断方法
##### 【代码范例】

```
DeviceBean deviceBean=TuyaHomeSdk.getDataInstance().getDeviceBean(mDevId);
if(deviceBean.isBleMesh()){
    L.d(TAG, "This device is mesh device");
 }else{
 
}

```

### 大小类介绍
#####  Mesh产品目前分为五大类
	  灯大类(01): 1-5路RGBWC彩灯
	  电工类(02)：1-6路插座
	  传感器类(04)：门磁、PIR（传感类主要是一些周期性上报的传感数据）
	  执行器类(10)：马达、报警器之类用于执行的设备
	  适配器(08)：网关（带有mesh及其他通信节点的适配器）
#####  小类编号
	  1-5路灯（01-05）
	  1-6路排插（01-06)
	  .....

#####  举例
```
四路灯  		0401
五路插座		0502
......

```


###  控制指令下发
#####  【指令格式】
发送控制指令按照以下格式： {"(dpId)":"(dpValue)"}  

##### 【方法调用】
```
//控制设备
* @param nodeId   子设备本地编号
* @param pcc		  设备产品大小类
* @param dps		  控制指令
* @param callback  回调
void publishDps(String nodeId, String pcc, String dps, IResultCallback callback);


//控制群组
* @param localId   群组本地编号
* @param pcc		  产品大小类
* @param dps		  控制指令
* @param callback  回调
void multicastDps(String localId, String pcc, String dps, IResultCallback callback)
```

##### 【代码范例】
```
//设备控制
String dps = {"1":false};
ITuyaBlueMeshDevice mTuyaBlueMeshDevice=TuyaHomeSdk.newBlueMeshDeviceInstance("meshId");
mTuyaBlueMeshDevice.publishDps(devBean.getNodeId(), devBean.getCategory(), dps, new IResultCallback() {
            @Override
            public void onError(String s, String s1) {
            		Toast.makeText(mContext, "发送失败"+ errorMsg, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onSuccess() {
            		Toast.makeText(mContext, "发送成功", Toast.LENGTH_LONG).show();
            }
        });
        
        
//群组控制        
String dps = {"1":false};
ITuyaBlueMeshDevice mTuyaBlueMeshDevice= TuyaHomeSdk.newBlueMeshDeviceInstance("meshId");
mTuyaBlueMeshDevice.multicastDps(groupBean.getLocalId(), devBean.getCategory(), dps, new IResultCallback() {
            @Override
            public void onError(String errorCode, String errorMsg) {
            		Toast.makeText(mContext, "发送失败"+ errorMsg, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onSuccess() {
            		Toast.makeText(mContext, "发送成功", Toast.LENGTH_LONG).show();
            }
        });

```
###  数据监听
##### 【描述】
mesh网内相关信息（dp数据、状态变更、设备名称、设备移除）会实时同步到 IMeshDevListener 

##### 【实现回调】
```
mTuyaBlueMeshDevice.registerMeshDevListener(new IMeshDevListener() {

		/**
         * 数据更新
         * @param nodeId    更新设备的nodeId
         * @param dps       dp数据
         * @param isFromLocal   数据来源 true表示从本地蓝牙  false表示从云端
         */
            @Override
            public void onDpUpdate(String nodeId, String dps,boolean isFromLocal) {
                //可以通过node来找到相对应的DeviceBean
                DeviceBean deviceBean = mTuyaBlueMeshDevice.getMeshSubDevBeanByNodeId(nodeId);
            }

		 /**
         * 设备状态的上报
         * @param online    在线设备列表
         * @param offline   离线设备列表
         * @param gwId      状态的来源 gwId不为空表示来自云端（gwId是上报数据的网关Id）   为空则表示来自本地蓝牙
         */
            @Override
            public void onStatusChanged(List<String> online, List<String> offline,String gwId) {

            }
            
        /**
         * 网络状态变化
         * @param devId
         * @param status
         */
            @Override
            public void onNetworkStatusChanged(String devId, boolean status) {

            }
            

        /**
         * raw类型数据上报
         * @param bytes
         */
            @Override
            public void onRawDataUpdate(byte[] bytes) {

            }
            

        /**
         * 设备信息变更（名称等）
         * @param bytes
         */            
            @Override
            public void onDevInfoUpdate(String devId) {

            }
            
        /**
         * 设备移除
         * @param devId
         */
            @Override
            public void onRemoved(String devId) {

            }
        });
```




### 创建群组
##### 【指令格式】
mesh网内支持创建 28672 个群组  返回时id范围 8000 ~ EFFF  （16进制）  由本地进行维护
#####  【方法调用】
```
* @param name			群组名称
* @param pcc			群组中设备的大小类  (支持跨小类创建  FF01 表示覆盖灯大类)
* @param localId		群组的localId  (范围 8000 ~ EFFF 16进制字符串)
* @param callback		回调
public void addGroup(String name, String pcc, String localId,IAddGroupCallback callback);
```

##### 【代码范例】
```
mITuyaBlueMesh.addGroup("群组名称","大小类", "8001", new IAddGroupCallback() {
			@Override
            public void onError(String errorCode, String errorMsg) {
            		Toast.makeText(mContext, "创建群组失败"+ errorMsg, Toast.LENGTH_LONG).show();
            }
            
            	
            @Override
            public void onSuccess(long groupId) {
            		Toast.makeText(mContext, "创建群组成功", Toast.LENGTH_LONG).show();
            }
        
        });
```

###  子设备重命名
##### 【方法调用】
```
* @param devId    	 设备Id
* @param name		  重命名名称
* @param callback	  回调
public void renameMeshSubDev(String devId, String name, IResultCallback callback);

```

##### 【代码范例】
```
 mTuyaBlueMesh.renameMeshSubDev(devBean.getDevId(),"设备名称", new IResultCallback() {
            @Override
            public void onError(String code, String errorMsg) {
            		Toast.makeText(mContext, "重命名失败"+ errorMsg, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onSuccess() {
            		Toast.makeText(mContext, "重命名成功", Toast.LENGTH_LONG).show();
            }
        });
```

###  子设备移除
#####  【方法调用】
```
* @param devId    	 设备Id
* @param pcc  		 设备大小类
* @param callback	  回调
public void removeMeshSubDev(String devId, String pcc, IResultCallback callback) ;

```
#####  【代码范例】
```
mTuyaBlueMesh.removeMeshSubDev(devBean.getDevId(),devBean.getCategory(), new IResultCallback() {
            @Override
            public void onError(String code, String errorMsg) {
            		Toast.makeText(mContext, "子设备移除失败 "+ errorMsg, Toast.LENGTH_LONG).show();
    
            }

            @Override
            public void onSuccess() {
            		Toast.makeText(mContext, "子设备移除成功", Toast.LENGTH_LONG).show();
            }
        });
```

### 单个子设备信息查询
##### 【说明】
云端获取到的dp点数据可能不是当前设备实时的数据，可以通过该命令去查询设备的当前数据值，结果通过IMeshDevListener的onDpUpdate方法返回

#####  【方法调用】
```
* @param pcc  		 设备大小类
* @param nodeId    	 设备nodeId
* @param callback	  回调
public void querySubDevStatusByLocal(String pcc, final String nodeId, final IResultCallback callback);

```

#####  【代码范例】
```
 mTuyaBlueMeshDevice.querySubDevStatusByLocal(devBean.getCategory(), devBean.getNodeId(), new IResultCallback() {
            @Override
            public void onError(String code, String errorMsg) {
            		Toast.makeText(mContext, "查询失败 "+ errorMsg, Toast.LENGTH_LONG).show();
            }

            @Override
            public void onSuccess() {
            		Toast.makeText(mContext, "查询成功 ", Toast.LENGTH_LONG).show();
            }
        });
```