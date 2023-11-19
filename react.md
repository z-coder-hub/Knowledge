是一套基于用户页面的JavaScript库，单项数据流

### jsx语法

> 1. jsx语法要求最外层只有一个根节点
> 2. 单标签必须闭合< />
> 3. img标签必须有alt属性
> 4. class 写成 className
> 5. label中for 写成 htmlFor
>

### 处理事件的方式

react的绑定事件方式和原生JavaScript的方式相同

```react
<onclick onClick={}/>
```

- 箭头函数

```react
<onclick onClick={()=>console.log('abc')}/>
```

- 通过bind将this指向改变成指向当前类

```react
<onclick onClick={this.test.bind(this,agg)}/>
test(str) {
    console.log(this, str)
}
```

##### 事件中传递参数

```react
<onclick onClick={this.test.bind(this,agg)}/>
test(el) {
	console.log(this, el)
    // 如果aggregate没有传参，那么第一个参数就是当前react的虚拟DOM节点
    // 通过虚拟DOM节点的 nativeEvent 可以获取到真实的DOM节点
    // 在通过target获取DOM元素
    el.nativeEvent.target
}
```

### 生命周期函数

##### 挂载阶段

> ***以下顺序和挂载阶段的执行顺序一致***
>
> - constructor  --->  是ES6中类的构造函数，同样也是React生命周期的初始化的钩子函数
>
> 
>
> - <u>*UNSAFE_componentWillMount ---> DOM挂载完毕之前，在React16.2版本之后弃用*</u>
>
> 
>
> - static getDerivedStateFromProps(props, state) ---> 挂载之前触发、只要state或props改变就会触发此静态钩子函数。
>
>   **props和state的值都是当前最新的数据**
>
> 
>
> - render ---> 解析jsx渲染页面，在render函数中不可使用setState
>
> 
>
> - componentDidMount ---> DOM挂载完毕

##### 更新阶段

> - *<u>UNSAFE_componentWillUpdate ---> DOM更新之前执行，在React16.2版本后弃用</u>*
>
>   
>
> - static getDerivedStateFromProps(props, state) ---> 挂载之前触发、只要state或props改变就会触发此静态钩子函数。
>
>   **props和state的值都是当前最新的数据**
>
>   
>
> - shouldComponentUpdate ---> render执行之前执行，返回值为true执行render，false则不执行render
>
> 
>
> 
>
> - render ---> 解析jsx渲染页面
>
> 
>
> - getSnapshotBeforeUpdate(prevProps, prevState) ---> render执行完毕后执行，参数为上一次的props和stata。**返回值由componentDidUpdate中的第三个参数接收**
>
>   
>
> - componentDidUpdate(prevProps, prevState, snapshot) ---> rander执行完毕

##### props更新执行的钩子函数

> *<u>UNSAFE_componentWillReciveProps 此生命周期函数会在props更新时执行，函数中可以执行setState函数</u>*

##### 卸载阶段

> *<u>UNSAFE_componentWillUnmount ---> DOM卸载之前执行，在React16.2版本后弃用</u>*
>
> componentDidUnmount ---> DOM卸载完毕后执行







### 配置路由

> npm i react-router-dom



#### react-route-dom@6 (函数组件)

```react
<BrowserRouter>
    <Routes>
        <Route path={'/'} element={<Main/>}/>
        <Route path={'/top'} element={<App/>}/>
        
        {/*重定向*/}
        <Route path='/' element={<Navigate to='/home'/>}/>
    </Routes>
</BrowserRouter>
```

或者

```react
import {createBrowserRouter} from 'react-route-dom'
const router = createBrowserRouter([
    {
        path:'',
        element:'',
        children:[
            {
                
            }
        ]
    }
])
export default router

// 根组件
import {RouterProvider} from 'react-route-dom'
import router from './router'
<RouterProvider router={router}>
	<App/>    
</RouterProvider>
```



###### BrowserRouter

> 所有的路由，在这个标签中只用

###### Routes

> 显示路由区域的标签

###### Route

> 参数：
>
> path 当浏览器的url改变后，会跳转到element中的组件里
>
> element 当url匹配上了要显示的组件，这里写的是jsx语法

###### Hooks

- useNavigate
  跳转路由

  ```react
  import {useNavigate} from "react-router-dom";
  const history = useNavigate();
  history('/路由')
  ```

- useSreachParams
  获取query数据

  ```react
  import {useSreachParams} from "react-router-dom";
  const [data,fn] = useSearchParams()
  data.get('id')
  ```

- useParams
  获取params数据

  ```react
  import {useParams} from "react-router-dom";
  const params = useParams()
  params.id
  ```

