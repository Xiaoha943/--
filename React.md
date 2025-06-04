#### React

##### React核心概念

1. 组件：函数式组件、类式组件
2. `JSX`：
3. `Virtual DOM`
4. 组件生命周期
5. 状态（state）和属性（props）ref
6. 事件处理
7. 条件渲染、列表渲染
8. 父子组件通信：react是单向数据流，数据只能由父传向子，子不能传向父

##### React基础概论，MVVM模式

MVVM是`Model-View-View-Model`的简写，即模式-视图-视图-模式

不需要直接操作DOM，主要是实现双向数据绑定

![](D:\interview\Note\复习笔记\img\MVVM.png)

用户<==>view(页面视图)<=双向数据绑定=>viewModel(视图数据模型和逻辑)<=JSON Ajax=>model(业务逻辑和数据库)

View层是视图层，也就是用户界面，主要是由HTML和CSS来构建

Model层是数据模型，后端进行业务逻辑处理及数据处理

ViewModel 是前端处理的视图数据层，用于连接view和model的桥梁，前端通过model层获取数据进行处理成界面需要展示的数据格式，视图状态和交互行为都存储在viewModel里面

![](D:\interview\Note\复习笔记\img\MVVM_Composition.png)

MVVM模式执行步骤：不直接操作DOM进行数据更新修改，将界面数据转换成js对象格式数据Model，HTML界面为View,通过修改js数据更新view

MVVM设计模式的优缺点：

优点：

1. 双向数据绑定及单向绑定

   双向绑定：View改变更新ViewModel,ViewModel改变驱动View更新

   单向绑定：ViewModel驱动自动更新View

2. 简化代码

3. 可维护性

4. 可测试

5. 低耦合可重用

##### JSX及JSX的理解与使用

`JSX`规则：

1. 只能返回一个根元素（顶层只能为一个节点包裹起来）

2. 标签必须闭合

   ```jsx
   <img/>
   <li>oranges</li>
   ```

3. 使用驼峰式命名法给大部分属性命名（包括类名className、内联样式属性名 backgroundColor）

   ```jsx
   <div className ='my-img'>图片</div>
   ```

4. 使用""引号传递字符串

5. 使用`<img alt={"图片不见啦"}/>`传递js中的属性或变量或状态

6. 使用`<img style={{backgroundColor:'red',withd:'10px'}}/>`传递必须传递对象格式的数据

7. JSX作为子组件传递

   ```react
   const MyCard = ({children})=>{
       return (<>
           {children}
       </>)
   }
   
   const MyApp =()=>{
       return (
           <div>我的组件</div>
       )
   }
   
   const App = ()=>{
       return (
           <MyCard>
               <MyApp/>
           </MyCard>
       )
   }
   
   ```

##### Props、State、Refs

函数组件通过props传入

State：

* state变量（index）会保存上次渲染的值
* setState函数（setIndex）可以更新state变量并触发React重新渲染组件

State特点：

* State是隔离且私有的（若渲染同一个组件两次，每个副本有完全隔离的state）
* 同一位置的相同组件，state默认一致:chestnut:`(isShow?<MyComponet person={'lili'}/>:<MyComponent person={'hanjingwen'} />)`​  

同一位置组件使用不同的state：

1. 将组件渲染在不同的位置
2. 使用`key`赋予每个组件明确的身份

```react
import {useState} form 'react'
const [index.setIndex] = useState(0)
//其中index是state变量，setIndex是对应的setter函数
//其中的[和]语法称为数组解构，允许从数组中取值，useState返回的数组正好有两项
```

ref定义：希望组件记住某些信息，但是又不想触发渲染时，可以使用ref

ref指向一个数字，向任何东西：字符串、对象甚至是函数。与state不同的是，ref是一个普通的js对象，具有可以被读取的current属性，**组件不会在每次递增时重新渲染**

ref和state不同的点

useRef()返回{current:initvalue} useState([state,setState])

改变时不会触发页面渲染，state改变时触发页面渲染

不能在渲染期读取和写入，可以随时读取state

可变-可以在渲染期间之外任何时候读取或者写入，不可改变-只能通过setState进行改变**组件不会在每次递增时重新渲染**

重点：

