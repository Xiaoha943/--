

## Vue

#### setup函数

vue3中的新钩子函数，在组件初始化的时候调用，替代了原有的beforeCreate()和created()钩子函数

* 参数：`setup`有两个入参，`props`和`context`两个对象
  * `props`：由父组件传递下来的数据，当父组件传入数据时则会被更新，不允许解构，解构后的变量不会变化，需要使用vue3提供的toRef方法
  * `context`：组件执行的上下文，包含两个对象一个方法，可以使用解构
    * `attrs` ：表示父组件传入的props里所有属性，通过`attrs.XXX`调用，对象
    * `slots`：表示父组件传入的插槽内的组件，通过`slots.XXX`，对象
    * `emit`：表示触发父组件绑定下来的事件.可以通过解构赋值，`{emit}` 使用`emit('XXX')`调用父组件事件方法

#### defineComponent

该API用于对`Typescript`代码进行类型推导，帮助开发者简化掉很多编码过程的类型声明,同时解决vue2对于类型推导不完善的问题

```javascript
import {defineComponent} from 'vue'
export default defineComponet({
    setup(props,context){
    ...
    return {
        ....
    }
    } 
})
```

#### vue生命周期

```javascript
vue2.0                     vue3.0				执行时机说明
beforeCreate()  			setup()				组件创建前执行
Created() 					setup()				组件创建后执行
beforeMount() 				onBeforeMount()		组件挂载到节点前执行
Mounted() 					onMounted()//通常在onMounted时调用接口返回数据，更新渲染界面，因为调用接口需要在组件挂载完毕后调用						组件挂载完成后执行
beforeUpdate() 				onBeforeUdate()      组件更新之前执行
Updtated() 					onUpdtated()		组件更新完成后执行
beforeDestroy() 			onBeforeUnMount()	组件卸载之前执行
destoryed() 				onUnmount()			组件卸载后执行
actived()  					onActived()	//被包裹在<keep-alive>中的组件,被激活时执行
							onDeactivated //A组件切换到B组件时候，A组件销毁前执行
errorCaputure()  			onErrorCapture()	当组件的子孙组件发生异常触发的函数
```

注意：`vue3`中的生命周期钩子函数都要从vue导入，将生命周期函数放在setup函数中执行，若有初始化需要执行的函数方法直接放入setup中调用

#### vue2.0+TypeScript

1.Vue.extend 2.class Component

* ```javascript
  import Vue from 'Vue'
  
  const Componet = Vue.extend({
      //内部进行类型限定
  })
  ```

* ```javascript
  import Vue from 'Vue'
  import Componet from 'component-class-vue'
  
  @Component{
      template:<botton @click="getName">Hello,world</botton>
  }
  export default class MyComponent extends vue({
      myName:string = 'hanjingwen'
      getName():void{
          window.alert(this.message)
      }
  })
  ```

#### vue3+TypeScript

官方建议

* vue3+defineComponent+template
* vue3+defineComponent+TSX

最为推荐 defineComponent+compositionAPi+Template

```vue
import Vue from 'Vue'
import defineComponent from 'Vue'
<template>
    <p classname='name'>{{myName}}<p>
</template>
<script lang='ts'>
 export default defineComponent({
        setup(props,context){
            
            onBeforeMount(){
                
            }
            onMounted(){
                
            }
            onBeforeUpdate(){
                
            }
            onUpdated(){
                
            }
            onBeforeUnMount(){
                
            }
            onUnMount(){
                
            }
            onErrorCapture(){
                
            }
            const myName:string='hanjingwen'
            return{
               myName 
            }
        }
    })
</script>
<style scoped>
    .name{
        color:red
    }
</style>
```

#### ref

 `ref` 是最常用的一个响应式 API，它可以用来定义所有类型的数据，包括 Node 节点和组件 

```vue
const myRef = ref<string>('hanjingwen')
myRef.value = '123'
```

```vue
<template>
<p ref ="myPRef">我的ref节点</p>
</template>

const myPRef = ref<HTMLElemnt>()
```

#### reactive

相对于ref，reactive只能用于定义对象或者数组