- useLocation
  获取当前路由信息，获取navigate('',{state:{id:1}}) state数据

  ```react
  // pathname是当前访问的完整路径
  const {pathname} = useLocation();
  const userId = location.state.userId
  // 或者
  const {state:{userId}} = location
  ```
  
  

#### react-redux

> 作用：用来获取store中的数据，派发dispatch任务
>
> 优点：省去了subscribe重新渲染的过程



- 在根组件中包裹所有的子组件

`import {Provider} from "react-redux";`

```react
<Provider store={store}>
    <div>
        <Routes>
            <Route path='/' element={<Navigate to='/home'/>}/>
            <Route path='/home' element={<Home/>}/>
            <Route path='/cart' element={<Cart/>}/>
        </Routes>
    </div>
</Provider>
```

- 子组件中通过Hooks来操纵store

  - useSelector
    来获取store中的数据，这有一个回调函数，其中的参数是state数据，将state数据返回。最后来对象结构的方式来接收

    ```react
    import {useSelector, useDispatch} from "react-redux";
    const {stu} = useSelector(state => state);
    ```

  - useDispatch
    创建dispatch对象

    ```react
    import {useSelector, useDispatch} from "react-redux";
    const dispatch = useDispatch()
    ```

    



#### react-router-dom@5







### 组建通信

#### 父传子

```react
// 父组件App , Child是子组件
class App extends Component{
    state = {
        num = 520;
    }
    render(){
        return (
        	<Child num={this.state.num} />
        )
    }
    
}

// 子 (类)组件
class Child extends Component{
    constructor(props){
        super(props)
    }
    
    render(){
        return (
        	<p>
                {this.props.num}
                {/*这里的num就是父组件传递过来的num*/}
            </p>
        )
    }   
}

// 子 (函数)组件
const Child = props =>{
    return (
    	<p>
            {props.num}
            {/*这里的num就是父组件传递过来的num*/}
        </p>
    )
}

```

##### 插槽

```react
// 父组件App , Child是子组件
class App extends Component{
    state = {
        num = 520;
    }
    render(){
        return (
        	<Child num={this.state.num}>
            	{/*在字标签中写的jsx代码，就相当于vue中的插槽*/}
            </Child>
        )
    }
    
}

// 子 (类)组件
class Child extends Component{
    constructor(props){
        super(props)
    }
    
    render(){
        return (
        	<div>
                {this.props.num}
                {/*这里的num就是父组件传递过来的num*/}
                
                
                {this.props.children}
                {/*通过children属性来显示插槽的内容，函数组件相同*/}
            </div>
            
        )
    }   
}

// 子 (函数)组件
const Child = props =>{
    return (
    	<p>
            {props.num}
            {/*这里的num就是父组件传递过来的num*/}
        </p>
    )
}
```



##### 检测prop值

```react
// 安装npm i prop-types

// 类组件和函数组件书写方式一致
import propTypes from 'prop-types'
class Child extends Conponent{
    constructor(props) {
        super(props);
    }
}

Child.propTypes = {
    参数名称: propTypes.类型
    // 这里可以访问npm官网去查找类型的种类
}

```



#### 子传父

```react
class App extends Component{
    state = {
        num = 520;
    }
    render(){
        return (
        	<Child callback={this.callback}>
            	{/*在字标签中写的jsx代码，就相当于vue中的插槽*/}
            </Child>
        )
    }
    cllback = ()=>{
        // 将这个函数传入子组件中
    }
    
}

// 子组件
const Child = props =>{
    return (
    	<p>
            {props.num}
            {/*这里的num就是父组件传递过来的num*/}
        </p>
    )
    changeEvent(){
        this.props.callback(回调参数)
    }
}
```



#### context

```react
// 新建myContext.js文件

// 在react中 引入 createContext
import {createContext} from 'react'

// 创建context对象赋值给一个常量
const myContext = createContext('默认值')

// 使用对象赋值的方式 在myContext 中引入 Provider和Consumer
const {Provider,Consumer} = myContext
/*
	Provider 传递数据函数
	Consumer函数组件用来接收数据函数
*/

// 默认导出myContext
export default myContext

// 导出Provider和Consumer
export {
	Provider,
    Consumer
}
```



##### 传递数据

```react
import {Provider} from 'myContext文件位置';
import React, {Component} from 'react';

class Parent extends Component {
    render() {
        return (
            <div>
                <Provider value=''>
                    {/*
                    	value参数可以传递很多数据
                    */}
                	{/*将子组件写到Provider标签中*/}
                    
                </Provider>
            </div>
        );
    }
}

export default Parent;
```



##### 接收数据