* ref是一种脱围机制，用于保留不用于渲染的值
* ref是一个普通的js对象，具有名为current的属性，可以读取或写入
* 通过useRef的Hook让React给一个Ref
* 与state一样，Ref允许在重新渲染的时候保留信息
* ref改变不会触发重新渲染
* 不要在渲染过程读取写入ref.current

##### forwardRef

具体使用：

1. 将`<MyInput ref= myRef >`传入绑定的ref

2. MyInput中接收传入的ref，使用

   ```typescript
   const MyInput = forWardRef((props,ref)=>{ ref.useImperativeHandle(ref,()=>({myFocus,myScroll}))
       return (<input ref=ref/>)
   })
   ```

3. MyInput使用useRef创建MyInputRef，通过MyInput.current获取子组件属性或者方法

:point_right:

* Refs是通用概念，但是大多数情况下会使用它们来保存DOM元素
* 通过`<div ref={myRef}></div>`React将DOM节点放入`myRef.current`
* refs通常用于聚焦或者滚动等非测量操作
* 组件不暴露DOM节点，通过使用`forwardRef`的第二个参数传入特地节点来暴露组件DOM
* 避免更改由React管理的DOM节点

##### Effect

三个规则：

1. 声明Effect
2. 指定Effect依赖
3. 必要时添加清理（cleanUp）函数

```react
//错误示范
const [page,setPage] = useState(0)
useEffect(()=>{
    setPage(page+1)
})
//会循环执行
```

```typescript
useEffect(()=>{
    //任何状态改变重新渲染就会触发
})
useEffect(()=>{
    //组件挂载后执行一次
},[])
useEffect(()=>{
    //a或者b的值发生改变后就会触发
},[a,b])
```

useEffect清理函数调用

```typescript
useEffect(()=>{
    return ()=>{
        //调用清理函数
    }
},[])
```

**React总是在执行下一轮渲染的Effect前清除上一轮渲染的Effect**

##### useEffect的生命周期

react组件生命周期：挂载，更新，卸载（不适用于effect）

##### 自定义hooks

注意：

* Hook的名称必须永远以use开头

* React组件名称必须以大写字母开头

* Hook的名称必须以use开头，然后紧跟一个大写字母

  ```typescript
  const useChatRoom =({roomId,serverUrl})=>{
      useEffect(()=>{
          const options={
              serverUrl:serverUrl,
              roomId:roomId
          }
          const connection = createConnection(option);
          connection.connect();
          connection.on('message',()=。{
                        showNotification('New message:'+msg);
                        });
      return ()=> connection.disconnect()
      },[roomId,serverUrl])
  }
  
  const chatRoom =({roomId})=>{
      const [serverUrl, setServerUrl] = useState('https://localhost:1234');
      //调用自定义hooks
      useChatRoom({roomId,serverUrl})
  }
  ```

  :chestnut:举个栗子

  ```typescript
  //提出的自定义hooks
  const useDate =({url})=>{
      const [data,setData] = useState([])
      useEffect(()={
          let ignore = false
          fetch(url).then(()=>{
          console.log('执行')
      }).then((e)=>{
          if(!ignore){
              setData(e.data)
          }
      })
     	return ()=>{
          ignore = true
      }
      },[url])
  }
  ```

  ```typescript
  const myArea =()=>{
      const city = useData('/api/get/city')
      const province = useData('/api/get/province')
      //....
  }
  ```

**总结**

* 自定义Hook可以在组件间共享逻辑
* 自定义Hook命名必须以后跟一个大写字母的`use`开头
* 自定义Hook共享的只是状态逻辑，不是状态本身（可以多次在同一个组件中使用自定义Hook，Hook中的状态不会互相影响）
* 可以将状态值从一个Hook传到另一个，并且会保持最新
* 每次组件重新渲染时，所有的Hook会重新运行
* 自定义Hook的代码应该和组件代码一样纯粹
* 把自定义Hook收到的事件处理函数包裹到EffectEvent

##### react-router

基本概念：

* react-router是router的核心部分代码
* react-router-dom是运用于浏览器

##### Reducer

定义：

1. 将设置状态的逻辑改成dispatch和一个action
2. 编写一个reducer函数
3. 在组件中使用reducer

**重点**