```JavaScript
const myReacObj:{name:string,age:number} = reactive({name:"hanjingwen",age:12})
const myReacArr = reactive({name:"hanjingwen",age:12},{name:"wanng",age:34},{name:"cheng",age:24})
console.log(myReacObj.name)
//清除数组数据
myReacArr.length = 0
```

* 可以直接通过对象或者数组的方法获取对应的数据,直接修改对应属性值
* 数组不可以直接删除赋值为[],会导致界面不能重新渲染，需要通过修改myReacArr =[],清空数组数据
* 不可以给reactive进行解构赋值，解构后的变量会失去响应，不会重新渲染

#### 响应式API toRef和toRefs

toRef:将单个reactive内的属性转换成ref类型变量，ref.value改变，reactive也同步改变

toRefs：将reactive内的所有属性转换成ref变量

```javascript
interface Member  {name:string,age:number}
const userInfo:Member  = reactive({name:'hanjingwen',age:24})
const myNewNameRef = toRef(userInfo,'name','initValue')
const myNewRefs = toRefs(userInfo)
```

```vue
<template>
<p>我的名字是{{name}}</p>
<div>今年{{age}}</div>
</tempalte>
<script lang='ts'>
          	interface Member{
               name:string,
               age:number,
             }
            interface MemberRefs {
              name: Ref<string>
              age: Ref<number>
            }
    export default definedComponent({
         setup(props,context){
            const userInfo:Member = reactive({name:'hanjingwen',age:18})
            const userInfoRefs:MemberRefs = toRefs(userInfo)
        	mounted(){
                userInfo.name = 'han'
                userInfo.age = 24
    		}
   
            return{
                ...userInfoRefs
            }
        }
    })
</script>
```

#### 函数的申明和使用

vue2：函数需要再method方法中生命，在生命周期mounted(){...}钩子函数内通过this调用，不可以使用箭头函数

vue3：直接可以在setup(){...}中调用，在setup中return出去，支持class和函数方法，箭头函数申明

#### watch

vue2：使用watch和method方法平级进行监听

```javascript
export default{
    data(){
    return{
        a:{}
        b:'hanjing'
        c:fnuction(){}
        }
    },
     mounted(){
            this.$watch('a',function(oldvalue,newVlaue){
               //.... 
            })
        },
    watch:{
        a:(oldvalue,newVlaue){

        }
    },
    method:{

    },
}

```

vue3:

* watch在setup内调用,watch()