```react
// 类组件
import React, {Component} from 'react';
import myContext from "../myContext";

class Parent extends Component {
	// 定义静态属性contextType ，将myContext赋值到它身上
    static contextType = myContext

    render() {
        return (
            <div>
                父节点
                {this.context}
                {/*
                	通过this.context获取数据
                */}
            </div>
        );
    }
}

export default Parent;

// 函数组件
import React from 'react';
import {Consumer} from "../myContext";
// 引入Consumer组件

const ChildB = props => {
    return (
        <div>
            这是函数子组件
            
            {/*
            	在函数组件中，通过Consumer组件获取数据
            	这个组件中存在一个函数，此函数的第一个参数就是数据本身。
            	函数使用jsx语法
            	
            */}
            <Consumer>
                {data => {
                    return (
                        data.num
                    )
                }}
            </Consumer>
        </div>
    );
};

export default ChildB;
```

### Redux

**安装阶段**

`npm i redux`



##### 大概步骤：

1. **组件通过dispatch派发action任务**
1. **reducer接收dispatch派发的任务**
1. **reducer完成任务并改变store中的state**
1. **最后store通过subscribe通知组件更新**

##### [具体使用方式查看文档](file:///D:/Desktop/React笔记.pdf)





### @reduxjs/toolkit(推荐)

#### 大概步骤：

1. **组件通过dispatch派发action任务**
1. **reducer接收dispatch派发的任务**
1. **reducer完成任务并改变store中的state**
1. **最后store通过subscribe通知组件更新**









#### 初始化

**1.创建configureStore对象**

```react
// store目录中index.js ; 创建configureStore对象
import {configureStore} from '@reduxjs/toolkit'
import changeNum from './reducer/change'
const store = configureStore({
    reducer: {changeNum}
    // reducer中是各个模块化的切片
})

```



**2.创建createSlice切片**

```react
// store目录下的reducer目录中拆分各个的切片
/*
	--store
		index.js
		--reducer
            model1.js
            model2.js	
*/
import {createSlice} from '@redexjs/toolkit'

// 创建模块化切片
const changeNum = createSlice({
	// 独立的名字
    name:'changeNum',
    
    // state初始化, 相当于vuex中的state
    initialState:{
        num:0
    },
    
    // 创建事件处理state数据的函数, 相当于vuex中的mutations
    reducers:{
        changeState(state, num){
            // 在这里可以直接修改state的数据
            // !!实际并没有改变state，因为使用的Immer库
        }
    }  
})
// 导出createSlice中的处理函数
export const {changeState} = changeNum.actions

// 导出createSlice
export default changNum.reducer
```



3.**需要用到@reduxjs/toolkit的组件中配置**

```react
import React, {Component} from 'react';
import store from "./store";
import {changeNum,nullNum} from "./store/reducer/model_1";
// 引入store对象
// 引入要派发的任务

class App extends Component {
    componentDidMount() {
        /*
	       	store对象的执行流程是。
        	通过dispatch派发action任务
        	reducer接收任务
        	最后通过subscribe来通知组件更新
        */
        // store中只要数据发生变化就会触发subscribe这个方法，
        store.subscribe(() => {
            this.setState({})
        })
    }

    render() {
        console.log();
        return (
            <div>
                <button onClick={()=>store.dispatch(changeNum(1))}>+</button>
                <span>{store.getState().model_1.num}</span>
                <button onClick={()=>store.dispatch(changeNum(-1))}>-</button>
                <button onClick={()=>store.dispatch(nullNum())}>清零</button>
            </div>
        );
    }
}

export default App;
```



#### 处理异步任务

