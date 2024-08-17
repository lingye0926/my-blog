### Redux and React

## Redux introduction 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tutorialjinni.com/redux/4.1.1/redux.min.js"></script>
    <title>Document</title>
</head>
<body>
    <button id="decrement">-</button>
    <span id="count">0</span>
    <button id="increment">+</button>
</body>
<script>
    //1.定义reducer函数
    //作用：根据不同的action对象，返回不同的新的state
    //state:管理的数据初始状态
    //action：对象type标记当前想要做什么样的修改
    function reducer(state = {count:0},action){
        if(action.type === 'INCREMENT'){
            return {count:state.count + 1}
        }
        if(action.type === 'DECREMENT'){
            return {count: state.count - 1}
        }
        return state
    }
    //2.使用reducer函数生成store实例
    const store = Redux.createStore(reducer)

    //3.通过store实例的subscribe订阅数据变化
    //回调函数可以在每次state发生变化的时候自动执行
    store.subscribe(()=>{
        console.log("state变化了" ,store.getState())
        document.getElementById('count').innerText = store.getState().count;
    })

    //4.通过store实例的dispatch函数提交action更改状态
    const inBtn = document.getElementById('increment')
    inBtn.addEventListener('click',()=>{
        store.dispatch({
            type:'INCREMENT'
        })
    })

    const dBtn = document.getElementById('decrement')
    dBtn.addEventListener('click',()=>{
        store.dispatch({
            type:'DECREMENT'
        })
    })

    //5.通过store实例的getState方法获取最新状态更新到视图中

</script>
</html>
```

## Redux in React
 redux store配置<br>
 - 配置counterStore模块<br>
 - 配置根store并组合counterStore模块<br>
 ⬇️<br>
 __store__<br>
 ⬆️<br>
 React组件<br>
 - 注入store（react-redux）<br>
 - 使用store中的数据(useSelector)<br>
 - 修改store中的数据(useDispatch)<br>

 ### redux store 配置
 
 store/modules/counterStore.js
 ```javascript
import {createSlice} from '@reduxjs/toolkit'

const counterStore = createSlice({
    name:'counter',
    //初始化state
    initialState:{
        count:0
    },
    //修改状态的方法 同步方法 支持直接修改
    reducers:{
        increment(state){
            state.count ++
        },
        decrement(state){
            state.count --
        }
    }
})

//解构出来actionCreater函数
const {increment,decrement} = counterStore.actions
//获取reducer
const counterReducer = counterStore.reducer

//以按需导出的方式到处actionCreater
export {increment,decrement}
//以默认导出的方式导出reducer
export default counterReducer;

```
store/index.js
```javascript
import { configureStore } from "@reduxjs/toolkit";
//导入子模块reducer
import counterReducer from './modules/counterStore'
const store = configureStore({
    reducer:{
        counter:counterReducer
    }
})

export default store;
```
App.js
```javascript
import { useSelector,useDispatch } from "react-redux";
import {increment,decrement} from './store/modules/counterStore'
function App(){
    const {count} = useSelector(state => state.counter)
    const dispatch = useDispatch()
    return (
        <div className="App">
            <button onClick={()=> dispatch(decrement())}>-</button>
                {count}
            <button onClick={()=> dispatch(increment())}>+</button>
        </div>
    )
}

export default App;
```
index.js
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
//导入项目的根组件
import App from './day_03/App';
import store from './day_03/store'
import { Provider } from 'react-redux';
//把App根组件渲染到id为root的dom节点上
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <React.StrictMode>
    <Provider store = {store}>
        <App />
    </Provider>
    </React.StrictMode>
    
);

//使用步骤
// 1.定义一个reducer函数（根据当前想要做的修改返回一个新的状态）
// 2.使用createStore方法传入reducer函数，生成一个store实例对象
// 3.通过store实例的subscribe订阅数据变化
// 4.通过store实例的dispatch函数提交action更改状态
// 5.通过store实例的getState方法获取最新状态更新到视图中
```


 