* 把useState转化为useReducer
  1. 通过事件处理函数dispatch actions；
  2. 编写一个reducer函数，它接受传入的state和action，并返回一个新的state
  3. 使用useReducer替换useState
* Reducer可能需要写更多代码，但是有利于调试和测试
* Reducer必须是纯净的
* 每个action都描述了一个单一的用户交互
* 使用immer来在Reducer里面直接修改状态

```typescript
const myReducer = (tasks,actions)=>{
    switch (actions.type){
            case: 'add'
            //....
            break;
            case: 'delete'
            //...
            break;
            case:'save'
            //....
            break;
            }
}

const app = ()=>{
    const [tasks,dispatch] = useReducer(myReducer,initValue)
    
    const delete =(deleteId)=>{
        dispatch({
            type:'delete',
            id:deleteId
        })
    }
    return (<><TaskList tasks={tasks}/></>)
}
```



##### 父子组件通信

react的数据流是单向的，由父项通过props传入子项，子项可以通过回调函数触发父项方法

##### 跨组件通信（主要是context深层传递）

通过props将信息从父组件传递到子组件，如果需要通过许多中间组件向下传递props，或者应用许多组件需要相同的信息，props会变得十分冗长和不便，context允许组件向其下层无论多深提供信息，也无需通过props显式传递。

* 什么是prop逐级传递
* 如何使用context代替重复的参数传递
* Context的常见用法
* Context的常见代替方案

总结：

1. `Context`使组件向其下方整个树提供信息
2. 传递`Context`的方法：
   * 首先创建context `export default const myContext = new CreateContext(defaultValue) `
   * 在不管多深的子组件获取值，`const value = useContext(myContext)`
   * 父组件包裹提供传入的值：`<myContext.Provider value={level+1}>{children}</myContext.Provider>`
3. `Context`会穿过中间的任何组件
4. `Context`可以让你写出较为通用的组件
5. 在使用`Context`之前，先试试传递Props或者将JSX作为`children`传递。

##### Reducer和Context运用

步骤：

1. 创建context
2. 将state和dispatch放入context
3. 在组件任何地方使用context

```typescript
//首先声明context
const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);
```

```typescript
//其次创建context.provider
const TaskProvider = ({children})=>{
    const [tasks,dispatch] = useReducer(myTaskReducer,initValue)
    return (
        <TasksContext.provider value={tasks}>
        <TasksDispatchContext.provider value={dispatch}>
       {children}
        </TasksDispatchContext.Provider>
        </TasksContext.Provider>
    )
}
```

```typescript
//顶级
const MyApp = ()=>{
    return (
        <TaskProvider>
        <TaskList/>
        <option/>
        </TaskProvider>
    )
}
```

```javascript
const MyTaskReducer = (tasks,action)=>{
    switch(action.type){
      case:'add'
           //...
      break;
           
           }
}
```



##### React语法

react语法是是使用JSX函数式编程

##### React生命周期

```js
constructor
componentWillMount
render
componentDidMount

shouldComponetUpdate
componentWillUpdate
render
componentDidUpdate

componetWillUnMount
```

##### React组件与DOM

##### React事件

react的原生事件被包装，所有的事件都冒泡都顶层document监听，然后在合成事件下发。

基于跨端使用事件机制，而不是Web DOM强绑定。

react组件上无事件，父子组件通信使用props

##### VirtualDOM

概念：使用`JS`模拟的`DOM`结构，将`DOM`变化的对比通过`JS`进行处理。

* 具有`batching`(批处理)和高效的`Diff`算法

优点：

* 不直接操作DOM，通过改变虚拟DOM通过diff运算对比修改点渲染界面
* 直接对于js对象进行开发，降低复杂度，提高可维护性

##### Diff算法

* 将DOM结构转换成js对象格式的虚拟DOM
* 使用Diff算法将原始虚拟DOM和修改后的虚拟DOM进行对比找出改变的那部分虚拟DOM
* 将改变的那部分`VirtualDOM`转换成真实DOM渲染回界面（把差异应用到到真正的DOM树上）

##### Fiber

fiber类似于一个判断是否或者何时进行diff算法对比虚拟DOM然后更新或者渲染的机制，可以随时终止浏览器超长时间执行实现渲染界面不卡顿优化