```react
import {createSlice, createAsyncThunk} from "@reduxjs/toolkit";
// 需要使用到 createAsyncThunk 
import axios from "axios";

// 使用createAsyncThunk 创建异步函数
// 第一个参数为自定义名称
// 第二个函数为任务函数，可以接收组件调用时的参数
const asyncFn = createAsyncThunk('model_1/getData', async () => {
    let {data} = await axios.get('https://m.maizuo.com/gateway?cityId=440300&pageNum=1&pageSize=10&type=1&k=7886505',{
        headers:{
            'X-Client-Info':' {"a":"3000","ch":"1002","v":"5.2.1","e":"16814426233032684997640193"}',
            'X-Host':'mall.film-ticket.film.list'
        }
    })
    return data
    // 异步任务需要一个返回值，在configureStore中的extraRedUcers 中接收
})

const model_1Slice = createSlice({
    name: 'model_1',

    initialState: {
        num: 1,
        data:[1,2]
    },

    reducers: {
        changeNum(state, action) {

            state.num += action.payload
        }, nullNum(state, action) {
            state.num = 0
        },
    },

    extraReducers: {
        // 这里接收异步任务函数 promise对象返回的 fulfilled成功后的回调
        // 编写方式 [异步函数名.fulfilled]:(state,action)=>{} 
        // 这里的state跟action的使用和reducer中是一样的，同样可以直接修改state
        // action.payload 接收的是异步任务函数的返回值，可以通过这个返回值来修改state中的数据
        [asyncFn.fulfilled]: (state, action) => {
            // console.log([...state.data],action.payload);
            state.data = action.payload.data
        }
        
    }
    /*
    	浏览器识别说这种写法即将弃用
    	解决方法如下
    */
    extraReducers: (builder) => {
    // extraReducer为一个函数，函数的参数builder.addCase来接收异步任务
    // builder.addCase(异步函数.fulfilled, reducer)
        builder.addCase(getData.fulfilled, (state, action) => {
            console.log(state, action);
        })
    }
})


export {asyncFn}
// 单独导出一下异步任务函数
export const {changeNum, nullNum} = model_1Slice.actions

export default model_1Slice.reducer


```



#### 使用react-redux方式

3.使用react-redux api中的provider组件包裹根组件

```react
import React from 'react';
import ReactDOM from 'react-dom/client';
import store from "./store"
import {Provider} from "react-redux";
// 引入react-redux库中的Provider

import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    // Provider中可以接收一个store参数这个参数可以向包裹的组件props对象中传入一个数据
    <Provider store={store}>
        <App/>
    </Provider>
);

```



4.在需要用到@reduxjs/toolkit的组件中配置



```react
import React, {Component} from 'react';
import {connect} from "react-redux";
// 通过connect高阶组件让组件具有store的方法

import {changeNum} from "./store/reducer/model_1";
// changeNum为分模块切片中的一个reducers任务

class App extends Component {
    render() {
        console.log(this.props);
        return (
            <div>
                <button onClick={()=>this.props.changeNum(1)}>+</button>
                {/*
                当点击加或减的时候
                会触发connect中getDispatch返回的changeNum任务
                */}
                <span>{this.props.num}</span>
                <button onClick={()=>this.props.changeNum(-1)}>-</button>
            </div>
        );
    }
}


const getState = state => {
    return {
        state
    }
}

const getDispatch = dispatch => {
    return {
        changeNum(num){
            dispatch(changeNum(num))
        }
        // 这里返回的多个方法，存在于props中；调用则会向store派发任务
    }
}

export default connect(getState,getDispatch)(App);
/*
connect函数有两个函数参数配置
1.函数中存在state数据；return返回这个state数据。当前被包裹的组件的props中就会存在返回的state的数据
2.函数中存在dispatch；通过dispatch可以向store中派发任务；此函数需要返回dispatch
*/
```



#### 函数组件Hooks

```react
import {useSelector, useDispatch} from "react-redux";
/*
 useSelector 通过回调函数获取store中数据
 useDispatch 创建dispatch对象，用于派发任务
*/
const App = ()=>{
    const {stu} = useSelector(state => state);
    const dispatch = useDispatch();
	return (
        <div></div>
    )    
}
```



### Hooks



#### useState

> 作用：定义状态数据
>
> `const [当前值, 修改值的函数] =  useState(默认值)`
>
> 可以自定义名称



#### useEffect

> 作用：只执行一次
>
> `useEffect(()=>{ }, [])`
>
> 参数：
>
> 1. 回调函数，在没有依赖的情况下只执行一次；**这里return出一个匿名函数，这个函数相当于类组件中的componentDidUnmount,， 作用就是结束不需要的定时器，事件等**
> 2. 数组，如果**传递空数组表示没有任何依赖**，数组中填入的是所依赖的数据。**如果依赖了**一个数据，那么那个数据**改变之后**才会触发useEffect方法



#### useCallback

> 当useState中的数据发生变化后，会将整体组件重新挂载，那样的话组件中所有的方法都会重新创建
>
> `useCalback(()=>{ }, [])`
>
> 作用：依赖state中的数据来处理方法的创建，提升性能
>
> 参数：
>
> ​	1.回调函数
>
> 2. 数组，依赖的数据



#### useMemo

> 和callback用法一致，memo中的回调函数有返回值，那个返回值是一个函数
>
> `useMemo(()=>{return ()=>{}}, [])`



#### useRef

> 实现非受控组件
>
> `useRef(默认值)`



#### useContext

> 用来接收**最近一个**Provider传递过来的数据
>
> 使用之前需要编写context组件
>
> `const context = useContext(mycontext)`
>
> context就是provider中的数据