* watch接收三个参数，

  * 参数一：source：监听的数据源（ 是通过 [响应式 API](https://vue3.chengpeiquan.com/component.html#响应式数据的变化-new) 定义的变量（ `Ref` ），或者是一个 [计算数据](https://vue3.chengpeiquan.com/component.html#数据的计算-new) （ `ComputedRef` ），或者是一个 [getter 函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/get) （ `() => T` ）），不能是单独定义的变量
  * 参数二：回调函数（newValue,oldVaue, onCleanup:清除函数）=>{}
  * 参数三：一个对象（可选），包含几个属性值{deep：true/false(是否监听引用数据类型值改变)， immediate 是否初始化时候就立刻监听， flush：  'pre' | 'post' | 'sync' 渲染前，渲染后，渲染时}

  监听停止

  ```javascript
  export default defineComponent({
      setup(){
          const infoRefs = ref({name:'hanjingwen',age:24})
          
          let watchStop = watch(infoRefs,(newValue,oldValue,onCleanup)=>{
              //清理监听
              onCleanUp(()=>{
                  //清理
              })
              if(typeof infoRefs.value = 'function'){
                  //停止监听
                  watchStop()
                 }
          },{deep:true,immeditate:true})
          return {
              
          }
      }
  })
  ```

#### watchEffect

* 用于处理批量调用
* 不能和watch一样访问oldvalue和Newvalue
* 直接调用侦听函数watchEffect(()=>{})
* 默认执行一次，数据变化时也执行一次

####  computed 

vue2

```javascript
export default {
    const firstName = 'han'
    const lastName = 'jingwen'
    computed:{
        complateName(){
           return  this.firstName+this.lastName
        }
    }
}
```

vue3

```javascript
export default defineComponent({
    setup(){
        const firstName:Ref<string> = ref('han')
   		const lastName:Ref<string> = ref('jingwen')
        const complateName = computed(() => firstName+lastName )
        
        return {complateName,}
    }
})
```

* computed定义的变量只能通.value获取，和ref一样（在template都不需要使用ref.value或者computed。value,可以通过watch监听）

* 不可修改值，是只读的

* 缺点：computed计算出来的变量是异步的，不能直接在底下方法中获取，且数据只读

* 优点：缓存值，若值未发生改变则不会重复调用computed方法，性能优化

* 使用computed代替watch提高性能

  ```javascript
  export default defineComponent({
      setup(){
          let firstName= ref<string>('han')
     		let lastName= ref<string>('jingwen')
          const complateName = computed({
              get:() => firstName+lastName
              set:(newName)=>{
             		firstName.value = newName
          }
          })
      	firstName.value = chen
          //chenjingwen
          return {complateName,}
      }
  })
  ```

#### 指令

常用指令

v-model

v-bind 为:

:key

v-if

v-for

v-html

v-text

v-on：@click

v-slot ：# 具名插槽

:class   `:class="{ cur: index === curIndex }" `

:style `  :style="{      fontSize: '13px',      'line-height': 2,      color: '#ff0000',      textAlign: 'center',    }" `

{{}}

#### 自定义指令

derectives中申明自定义指令，和setup函数平级

```vue
<template>
<p v-heightcolor></p>
<p v-heightcolor>{{msg}}</p>
</tempalte>
<script lang='ts'>
    export default defineCompoent({
        derictives:{
            //derectives下每一个函数自定义指令
            heightcolor:()=>{
                mounted(el,binding){
                    el.style.backgroundColor = typeof binding.value === 'string'?binding.value:'unset'
                }
            }
        }，
        setup(){
        const msg = ref<string>('hello world')
        	return{
                msg
            }
        
    	}
    })
</script>
```

:tipping_hand_woman:全局指令可以在main.ts内启用，deep属性用于 updated和 onbefoeUpdate监听嵌套的对象，deep设定为true，更新的钩子函数才会触发

#### 路由的使用

```typescript
const routes:Array<RouteRecordRaw> = [
    //一级路由
    {
        path:'./',
        name:'home',
        component:homeView
    }.
    //路由懒加载，通过import导入路由地址文件
    //字段 component 接收一个函数，在 return 的时候返回模板组件，同时组件里的代码在打包的时候都会生成独立的文件，并在访问到对应路由的时候按需引入。
     {
        path:'./about',
        name:'about',
        component:()=>import('../views/AboutView.vue')
    },
    
]
const router = createRouter({
    history:createWebHistory(import.meta.env.BASE_URL),
    linkActiveClass: 'cur',
  	linkExactActiveClass: 'cur',
    routes
})
export default router
```

#### 路由懒加载

component接收一个函数，在return的时候返回模板组件，同时组件里的代码在打包的时候会生成独立的文件，并在访问到对应路由的时候按需引入。

```javascript
  {
        path:'./about',
        name:'about',
        component:()=>import('../views/AboutView.vue')
    }
```

#### 三级路由

通过children:[{}]展示对应子路由

:tipping_hand_woman:注意其中子路由的path不需要使用`/`否则是从一级路由开始

```javascript

    path: '/lv1',
    name: 'lv1',
    component: () => import('@views/lv1.vue'),
    // 注意：这里是二级路由，在 `path` 的前面没有 `/`
    children: [
      {
        path: 'lv2',
        name: 'lv2',
        component: () => import('@views/lv2.vue'),
        // 注意：这里是三级路由，在 `path` 的前面没有 `/`
        children: [
          {
            path: 'lv3',
            name: 'lv3',
            component: () => import('@views/lv3.vue'),
          },
        ],
      },
    ],
  },
]
```

#### 使用route获取路由信息

route和Router需要路由导入,并从setup导出

```vue
<template>
<router-link to='/home'>首页
    </router-link>
</template>

setup(){
const route = useRoute()
const router = useRouter()
	return{
		route,
		router,
	}
    //跳转首页
	router.push({
		path:'home'
		params:{
		id:123,
	}
	})
	//返回上一页
	router.back()
}
```

```javascript
//不使用a标签跳转
<template>
  <router-link to="/home" custom v-slot="{ navigate }">
    <span class="link" @click="navigate"> 首页 </span>
  </router-link>
</template>
```

#### 路由重定向

redirect

| string   | 另外一个路由的 `path`                                        |
| -------- | ------------------------------------------------------------ |
| route    | 另外一个路由（类似 `router.push`）                           |
| function | 可以判断不同情况的重定向目标，最终 `return` 一个 `path` 或者 `route` |

```vue
const routes: Array<RouteRecordRaw> = [
  // 重定向到 `/home`
   {
    path: '/',
    redirect: {
      name: 'home',
      query: {
        from: 'redirect',
      },
    },
  },
  // 真正的首页
  {
    path: '/home',
    //路由别名
    alias: '/index',
    name: 'home',
    component: () => import('@views/home.vue'),
  },
  //404界面配置
  {
    path: '/:pathMatch(.*)*',
    name: '404',
    component: () => import('@views/404.vue'),
  },
]
```

#### 导航守卫

| 可用钩子      | 含义         | 触发时机                                               |
| :------------ | :----------- | :----------------------------------------------------- |
| beforeEach    | 全局前置守卫 | 在路由跳转前触发                                       |
| beforeResolve | 全局解析守卫 | 在导航被确认前，同时在组件内守卫和异步路由组件被解析后 |
| afterEach     | 全局后置守卫 | 在路由跳转完成后触发                                   |

##### 路由拦截：beforeEach

在进入界面渲染前进行一些拦截操作

 to.matched.length 等于param.length用于判断路由参数的长度

| 参数 | 作用                   |
| :--- | :--------------------- |
| to   | 即将要进入的路由对象   |
| from | 当前导航正要离开的路由 |

```javascript
router.beforeEach((to, from) => {
    //是否登录，没登录跳转登录界面
  const { isNoLogin } = to.meta
  if (!isNoLogin) return '/login'
    //显示默认标题
  const { title } = to.meta
  document.title = title || '默认标题'
})
```

##### afterEach在路由跳转完成后后，默认发送数据。添加埋点

#### 路由独享钩子 beforeEnter

```javascript
const routes: Array<RouteRecordRaw> = [

  {
    path: '/home',
    //路由别名
    alias: '/index',
    name: 'home',
    component: () => import('@views/home.vue'),
    beforeEnter:(from,to)=>{
    switch(to.path){
        case 'home':
      		  document.title = '首页';
       		  break;
        case 'login':
        	  document.title = '登录';
       		  break;
        
    	}
    }
  },
 
]
```

#### 组件单独使用钩子

| 可用钩子            | 含义             | 触发时机                               |
| :------------------ | :--------------- | :------------------------------------- |
| onBeforeRouteUpdate | 组件内的更新守卫 | 在当前路由改变，但是该组件被复用时调用 |
| onBeforeRouteLeave  | 组件内的离开守卫 | 导航离开该组件的对应路由时调用         |

#### vue的组件传值

1. 父级向子级传值，通过Props传值、或者通过ref传递数据
2. 子向父传值，子级通过this.emit(‘回调函数方法名’，传出去的值)发射器，父级组件< @回调方法名=‘父组件方法’>，fnc（data）{ 其中data为子组件传给父组件的方法}
3. 兄弟组件传值，通过共同的父级

父到子：

* Props 父·template :userInfo='userinfo'  子 props：{userInfo:userInfo}

* ref  父`template: ref='child' component:{child} const child = ref<InstanceType<typeof Child>>()  child.value.change(newValue)` 子：function change(value){console.log(value)}

* v-model

  ```javascript
  //Father.vue
  <template>
    <Child
      v-model:uid="userInfo.id"
      v-model:username="userInfo.name"
      v-model:age="userInfo.age"
    />
  </template>
  
  // Child.vue
  export default defineComponent({
    props: {
      uid: Number,
      username: String,
      age: Number,
    },
    // 注意这里的 `update:` 前缀
    emits: ['update:uid', 'update:username', 'update:age'],
    setup(props, { emit }) {
      // 2s 后更新用户名
      setTimeout(() => {
        emit('update:username', 'Tom')
      }, 2000)
    },
  })
  ```

子到父：

* 父template @get-name=‘getName’ function getName(value){console.log(value)} 子 emits:['get-name'] emit(get-name,'newName')

父到孙：

* provider inject  父级provider方法 provider('msg',msg)，子级通过inject调用方法 const msg = inject <Ref<string>>() 函数方法 const getName = reject <()=>void>()

兄弟：

* 通过共同的父级传输

* EventBus.on 注册方法，eventBus.off 销毁方法 eventBus.emit()调用方法  EventBus.on('getName',(value)=>{console.log(value)}) EventBus.emit('getName',newValue)

* Reactive state 

  ```javascript
  export const state = reactive({
    // 设置一个属性并赋予初始值
    message: 'Hello World',
  
    // 添加一个更新数据的方法
    setMessage(msg: string) {
      this.message = msg
    },
  })
   setup() {
      console.log(state.message)
      // Hello World
  
      // 因为是响应式数据，所以可以侦听数据变化
      watch(
        () => state.message,
        (val) => {
          console.log('Message 发生变化：', val)
        }
      )
  
      setTimeout(() => {
        state.setMessage('Hello Hello')
        // Message 发生变化： Hello Hello
      }, 1000)
  
      setTimeout(() => {
        state.message = 'Hi Hi'
        // Message 发生变化： Hi Hi
      }, 2000)
    },
  ```

  

#### vue2和vue3的区别

1. 2需要根节点，3可以支持多个根节点，使用<fragement/>标签包裹，实现高效渲染
2. v-if和v-for的优先级变更，2中for高于if且可以同时使用但不建议，3 if 大于for 且不能同时使用 for需要使用在外层新建标签
3. 生命周期变化
4. 虚拟DOMdiff算法变化，2中使用patch对象将每一个element元素的变更状态存储下来，实时对比更新，3使用的patchFlag判断每个元素是否变更，变更的元素才进行实时对比更新
5. 3添加了watchEffect()和hooks
6. vue3使用的组合式API 2是选项式API,组合式更适用于复杂逻辑的提取与读写，比较易于理解
7. 更小更轻便
8. 更适用于TypeScript

#### 什么是双向数据绑定

MVVM view和viewModel进行双向数据绑定，viewmodel和Model进行业务数据数据库逻辑更新

通过Object.definedProperty('obj','foo',()=>{

set:(newValue)=>{

textInput.value = newValue

textInput.innerText = newValue

}

})

textInput.addEventListener('onKeyUp',(e)=>{

obj.foo = e.target.value

})

#### 为什么react和vue项目中列表组件要写key，其中作用是什么？

对于react和vue这种通过diff算法对比新旧虚拟DOM节点进行渲染的框架，如果不给key的话，那就没办法找到旧节点和新节点进行对比渲染，key的作用就是更新组件时候判断两个节点是否相同，相同就直接复用，不相同就删除旧的新增新的节点。

#### vue的双向数据绑定是如何实现的，使用definedProperty有什么缺点，vue3对此有什么改善

vue2.0使用Object.definedProperty 的getter和setter vue3.0使用proxy.definedProperty的getter和setter

vue2.0直接使用Object.definedProperty的缺点有

* 不能监听数组的下标改变，Arr[i] = newDate 不会触发
* 不能监听数组长度的改变，arr.length = 10 不会触发监听
* 不能监听整个对象，只能监听对象的属性，监听对象需要遍历对象，或者深度遍历监听
* 使用Object.assign()合并对象，不会触发监听

proxy可以进行代理，不需要遍历所有的属性，通过冒泡进行遍历

**VDOM重构2.0和3.0的diff算法修改变化**

2.0 watcher监听组件级别的变化，该组件的子组件其中之一变化，则整个组件重新生成新虚拟DOM进行diff对比再dispath到界面更新

3.0增加了静态标记，子组件中存在静态组件则不进行对比变化，讲静态节点缓存下来，只对比动态变化的子组件，使用了静态缓存及静态提升。

```vue

```

#### vue3新特性

* vue2.0使用Object.definedProperty 的getter和setter vue3.0使用proxy.definedProperty的getter和setter
* VDOM的diff算法变化
* 入口函数setup
* ref/reactive