vue可以不使用diff是因为vue使用了模板，固定的模板可以根据具体部分节点变更diff对比局部更新渲染不会造成大部分界面渲染，vue2使用getter和setter准确更新需要渲染的界面

react则是通过比较引用的方式进行更新，Props变化会导致涉及到的子组件更新

##### 异常处理

##### React设计原理

##### React的调试

##### React的性能优化

##### React Hooks

基础的hooks有哪些？

`useState useEffect useRef useMemo useCallBack useContext useReducer`

useMemo和useCallBack有什么区别？

区别是Memo用于返回一个数组，callback用于返回一个函数

##### Redux概念

Redux是一个小型的独立JS库。

React-Redux，Redux可以集成到任何的UI框架，其中最常见的是React。

React Toolkit 是推荐的编写Redux逻辑方法。

React DevTools扩展 可以显示Redux存储中状态随着时间变化的历史记录。

三个部分：Actions=>State==>View==>Actions

**Redux期望所有的状态更新都是使用不可变的方式**

reducer必须符合：

* 仅使用state和action参数计算新的状态值
* 禁止直接修改state。
* 禁止任何异步逻辑、依赖随机值导致其他副作用的代码

##### Redux基本使用

* Action:事件，一个具有type字段的普通js对象

* Reducer：函数，接收state和action对象，必要时决定如何更新状态，并返回新状态

   redux reducer函数与reducer的回调函数原理相同，接收上一个结果（state）和当前项（action对象），根据这些参数计算state，并返回一个新的state

  ```typescript
  	const actions = [
      {type:'couter/increment'},
      {type:'couter/increment'},
      {type:'couter/increment'},
  ]
  
  const initialState ={value:0};
  const finalResult = actions.reduce(counterReducer,initialState)
  //{value:3}
  ```

* Store : 当前Redux应用的state存在于一个名为store的对象中，store是通过传入一个Reducer来创建的，并且有一个`getState`的方法，返回当前状态

  ```react
  import {configureStore} from '@reduxjs/toolkit';
  const store = configureStore({reducer:counterReducer});
  console.log(store.getState())//{value:0}
  ```

* Dispatch :函数方法，**更新state的唯一方法就是调用**`store.dispatch({})`**传入一个action对象**

  ```react
  store.dispatch({type:'couter/increment'})
  console.log(store.getState())//{value:1}
  ```

  dispatch可以看作是调用触发了一个事件

  ```typescript
  const increment = ()=>{
      return {
          type:'pay/increment'
      }
  }
  store.dispatch(increment());
  store.getState() //{value:1}
  ```

* Selector 

  Selector函数可以从store状态树中提取指定片段，避免重复读取逻辑。

  ```react
  const selectCounterValue = (state) =>state.value;
  const currentValue = selectCounterValue(store.getState());
  console.log(currentValue)
  ```

:point_right:**总结**

* redux是一个管理全局的应用状态的库

* redux使用单向数据流

* redux使用代码类型

  1. action，是一个js对象
  2. Reducer是一个纯函数，通过state和action计算状态返回新的状态
  3. dispatch，每当dispatch一个action，store就会调用root Reducer

  Redux Slice 是应用中单个功能的Redux Reducer 逻辑和action的集合，通常一起定义在一个文件中

##### CreateSlice、reducer函数

```javascript
//创建Reducer函数，及store的部分切片
export const counterSlice = createSlice({
    name:'common',
    initialState：{
                    value:0
                },
    reducer:{
        increment:(state)=>{
        state.value +=1
        },
        decrement:(state)=>{
            state.value -=1
        },
        incrementByAmount:(state,action)=>{
            state.value += action.payload;
        },
   	},
})

export const {increment,decrement,incrementByAmount} = counterSlice.actions;
export default counterSlice.reducer;
```

```javascript
store.dispatch({
    type:'common/increment',
})
[
    type:'common/increment',
    type:'common/decrement',
    type:'common/incrementByAmount'
]
```

**createSlice内部使用了一个immer的库，immer使用了一种称为proxy的特殊js工具来包装提供的数据，immer会跟踪尝试进行所有的更改，然后返回一个安全的、不可变的更新值**

