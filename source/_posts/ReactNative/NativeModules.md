---
title: React Native 调用原生方法
date: 2018-06-22 10:18:22
categories: 
    - React Native
tags:
    - React Native
---

> 在使用React Native进行开发的过程中，必不可少的需要在React Native组件里面调用原生的模块，这种调用是非常消耗性能的，而且可能导致很多莫名其妙的问题，所以应该尽量少用。

### 1. IOS调用原生方法

在这里我采用swift来实现原生模块供Js调用
<!--more-->

#### 1.1 新建Module类
新建一RNDemoModule的swift类，创建完后会提示你是否创建Bridging Header的文件，因为IOS开发如果需要swift和oc共存，则swift必须提供一个Bridging Header给oc，否则oc无法调用swift。目前swift无法直接暴露给ReactNative，所以需要oc来做桥接。

在方法和类前面加上`@objc`是为了让swift可以被oc所调用

`RNDemoModule.swift`

    import Foundation
    @objc(RNDemoModule)
    class RNDemoModule: NSObject {
        
        @objc(addEvent:location:)
        func addEvent(name: String, location: String) -> Void {
            // Date is ready to use!
        }
        
        @objc
        func closeCurrentPage(){
             // 由于Js 调用 Native 是异步的，所以有些操作需要回到主线程来执行 
             DispatchQueue.main.async {
                let rnDemoApplication = RnDemoApplication()
                RnDemoApplication.getCurrentController()?.navigationController?.popToRootViewController(animated: true)
            }
        }
        
        @objc
        func constantsToExport() -> [String: Any]! {
            return ["someKey": "someValue"]
        }
        
    }

`RNDemoModule-Bridging-Header.h` 文件中添加一行代码，这是React Bridge的头文件

    #import <React/RCTBridgeModule.h>

#### 1.2 新建OC类

要将Native模块暴露给ReactNative，必须要在oc类型文件中去做，新建一个`RNDemoModuleBridge.m`文件

    #import <Foundation/Foundation.h>
    
    #import <React/RCTBridgeModule.h>
    
    @interface RCT_EXTERN_MODULE(RNDemoModule, NSObject)
    
    RCT_EXTERN_METHOD(addEvent:(NSString *)name location:(NSString *)location)
    
    RCT_EXTERN_METHOD(closeCurrentPage)
    
    @end


#### 1.3 在React Native中调用

Native语言暴露给ReactNative的模块都在NativeModules这个模块上，上面我们暴露了一个`RNDemoModule`模块，因此我们可以在React Native中通过如下方式得到这个模块。

新建一个`RNNativeModules.js`文件，文件的的内容如下

    import {NativeModules} from 'react-native';
    module.exports = NativeModules.RNDemoModule; 
    
在React Native的业务代码中使用

    import RNDemoModule from '.././RNNativeModules.js';
    
    ...
    
    RNDemoModule.closeCurrentPage();
    
### 2. Android调用原生方法

在这里我采用java来实现原生模块供Js调用

#### 1.1 新建Module类

创建一个原生模块类`RNDemoModule.java`，继承`ReactContextBaseJavaModule`类。

 `getName`函数返回一个字符串名字，这个名字在JavaScript端标记这个模块，这个函数必须实现。
 
 `getContants`函数返回需要导出给JavaScript使用的常量。它并不一定需要实现，但在定义一些可以被JavaScript同步访问到的常量值时非常有用。
 
 `show`函数是导出给React Native使用的方法，必须用`@ReactMethod`进行注解，函数的返回类型必须为`void`，React Native的跨语言访问是异步进行的，所以想要给JavaScript返回一个值的唯一办法是使用回调函数或者发送事件
 
  
`RNDemoModule.java`

    package com.rn.demo.rn;
    
    import android.widget.Toast;
    
    import com.facebook.react.bridge.NativeModule;
    import com.facebook.react.bridge.ReactApplicationContext;
    import com.facebook.react.bridge.ReactContext;
    import com.facebook.react.bridge.ReactContextBaseJavaModule;
    import com.facebook.react.bridge.ReactMethod;
    
    import java.util.Map;
    import java.util.HashMap;
    
    public class RNDemoModule extends ReactContextBaseJavaModule {
    
      private static final String DURATION_SHORT_KEY = "SHORT";
      private static final String DURATION_LONG_KEY = "LONG";
    
      public RNDemoModule(ReactApplicationContext reactContext) {
        super(reactContext);
      }
      
     @Override
      public String getName() {
        return "RNDemoModule";
      }
      
      @Override
      public Map<String, Object> getConstants() {
        final Map<String, Object> constants = new HashMap<>();
        constants.put(DURATION_SHORT_KEY, Toast.LENGTH_SHORT);
        constants.put(DURATION_LONG_KEY, Toast.LENGTH_LONG);
        return constants;
      }
      
      @ReactMethod
      public void show(String message, int duration) {
        Toast.makeText(getReactApplicationContext(), message, duration).show();
      }
    }

#### 2.2 注册模块

Android需要将自己写的模块注册后才可以在JavaScript中被访问到，新建一个`RNReactPackage.java`类，继承`ReactPackage`类，在`createNativeModules`方法中添加这个模块



`RNReactPackage.java`

    package com.rn.demo.rn
    
    import com.facebook.react.ReactPackage;
    import com.facebook.react.bridge.NativeModule;
    import com.facebook.react.bridge.ReactApplicationContext;
    import com.facebook.react.uimanager.ViewManager;
    
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.List;
    
    public class RNReactPackage implements ReactPackage {
    
      @Override
      public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
      }
    
      @Override
      public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
    
        modules.add(new RNDemoModule(reactContext));
    
        return modules;
      }
    }
        
这个`RNReactPackage`需要被添加到`ReactInstanceManager`里面，我们在创建`ReactInstanceManager`的地方进行修改

    ReactInstanceManager.builder()
        .setApplication(RNDemoApplication.getInstance())
        .setBundleAssetName("index.android.bundle")
        .setJSMainModulePath("index")
        .addPackage(MainReactPackage())
        .addPackage(RNReactPackage()) // 添加刚刚创建的package
        .setUseDeveloperSupport(BuildConfig.DEBUG)
        .setInitialLifecycleState(LifecycleState.RESUMED)
        .build()    

#### 2.3 在React Native中调用

 调用方式和IOS一样
 
### 3. 通过回调返回值
#### 3.1 IOS通过callback返回

在Native中实现的函数

`RNDemoModule.swift`

    @objc
    func findEvents(callback : RCTResponseSenderBlock){
        let events = ["happy", "every", "day"]
        callback(events)
    }
    
`RNDemoModuleBridge.m`

    RCT_EXTERN_METHOD(findEvents:(RCTResponseSenderBlock)callback)

在Js中获取回调回的信息

    RNDemoModule.findEvents(( events) => {
        this.setState({events: events});
    });

#### 3.2 Android通过callback返回

我们在实现native方法的时候可以通过以下方式返回值

    @ReactMethod
    public void getVersionAndUidByCallback(Callback getVersionAndUidCallback) {

        String versionAndUid = "Callback " + RNDemoApplication
                .getVersionName() + "/" + RNDemoAccountManager.getInstance().getUserId();

        getVersionAndUidCallback.invoke(versionAndUid);
    }


Js调用的时候通过以下方式拿到返回值

    RNDemoModule.getVersionAndUidByCallback((versionAndUid) => {
                            console.log("versionAndUid: " + versionAndUid);
        });
