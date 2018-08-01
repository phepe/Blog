---
title: React Native 集成到现有项目中
date: 2018-06-21 15:25:01
categories: 
    - React Native
tags:
    - React Native
---

### 1. 创建新的项目结构

新建一个目录，里面包含如下目录和文件

* ios : 将现有的IOS工程原封不动的Copy到这个目录下
* android : 将现有的Android工程原封不动的Copy到这个目录下
* index.js : React Native的入口文件
* package.json : React Native的配置文件
* scr : 存放React Native代码
* images : 存放React Native中需要用到到图片

<!--more-->

### 2. 配置项目

在`package.json`文件中做一些简单的配置

    {
      "name": "RNDemo",
      "version": "0.0.1",
      "private": true,
      "scripts": {
        "start": "node node_modules/react-native/local-cli/cli.js start"
      }
    }

然后通过`yarn`来添加React Natvie工程需要的模块

    yarn add react-native
    yarn add react@version_printed_above    //  根据react-natvie的版本添加react
    
还可以直接在`package.json`文件中添加React Native相关的依赖模块

    {
      "name": "RNDemo",
      "version": "0.0.1",
      "private": true,
      "scripts": {
        "start": "node node_modules/react-native/local-cli/cli.js start"
      },
      ,
      "dependencies": {
        "react": "^16.3.0-alpha.1",
        "react-native": "0.54",
      }
    }
    
然后在项目目录下执行以下命令进行安装 
    
    yarn install

### 3. 编写测试页面

修改`index.js`文件，添加一个`<Text>`组件，测试环境是否OK
   
 
    import React from 'react';
    import {AppRegistry, StyleSheet, Text, View} from 'react-native';
    
    class RNDemo extends React.Component {
      render() {
        return (
          <View style={styles.container}>
            <Text style={styles.hello}>Hello, World</Text>
          </View>
        );
      }
    }
    var styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
      },
      hello: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
      },
    });    
    // Module name
    AppRegistry.registerComponent('RNDemo', () => RNDemo);
 
    
### 4. 集成到IOS项目中
#### 4.1 导入React Native模块

我们采用CocoaPods来集成React Native，在把React Native集成到你的应用中之前，首先要决定具体整合的是React Native框架中的哪些部分。而这就是`subspec`要做的工作。在创建Podfile文件的时候，需要指定具体安装哪些React Native的依赖库，所指定的每一个库就称为一个`subspec`。

可用的subspec都列在node_modules/react-native/React.podspec中，基本都是按其功能命名的。
进入到`/ios`子目录下,使用命令创建Podfile文件

    pod init
    
编辑Podfile文件,根据需要加入各种`subspec`

    # The target name is most likely the name of your project.
    target 'RNDemo' do
    
      # Your 'node_modules' directory is probably in the root of your project,
      # but if not, adjust the `:path` accordingly
      pod 'React', :path => '../node_modules/react-native', :subspecs => [
        'Core',
        'CxxBridge', # Include this for RN >= 0.47
        'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
        'RCTText',
        'RCTNetwork',
        'RCTWebSocket', # Needed for debugging
        'RCTAnimation', # Needed for FlatList and animations running on native UI thread
        # Add any other subspecs you want to use in your project
      ]
      # Explicitly include Yoga if you are using RN >= 0.42.0
      pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'
    
      # Third party deps podspec link
      pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
      pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
      pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'
    
    end
    
创建好`Podfile`后，执行命令，将对应的React Native模块导入到项目中

    pod install
    
#### 4.2 打开测试页面
    
用XCode打开`.xcworkspace`结尾的项目，添加一个按钮来跳转到我们刚刚编写的React Native测试页面,在`ViewController.swift`添加按钮的Action

    import React
    
    ...
    
    // MARK: Actions
    @IBAction func goRNPage(_ sender: UIButton) {
        let jsCodeLocation = URL(string: "http://localhost:8081/index.bundle?platform=ios")
    
        let rootView = RCTRootView(
            bundleURL: jsCodeLocation,
            moduleName: "RNDemo",
            initialProperties: nil,
            launchOptions: nil
        )
        let vc = UIViewController()
        vc.view = rootView
        self.present(vc, animated: true, completion: nil)
    }

在项目目录下运行命令启动包服务器

    yarn start