##### useSelector提取数据

useSelector这个hooks可以让组件从redux的store状态树中提取到所有的数据。

```javascript
const selectCount = (state)=> state.counter.value;
//传递数据
const useMySelector = useSelector;
//获取数据
const {
    name,
    list,
} = useMySelector((state)=> state.commonSlice)
```

#####  useDispatch进行dispatch action

```javascript
//store/index.ts中创建
const useMyDispatch = useDispatch;
//具体组件界面使用
const dispatch = useMyDispatch()
//调用,更新state值
dispatch(func())
```

##### Redux进阶

##### Router 概念

##### Router基本使用

* react-router是router的核心部分代码
* react-router-dom是用于浏览器的
* react-router-native是用于原生应用的

安装react-router-dom会自动安装react-router的依赖

```javascript
npm i react-router-dom
```

**BrowserRouter和HashRouter/Routers和route**

* BrowserRouter使用history模式

* HashRouter使用hash模式

* `<Routes>`都会匹配当前位置的子路由

* `Route`用于路径的匹配

  * path：匹配路径

  * `element`路由与URL匹配时要渲染的React元素

    ```react
    <Routes>
        <Route path='/home' element={<Home/>}></Route>
        <Route path='/about' elemnt={<About/>}></Route>
        <Route></Route>
        <Route></Route>
    </Routes>
    ```

1. **Link和NavLink**

   * 通常路径的调整是使用link组件，最终会被渲染成a元素
   * NavLink是在Link基础上增加一些样式属性
   * to属性：Link中最重要的属性，用于设置跳转到的路径

   2.Navigate导航

2. **ReactRouter的路由嵌套**

   ```react
   <div>
       <Routes>
           /**此时访问路径会发现一个问题，当访问/home路径的时候界面并没有出现子路由界面，因此要进行优化使用Navigate App.jsx*/
           <Route path='/' element={<Navigate to="/home">}></Route>
           <Route path='/home' element={<Home/>} >
               <Route path='/home/HomeSon1' element={<HomeSon1/>}></Route>
               <Route path='/home/HomeSon2' element={<HomeSon2/>} ></Route>
           </Route>
           <Route path='/about' element={<About/>} ></Route>
           <Route path='/user' elemtn={<User/>}></Route>
       </Routes>
   </div>
   ```

3. **Hooks中的useNavigate实现路由的跳转**

   ```react
   import {useNavigate} from 'react-route-dom'
   const navigate = useNavigate()
   <Header className='Header'>
       <h3>
           <button onClick={e=>navigate("/home")}>首页</button>
           <button onClick={e=>navigate("/about")}></button>
           <button onClick={e=>navigate("./user")}></button>
       </h3>
   </Header>
   <div>
       <Routes>
           /**此时访问路径会发现一个问题，当访问/home路径的时候界面并没有出现子路由界面，因此要进行优化使用Navigate App.jsx*/
           <Route path='/' element={<Navigate to="/home">}></Route>
           <Route path='/home' element={<Home/>} >
               <Route path='/home/HomeSon1' element={<HomeSon1/>}></Route>
               <Route path='/home/HomeSon2' element={<HomeSon2/>} ></Route>
           </Route>
           <Route path='/about' element={<About/>} ></Route>
           <Route path='/user' elemtn={<User/>}></Route>
       </Routes>
   </div>
   ```

   封装高阶组件拆分

   ```react
   function HocWithRouter(WrapperComponent){
       return function(){
           const navigate = useNavigate()
           const router ={navigate}
           return <WarpperComponent{...props} router={router}/>
       }
   }
   ```

