---
title: React Navigation使用说明
date: 2018-06-21 10:20:52
categories: 
    - React Native
tags:
    - React Native
---
### 1. 创建项目

创建包含Native的工程，运行。

    react-native init ReduxNavigation
    cd ReduxNavigation
    react-native run-ios --simulator "iPhone X"

<!--more-->

### 2. 集成React Navigation

#### 2.1 创建一个 stack navigator

在项目中安装react-navigation这个包，可以参考[React Navigation官方指南](https://reactnavigation.org/zh-Hans/)

    yarn add react-navigation

使用`createStackNavigator`创建一个 `stack navigator`, 它需要`a route configuration object`（一个路由配置对象） 和 `an options object`（一个可选对象）. 由于`createStackNavigator`函数会返回一个React组件，因此我们可以直接从`App.js`中导出它以用作我们应用程序的根组件。

    const RootStack = createStackNavigator(
      {
        Home: HomeScreen,
        Details: DetailsScreen,
      },
      {
        initialRouteName: 'Home',
      }
    );
    export default class App extends React.Component {
      render() {
        return <RootStack />;
      }
    }

#### 2.2 页面切换

跳转到新的页面，如果你已经在`Details`那个页面上，那么它不会做任何事情。

    <Button
          title="Go to Details"
          onPress={() => this.props.navigation.navigate('Details')}
        />

多次导航到同一路由的方法

    <Button
      title="Go to Details... again"
      onPress={() => this.props.navigation.push('Details')}
    />

要点：

* ` this.props.navigation.navigate('RouteName') `将新路由推送到堆栈导航器，如果它尚未在堆栈中，则跳转到该页面。如果你已经在那个页面上，那么意味着它不会做任何事情。

* 我们可以多次调用` this.props.navigation.push('RouteName') `，来多次导航到同一路由。

* 如果当前页面可以执行返回操作，标题栏会自动显示返回按钮，但你可以通过调用` this.props.navigation.goBack() `以编程方式返回。 在Android上，硬件返回按钮会按预期工作。

* 您可以使用` this.props.navigation.navigate('RouteName') `返回堆栈中的现有页面，你可以使用` this.props.navigation.popToTop() `返回堆栈中的第一个页面。

#### 2.3 传递参数给路由

* 将参数包装成一个对象，作为navigation.navigate方法的第二个参数传递给路由
    
        <Button
              title="Go to Details"
               => {
                this.props.navigation.navigate('Details', {
                  itemId: 86,
                  otherParam: 'anything you want here',
                });
              }}
            />

* 读取页面组件中的参数的方法：this.props.navigation.state.params

        const { navigation } = this.props;
        const itemId = navigation.getParam('itemId', 'NO-ID');
        const otherParam = navigation.getParam('otherParam', 'default');

也可以直接使用this.props.navigation.state.params访问 params 对象。 如果没有提供参数，这可能是null，所以使用getParam通常更容易，不必处理这种情况。

#### 2.4 配置标题栏

每个页面组件可以有一个名为`navigationOptions`的静态属性，它是一个对象或一个返回包含各种配置选项的对象的函数。 我们用于设置标题栏的标题的是title这个属性，如以下示例所示。

    class HomeScreen extends React.Component {
      static navigationOptions = {
        title: 'Home',
      };
    
      /* render function, etc */
    }
    
    class DetailsScreen extends React.Component {
      static navigationOptions = {
        title: 'Details',
      };
    
      /* render function, etc */
    }

跨页面共享通用的`navigationOptions`

    const RootStack = createStackNavigator(
        {
            Home: {
                screen: HomeScreen,
            },
            Details: {
                screen: DetailsScreen,
            },
        },
        {
            initialRouteName: 'Home',
    
            navigationOptions: {
                headerStyle: {
                    backgroundColor: '#f4511e',
                },
                headerTintColor: '#fff',
                headerTitleStyle: {
                    fontWeight: 'bold',
                },
            },
        }
    );

我们也可以覆盖共享的`navigationOptions`,你的页面组件上指定的`navigationOptions`与其父级 `stack navigator` 中的`navigationOptions`一起合并时，页面组件上的选项优先。 

    class DetailsScreen extends React.Component {
      static navigationOptions = ({ navigation, navigationOptions }) => {
        const { params } = navigation.state;
    
        return {
          title: params ? params.otherParam : 'A Nested Details Screen',
          /* These values are used instead of the shared configuration! */
          headerStyle: {
            backgroundColor: navigationOptions.headerTintColor,
          },
          headerTintColor: navigationOptions.headerStyle.backgroundColor,
        };
      };
    
      /* render function, etc */
    }
    
我们也可以使用自定义组件替换标题

    class LogoTitle extends React.Component {
      render() {
        return (
          <Image
            source={require('./spiro.png')}
            style={{ width: 30, height: 30 }}
          />
        );
      }
    }
    
    class HomeScreen extends React.Component {
      static navigationOptions = {
        // headerTitle instead of title
        headerTitle: <LogoTitle />,
      };
    
      /* render function, etc */
    }

标题栏和其所属的页面之间的交互，在`navigationOptions`中`this`绑定的不是 `HomeScreen` 实例，所以你不能调用`setState`方法和其上的任何实例方法。

    class HomeScreen extends React.Component {
      static navigationOptions = ({ navigation }) => {
        return {
          headerTitle: <LogoTitle />,
          headerRight: (
            <Button
              onPress={navigation.getParam('increaseCount')}
              title="+1"
              color="#fff"
            />
          ),
        };
      };
    
      componentDidMount() {
        this.props.navigation.setParams({ increaseCount: this._increaseCount });
      }
    
      state = {
        count: 0,
      };
    
      _increaseCount = () => {
        this.setState({ count: this.state.count + 1 });
      };
    
      /* later in the render function we display the count */
    }

您可以通过`navigationOptions`中的`headerLeft`和`headerRight`属性在标题栏中设置按钮，后退按钮可以通过`headerLeft`完全自定义，但是如果你只想更改标题或图片，那么还有其他`navigationOptions` — `headerBackTitle`，`headerTruncatedBackTitle`和`headerBackImage`