用Xcode运行项目，如果一切顺利，就可以点击按钮，跳转到用React Native编写的页面了

#### 4.3 可能会遇到各种问题

由于React Native还没有发布正式版本，目前对Swift的支持有一些问题。所以在集成的时候很有可能会编译失败。

1. RCTValueAnimatedNode.h file not found

    在`package.json`里面加入
     
        "postinstall": "sed -i '' 's\/#import <RCTAnimation\\/RCTValueAnimatedNode.h>\/#import \"RCTValueAnimatedNode.h\"\/' ./node_modules/react-native/Libraries/NativeAnimation/RCTNativeAnimatedNodesManager.h"
        
    然后在项目目录下运行以下命令
    
        yarn run postinstall
2. ‘fishhook/fishhook.h’ file not found

    在`package.json`里面加入
     
    "fishhookinstall": "sed -i '' 's#<fishhook/fishhook.h>#\"fishhook.h\"#g' ./node_modules/react-native/Libraries/WebSocket/RCTReconnectingWebSocket.m"
        
    然后在项目目录下运行以下命令
    
        yarn run postinstall
    返回ios目录运行
    
        pod install

3. 'algorithm' file not found

    在`/ios`目录下的`Podfile`文件末位加入修复脚步，修改后的`Podfile`如下
    
        # The target name is most likely the name of your project.
        target 'RNDemo' do
            # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
            use_frameworks!

            # Pods for RNDemo

            # Your 'node_modules' directory is probably in the root of your project,
                # but if not, adjust the `:path` accordingly
                pod 'React', :path => '../node_modules/react-native', :subspecs => [
                    'Core',
                    'CxxBridge', # Include this for RN >= 0.47
                    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
                    'RCTText',
                    'RCTNetwork',
                    'RCTWebSocket', # needed for debugging
                    'RCTImage',
                    # Add any other subspecs you want to use in your project
                ]
                # Explicitly include Yoga if you are using RN >= 0.42.0
                pod "yoga", :path => "../node_modules/react-native/ReactCommon/yoga"

                # Third party deps podspec link
                pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
                pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
                pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'

        end

        def fix_cplusplus_header_compiler_error
            filepath = '../node_modules/react-native/React/Base/Surface/SurfaceHostingView/RCTSurfaceSizeMeasureMode.h'

            contents = []

            file = File.open(filepath, 'r')
            file.each_line do | line |
                contents << line
            end
            file.close

            if contents[32].include? "&"
                contents.insert(26, "#ifdef __cplusplus")
                contents[36] = "#endif"

                file = File.open(filepath, 'w') do |f|
                    f.puts(contents)
                end
            end
        end

        def fix_unused_yoga_headers
            filepath = './Pods/Target Support Files/yoga/yoga-umbrella.h'

            contents = []

            file = File.open(filepath, 'r')
            file.each_line do | line |
                contents << line
            end
            file.close

            if contents[12].include? "Utils.h"
                contents.delete_at(15) # #import "YGNode.h"
                contents.delete_at(15) # #import "YGNodePrint.h"
                contents.delete_at(15) # #import "Yoga-internal.h"
                contents.delete_at(12) # #import "Utils.h"

                file = File.open(filepath, 'w') do |f|
                    f.puts(contents)
                end
            end
        end

        def react_native_fix
            fix_cplusplus_header_compiler_error
            fix_unused_yoga_headers
        end

        post_install do |installer|
            react_native_fix
        end

    然后关闭Xcode工程,在`/ios`目录下执行update

        pod update
    
    重新打开Xcode工程，运行app

4. Native module cannot be null

    当引入`react-navigation`，IOS项目会报错：Native module cannot be null，这个时候需要在 podfile 中加入：RCTLinkingIOS
    
        pod 'React', :path => '../node_modules/react-native', :subspecs => [
          'Core',
          'CxxBridge', # Include this for RN >= 0.47
          'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
          'RCTText',
          'RCTNetwork',
          'RCTWebSocket', # needed for debugging
          'RCTImage',
          'RCTLinkingIOS'
          # Add any other subspecs you want to use in your project
        ]
    然后关闭Xcode工程,在`/ios`目录下执行update

        pod update
    
    重新打开Xcode工程，运行app
    
    错误原因：react-navigation会生成一个原生的RCTLinkingIOS库，外部引用的需要手动导入，内部的自动链接
    
    
