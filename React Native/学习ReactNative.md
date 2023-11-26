> 业余爱好自整理一些使用小技巧，满满干货。仅供参考！!

## 启动项目的命令

### Mac

安装IOS需要的依赖`bundle install`

安装cocoapods所需要的依赖`bundle exec pod install --project-directory=ios`

自动打开xcode并定位到当前的目录`xed ios`

运行到不同设备`npx react-native run-ios --simulator="设备名称 例如 iPad Pro"`

### Android

`yarn android`

`npx react-native run-android`

`yarn react-native run-android`

三条命令作用相同，如果需要指定的模拟器提前打开模拟器之后运行以上命令



## 可视化知识整理（组件、样式等）

### `Text`

- `RN`当中所有的显示文本必须放到`Text`组件当中
- css上约束很多，如果添加一些高级的样式可以选择外层套一层view组件

### `View`

- `View` === `div`

### `ScrollView`

- `RN`中不可以通过`css`控制溢出滚动也不可以自动识别溢出滚动，只能通过这个组件才可以溢出滚动



### `样式`

- `RN`中默认所有的盒子都属于弹性盒子

- 在`RN`中不能通过引入`css/sass/scss/less`来设计样式，在`RN`当中有特定的`API`来规定样式的设计

- 通过`StyleSheep`设计样式

- ```jsx
  import {View, StyleSheep} from 'react-native'
  function App() {
      <View style={styles.ViewStyle}>
  		
  	</View>
  }
  const styles = StyleSheep.create({
      ViewStyle: {
          // css样式写到这里,和css的区别是  去掉 - 将单词写为小驼峰法
      }
  })
  
  ```



### `Height`和`width`

- 在`RN`中没有像素单位，只有数字单位、百分比字符串、flex
- 数字你可以认为是`px`，我说的是认为，它在不同设备上会有一些差异
- 百分比字符串就是可以根据父级盒子的高度来决定当前比例
- flex就是弹性壳子，设置`flex: 1`相当于撑满高度

### 获取状态栏高度

#### IOS

``` js
import {NativeModules} from 'react-native';
const {StatusBarManager} = NativeModules;
// StatusBarManager.HEIGHT 就是状态栏的高度
```

#### Android

``` js
import {StatusBar} from 'react-native';
// StatusBar.currentHeight. 就是状态栏的高度
```





## 非可视化整理（API、原生模块等）


### `get IP`
- ``` jsx
  if (Platform.OS === 'android') {
      NativeModules.NetworkInfo.getIPAddress((err, ip) => {
        if (err) {
          reject(err);
        } else {
          ipAddress = ip;
          resolve(ipAddress);
        }
      });
    } else {
      setTimeout(() => {
        fetch('https://api.ipify.org?format=json')
          .then((response) => response.json())
          .then((json) => {
            ipAddress = json.ip;
            resolve(ipAddress);
          })
          .catch((error) => {
            reject(error);
          });
      }, 500);
    }

### `Platform`
- `Platform.OS`获取当前手机的型号，返回值时`android || ios`


### `Image`图片

> 完整使用教程 https://reactnative.cn/docs/images

```jsx
<Image source={require('./my-icon.png')} />
```

- 这是引入静态图片，这里注意`require`里边中只能是完整的路径，不能用`js`的方式拼接等等

```jsx
<Image source={{ uri: "网络路径" }} />
```

- 这是引入网络图片
>>>>>>> Stashed changes