4. **通过Hooks-useParams使用动态路由传参**

   * `/search`的`path`对应一个组件的`Search`
   * 如果将`path`在`Route`匹配时写成`/search/:id`,那么`/search/abc`、`/Search/123`都可以匹配到改Route，并且进行显示；
   * 这个匹配规则，我们就称之为动态路由，通常情况下，使用动态路由可以为路由传递参数

   ```react
   function hocWithRouter(WarpperComponent){
       return (props)=>{
           const navigate = useNavigate()
           const params = useParams()
           const router ={ navigate, params }
           return <WarpperComponent {...props} router={router}>
       }
   }
   ```

   5. **Router的配置方式-useRoutes**

   ```react
   <Routes>
       <Route path='/' element={<Navigate to="/home"/>}></Route>
       <Route path='/home'element={<Home/>}></Route>
       <Route path='/about'element={<About/>}></Route>
       <Route path='/user' element={<User/>}></Route>
       <Route path='*' element={<div>页面丢失了</div>}></Route>
   </Routes>
   ```

   ```javascript
   const routes=[
       {
           path:'/',
           element:<Navigate to="/home" />
       },
       {
           path:'/home',
           element:<Home />,
           children:[
               {
                   path:'/home',
                   element:<Navigate to="/home/HomeSon1">
               },
       		{
                   path:'/home/HomeSon1',
                   element:<HomeSon1 />
               },
               {
                   path:'/home/HomeSon2',
                   element:<HomeSon2>
               }
       
           ]
       },
       {
           path:'/about',
           element:<About/>
       },
       {
           path:'/user',
           element:<User/>
       },
       {
           path:'*',
           element:<div>页面丢失了</div>
       }
   ]
   
   export default routes
   ```

   6. **路由懒加载**

      对某些组件进行异步加载（懒加载），那么需要使用Suspense进行包裹：

      使用React.lazy进行懒加载

      `Suspense`用于解决访问页面时候需要下载相关文件会耗时这时会有一定空白窗，页面不显示内容，使用`<Suspense>`组件用来做显示：在子组件记载前展示后备方案

      ```react
      import React from 'react'
      const HomeSon1 = React.lazy(()=> import("./page/HomeSon1"))
      const HomeSon2 = React.lazy(()=> import("./page/HomeSon2"))
      ```

##### Router进阶

#### 细节问题：

##### 1.React中的类组件和函数组件之间有什么区别？

解：类组件不允许改变传入的props中属性，

函数组件：语法上接收一个props对象返回一个react元素，调用方式=>函数调用

类组件：语法上需要继承React.Component并创建render函数返回React元素，调用方式=>new一个实例，通过实例进行调用

##### 2.react的特点 

* 声明式编码
* 组件化编码
* React Native 编写
* 高效 （优秀的diffing算法）

##### 3.React高效的原因

* 使用虚拟DOM渲染方式，不直接操作真实DOM
* DOM Diffing算法，最小化页面重构

##### 4.React和Vue的区别点

* 框架本质不同，Vue框架是基于MVVM模式，而React是封装的js库
* 数据流和响应式不同，Vue是双向数据绑定，而React是单向数据流
* 模板语法不同，Vue是指令+模板语法，React是JSX函数式编程
* Vue的diff算法实现，是边对比边渲染更新，React的diff算法是找出修改点进入队列批量处理渲染更新
* 事件机制：vue是直接原生事件，react是合成事件：事件冒泡到根节点进行事件委托处理，且做了跨端兼容处理

##### 5.不能把函数组件的定义嵌套起来的原因

* 每次父组件的方法状态更新都会渲染创建一个不同的子组件，其实在相同位置渲染的是不同的子组件，所以react每次都会清空子组件的state

  导致bug。

* **永远要将组件定义在最上层并且不能将组件的定义嵌套起来**

##### 6.**React为什么要废弃componentwillMount、ComponentWillReciveProps、componentWillUpdate**三个生命周期钩子？他们有哪些问题？React又是如何解决的呢？

导致的问题：在render阶段执行的生命周期函数会重复执行多次

通过使用`getDerviedStateFormProps`静态生命周期，则不能通过this获取组件实例，该方法中则不能调用this.setState或者接口请求等方法进行副作用操作导致生命周期重复执行，强制开发者在render之前不要掉用有副作用的操作

##### 7.react.render方法的原理？在什么时候会触发

有两种形式：

1. 类组件：

   ```react
   class Foo extends React.component{
       render(){
           return <h1>Foo</h1>
       }
   }
   ```