### 5. 集成到Android项目中
#### 5.1 导入React Native模块

在`android/app`目录下的`build.gradle`文件中添加React Native依赖

     dependencies {
         ...
         compile "com.facebook.react:react-native:+" // From node_modules.
     }

在`android`目录下的`build.gradle`文件中添加一个`maven`依赖的入口，，必须写在 "allprojects" 代码块中

    allprojects {
        repositories {
            ...
            maven {
                // All of React Native (JS, Android binaries) is installed from npm
                url "$rootDir/../node_modules/react-native/android"
            }
        }
        ...
    }

#### 5.2 配置权限

在`AndroidManifest.xml`文件中加入网络权限

    <uses-permission android:name="android.permission.INTERNET" />

为了方便调试以及显示错误信息，加入开发者菜单界面`DevSettingsActivity`，同样配置在`AndroidManifest.xml`文件中，正式发布的时候可以去掉

    <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
    
由于我们调试的时候需要`悬浮窗(overlay)` 权限，而Android 6.0以上版本需要动态申请这个权限，所以我们需要在主`Activity`中添加如下代码

    private final int OVERLAY_PERMISSION_REQ_CODE = 11111;

    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {

      ...

      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
      if (!Settings.canDrawOverlays(this)) {
          Intent intent = new Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION,
                                     Uri.parse("package:" + getPackageName()));
          startActivityForResult(intent, OVERLAY_PERMISSION_REQ_CODE);
        }
      }

    }


    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == OVERLAY_PERMISSION_REQ_CODE) {
          if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
              if (!Settings.canDrawOverlays(this)) {
                  // SYSTEM_ALERT_WINDOW permission not granted...
            }
        }
      }
    }



#### 5.3 打开测试页面

在项目中新增一个Activity

    public class RNDemoActivity extends Activity implements DefaultHardwareBackBtnHandler {
        private ReactRootView mReactRootView;
        private ReactInstanceManager mReactInstanceManager;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            mReactRootView = new ReactRootView(this);
            mReactInstanceManager = ReactInstanceManager.builder()
                .setApplication(getApplication())
                .setBundleAssetName("index.android.bundle")
                .setJSMainModulePath("index")
                .addPackage(new MainReactPackage())
                .setUseDeveloperSupport(BuildConfig.DEBUG)
                .setInitialLifecycleState(LifecycleState.RESUMED)
                .build();
        // The string here (e.g. "MyReactNativeApp") has to match
        // the string in AppRegistry.registerComponent() in index.js
            mReactRootView.startReactApplication(mReactInstanceManager, "RNDemo", null);

            setContentView(mReactRootView);
        }

        @Override
        public void invokeDefaultOnBackPressed() {
            super.onBackPressed();
        }
    
    
        @Override
        protected void onPause() {
            super.onPause();
        
            if (mReactInstanceManager != null) {
                mReactInstanceManager.onHostPause(this);
            }
        }
    
        @Override
        protected void onResume() {
            super.onResume();
        
            if (mReactInstanceManager != null) {
                mReactInstanceManager.onHostResume(this, this);
            }
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
        
            if (mReactInstanceManager != null) {
                mReactInstanceManager.onHostDestroy(this);
            }
            if (mReactRootView != null) {
                mReactRootView.unmountReactApplication();
            }
        }
        
        @Override
         public void onBackPressed() {
            if (mReactInstanceManager != null) {
                mReactInstanceManager.onBackPressed();
            } else {
                super.onBackPressed();
            }
        }
        
        @Override
        public boolean onKeyUp(int keyCode, KeyEvent event) {
            if (keyCode == KeyEvent.KEYCODE_MENU && mReactInstanceManager != null) {
                mReactInstanceManager.showDevOptionsDialog();
                return true;
            }
            return super.onKeyUp(keyCode, event);
        }
    }

同时要在`AndroidManifest.xml`文件中进行注册

    <activity
                android:name=".rn.RNDemoActivity"
                android:label="@string/app_name"
                android:theme="@style/AppTheme.NoActionBar.FullScreen"/>
                
在项目中通过调用如下代码，跳转到测试页面

    startActivity(new Intent(this, rn.RNDemoActivity));
                
项目目录下运行命令启动包服务器

    yarn start

然后通过Android Studio 编译运行              





    
 

