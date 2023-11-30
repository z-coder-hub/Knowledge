> **注意事项**
>
> - React Native >= 0.63.0
> - Expo >= 41
> - TypeScript >= 4.1.0

### 安装库

`npm install @react-navigation/native react-native-screens react-native-safe-area-context @react-navigation/native-stack` 

or

 `yarn add @react-navigation/native react-native-screens react-native-safe-area-context @react-navigation/native-stack`

> 说明：`@react-navigation/native-stack` 依赖于其余三个库

### 创建路由

``` jsx
import {NavigationContainer} from '@react-navigation/native';
import {createNativeStackNavigator} from '@react-navigation/native-stack';
import Home from '.......'
// 1. 引入组件

const Stack = createNativeStackNavigator();
// 2. 创建Stack组件

function App = () => {
  return (
    {/* 3. 创建最外层的容器 */}
    <NavigationContainer>
    	{/* 4. 创建导航器 */}
      <Stack.Navigator>
        {/* 5.创建导航屏幕 */}
      	<Stack.Screen name="Home" component={Home}/>
      </Stack.Navigator>
    </NavigationContainer>
  )
}
```

### 

### `props`

> 它是你的`<Stack.Screen component={}/>`的组件中接收的`props`

1. ###### `props.navigation`对象

   1. `push()`方法，特点是无论**跳转的页面**是不是**当前所在页面**都会将这个路由添加到路由的堆栈中，也就是说`goBack()`调用返回的方法时会涉及到页面是当前页面的问题。
   2. `navigate()`方法，特点是跳转页面如果是当前页面**什么事情都不做**。
   3. `goBack()`方法，返回。
   4. `popToTop()`方法，特点是**直接返回**到路由堆栈的**第一个屏幕**。
   5. `setOptions()`方法，可以修改`options`的配置，`setOptions({title: "修改后的标题"})`
   6. `setParams()`方法，可以修改`params`的值，假如你的`params`需要二次处理，那么就用这个方法

2. `props.route`对象

   1. `params`对象，接收跳转页面时传递的**参数**`navigation.push('Screen1', { userName:XXX,..... })`这样跳转页面的时候，所带的参数就会传递到`Screen1`这个页面的`route.params`中。