2. 函数组件

   ```react
   const Foo =()=> <h1 className={foo-h}>Foo</h1>
   ```

   在`render`中，会编写jsx，jsx通过babel编译后会转化成js，在render过程中，React将新调用render函数返回的树与旧的的树虚拟DOM通过使用diff算法进行比较，对比更新DOM树

   ```react
   return (
       <div className='my-App'>
           <header>头</header>
           <div>start</div>
           Right
       </div>
   )
   
   //babel编译后：
   return (
       React,createElement(
           'div',
           {
               className:'my-App',
           },
           React.CreateElemnt(
               'Header',
               null,
               '头',
           ),
           React.createElement(
               'div',
                null,
               'start',
           ),
           'Right')
       )
   //创建DOM节点的三个属性 type:标签 attributes:标签属性，若无则null
   //children:标签的子节点
   ```

##### 8.React.memo、useMemo、useCallBack作用及区别

* `React.momo`是一个高阶函数组件，将接受的Props进行浅比较，只有当接收的props改变（与上次不相等）时才重新执行渲染
* `useMemo`是一个hooks，有两个参数，监听第二个参数数组值改变，若改变了则更新上次缓存的值返回

![1712834628751](C:\Users\xiaoha\AppData\Roaming\Typora\typora-user-images\1712834628751.png)

##### 9.React.useCallback()和React.useMemo()的区别

`useCallBack`可缓存函数，用于避免每次重新渲染后去重新执行一个新的函数

`useMemo`缓存记忆一个值或者计算结果

**注意**

`useEffect`中使用某个定义的外部函数，添加到监听`deps`数组中，这个定义的函数每次重新执行渲染时都是一个全新的函数，也就是引用地址发生了变化，会导致useEffect重复执行，可以通过`useCallback`缓存函数方法，避免每次重新执行新的函数

##### 10.React.forwardRef是什么及其作用

`React.forwardRef`会创建一个React组件，该组件可以将ref转发到该组件树下的另一组件

```react
const FancyButton = React.forwardRef((props,ref)=>{<Button ref={ref}>
        {props.children}
            </Button>})
const myRef = React.createRef()
//myRef可以获取到Button的DOM元素实例
<FancyButton ref={myRef}>
   Click</FancyButton>
```

CBS8业务代码使用

```react
//子组件
const Child = ForwardRef((props,ref)=>{
const {title,columns} = props
useImperativeHandle(ref,()=>{
    clearTable,
})
    const clearTable =(e)=>{
    console.log('点击事件触发')    
    }
return (
    <Table columns={columns} title={title}/>
)
})
    
const Parents =()=>{
   const myRef = useRef(null)
   const myClick =()=>{
       myRef?.current.clearTable()
   }
    (
        <button OnClick={myClick}>
        一键清除</button>
        <Child ref={myRef}/>
    )
}
```

##### React.lazy懒加载的使用说明

允许定一个动态加载的组件。优化缩减bundle的体积，并延迟加载在初始渲染时未用到的组件

```react
//使用之前
import OtherComponent from './OtherComponent';
//使用懒加载后
const OtherComponet = React.lazy(()=> import ('./OtherComponent'))
```

`React.lazy`接受一个函数，这个函数需要动态调用`import()`必须返回一个`promise`,该`promise`需要`resolve`一个`default export`的`React`组件

然后应该在`Suspense`组件中渲染`lazy`组件，`suspense`可以支持在组件在懒加载时显示缓冲展示如`loading`效果等

其中`fallback`属性接受任何在组件加载过程中需要展示的组件

```react
function lazyComponent(){
    return(
        <div>
            <Suspense fallback={(<div>
            loading...</div>)}>
           <OtherComponent/>
            </Suspense>
        </div>
    )
}
```

##### 性能优化

* 防抖和节流
* 使用`React.memo`来缓存组件、使用`useMemo`缓存大量计算数据值
* 避免使用匿名函数
* 使用`React.lazy`和`React.Suspence`延迟加载不是立即需要的组件
* 减少回流（重排）和重绘
* 事件委托
* 减少DOM操作
* 按需加载，如React中使用`React.lazy`和`React.Suspense`,通常需要与`Webpack`中的`splitChunks`配合使用

webpack方面：

* 压缩代码文件，在webpack中使用`terser-webpack-plugin`压缩，JavaScript代码使用`css-minimizer-webpack-plugin`压缩CSS代码,使用`html-webpack-plugin`压缩html代码

其他：

* 使用http2
* 使用服务端渲染
* 图片压缩
* 使用http缓存，如服务端响应：`Cache-Control/Expires`

