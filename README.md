#百度鹰眼cordova插件 cordova-baidu-yingyan

百度鹰眼是集轨迹追踪、轨迹数据存储、轨迹处理与查询的一套完整轨迹管理开放服务，通过覆盖多平台的SDK、API以及强大的云端服务，帮助开发者轻松开发人员和车辆轨迹追踪系统。
使用百度鹰眼，您可以轻松开发出一套适用于车队监控、外业人员监管、儿童防丢领域的轨迹管理产品。

[百度的鹰眼系统](http://lbsyun.baidu.com/index.php?title=yingyan)

# 下载安装
`cordova plugin add https://github.com/taofengno1/cordova-plugin-baiduyy.git`

# 配置
## android

需要在config.xml里添加下面的配置来配置您的AK：

```
config.xml:

  <platform name="android">
    <config-file target="AndroidManifest.xml" parent="./application">
      <meta-data
        android:name="com.baidu.lbsapi.API_KEY"
        android:value="你的android AK" />
    </config-file>
  </platform>
```

# js API

* 设置打包间隔和传间隔

  ```
  cordova.baiduTrace.setInterval(gatherInterval, packInterval).then(successCb, errorCb)

    gatherInterval：打包间隔，单位：秒
    packInterval：上传间隔，单位：秒
  ```

* 设置定位方式（只有android支持）
  ```
  cordova.baiduyingyan.setLocationMode(locationMode).then(successCb, errorCb)

    locationMode:
          * 0：高精度定位模式，GPS与网络综合定位
          * 1：低功耗定位模式，仅使用网络定位(WiFi和基站定位)
          * 2：仅使用设备(GPS)定位
  ```


* 设置上传用的协议（只有android支持）
  ```  
  cordova.baiduTrace.setProtocolType(type).then(successCb, errorCb)

    type: 获取定位的方式
        * 0：https类型
        * 1：http类型
  ```

* 开始采集
  ```
    cordova.baiduTrace.startTrace(entityName, serviceId, operationMode, customAttr)
      .then(successCb, errorCb)

      entityName: 实例名
      serviceId: 服务ID
      operationMode: 操作模式
        * 0 : 不上传位置数据，也不接收报警信息
        * 1 : 不上传位置数据，但接收报警信息
        * 2 : 上传位置数据，且接收报警信息
      customAttr： 自定义属性, 见下面的栗子
  ```

* 停止采集

  ```
    cordova.baiduTrace.stopTrace().then(successCb, errorCb)
  ```

# 栗子：

```
      cordova.baiduTrace.setInterval(2, 10).then(function() {
          console.log('setInterval OK');
      });

      cordova.baiduTrace.setLocationMode(0).then(function(){
        console.log('setLocationMode OK');
      });

      cordova.baiduTrace.setProtocolType(0).then(function(){
        console.log('setProtocolType OK');
      });

      cordova.baiduTrace.startTrace('cordova-test', '118XXX', '2',
        {
          'type': 'system',
          'workerId': 'senior'
        }).then(function () {
        // 调用成功
        console.log('leon', '开始')
        alert('开始收集')
      }, function (error) {
        // 调用
        alert('收集失败' + error);
      });

      cordova.baiduyingyan.stopTrace().then(function(msg) {
        alert('停止收集成功：'+msg)
      }, function(error) {
        alert('停止收集失败：'+error)
      });
```