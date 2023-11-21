> 业余爱好自整理一些使用小技巧，满满干货。仅供参考！!

### `Text`

- `RN`当中所有的显示文本必须放到`Text`组件当中

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



### `Height`

- 在`RN`中没有像素单位，只有数字单位、百分比字符串、flex
- 数字你可以认为是`px`，我说的是认为，它在不同设备上会有一些差异
- 百分比字符串就是可以根据父级盒子的高度来决定当前比例
- flex就是弹性壳子，设置`flex: 1`相当于撑满高度