##### react 生命周期

加载：contructor getDrivedStateFormProps render conponnentDidmount

更新 getDrivedStateFromProps shouldComponent render getsnapShotbeforeUpdate componentDidUpdate

卸载 componentWillUnmount

##### webpack面试题

1. 常见的loader，用过哪些loader

   * `raw-loader`加载文件原始值
   * `file-loader`文件输出到文件夹中，在代码中通过URL去引用输出的文件（处理图片和字体）
   * `url-loader`与`file-loader`类似，区别是用户可以设置一个阈值，大于阈值会交给file-loader处理，小于阈值时返回文件base64形式编码（处理图片和字体）
   * `source-map-loader`加载额外的Source Map文件，以方便断点调试
   * `svg-inline-loader`将压缩后的SVG内容注入代码中
   * `image-loader`加载并且压缩图片文件
   * `json-loader`加载JSON文件（默认包含）
   * `babel-loader`将ES6转成ES5
   * `ts-loader`将`Typescript`转成`javaScript`
   * `sass-loader`将`sass/scss`转成`css`
   * `style-loader`把CSS代码注入到javaScript中，通过DOM操作去加载CSS
   * `eslint-loader、tslint-loader`通过ESlint或者TSlint检查JS、TS代码

2. 常见的Plugin，及使用过哪些Plugin

   * `define-plugin`定义环境变量
   * `ingnore-plugin`忽略部分文件
   * `html-webpack-plugin`简化HTML文件创建

3. 说说loader和plugin的区别？

   `loader`本质是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果

   `plugin`就是插件，基于事件流框架`Tapable`,插件可以扩展`Webpack`的功能，通过webpack提供的API改变输出的结果

   `loader`在module.rules中配置，作为模块的解析规则，类型为数组，每一项是一个obj，内部保函test、loader、option等属性

   `Plugin`在Plugin中单独配，类型为数组，每一项为Plugin的实例

4. webpack的构建流程

   1. 初始化参数
   2. 开始编译
   3. 确定入口文件
   4. 编译模块
   5. 完成编译模块
   6. 输出资源
   7. 输出完成

   * 初始化：启用构建build，读取与合并配置参数，加载plugin实例化Compiler
   * 编译：从Entry出发，针对每个Module串行调用对应的Loader去翻译文件内容，再找到该Module依赖的Module，递归地进行编译处理
   * 输出：将编译后的Module组合成Chunk，将Chunk转换成文件，输出到文件系统中

5. source map是什么？生产环境怎么用？

   source map 是将编译、打包、压缩后的代码映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码需要使用source map

6. 模块打包原理？

   Webpack实际上为每个模块创造了一个可以导入和导出的环境，本质并没有修改代码的执行逻辑，代码执行顺序与模块加载顺序也完全一致

7. 文件监听原理

   Webpack开启监听模式，有两种方式：

   * 启动webpack命令，watch参数
   * 配置webpack.config.js中设置watch：true

##### useContent

1. 创建content！hooks=> createPayInfoContext.ts

```javascript
/*
 * @Author: 韩静文
 * @Date: 2024-11-28 14:43:56
 * @Description:
 */
import type { FormInstance } from 'antd/lib/form'
import React from 'react';

export const createPayInfoContext = React.createContext<FormInstance<any> | null>(null)
```

2. 创建content的component！CreatePayInfoContext

```react
import { createPayInfoContext } from "../../hooks/createPayInfoContext"
//children特指子组件

export const CreatePayInfoContext: React.FC<any> = ({ children, ...props }) => {

    return (
        <createPayInfoContext.Provider value={props}>
            {children}
        </createPayInfoContext.Provider >

    )

}
```

index.tsx

3. 使用contentComponent 传入属性

```react
  {/* 建立收款人组件 */}
 <CreatePayInfoContext ColSpan={ColSpan} form={form}>
       <AccountTypeInfo ColSpan={ColSpan} form={form} />
 </CreatePayInfoContext>
```

AccountTypeInfo=?children=>component

src\pages\EmployeeManagement\Edit\accountTypeInfo.tsx

4. 在孙子组件中获取provider的props

```javascript
const { form } = useContext(createPayInfoContext)
```

