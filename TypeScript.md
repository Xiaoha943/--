### TypeScript

##### 基础类型:

`string、number、boolean、Null、undefined、symbol`

数字：

```typescript
let age:number = 18
```

字符串：

```typescript
let name:string -'hanjingwen'
```

模板字符串：

```typescript
let age:number = 18
let name:string ='hanjingwen'
let sentence:string = `Hello,my name is${name},I'll be ${age+1}years old next month`
```

数组：

```typescript
let list:number[] =[1,2,3]
let list:Array<string> =['hanjingwen','yangjing']
```

元组：

元组允许表示一个已知元素数量和类型的数组，各元素的的类型不必相同

```typescript
let x:[string,number] = ['hanjingwen',23]
```

枚举：

`enum`类型是对`javascript`标准数据类型的一个补充，使用枚举类型可以为一组数值赋予友好的名字

```typescript
enum Color {Red=1,Green=2,Blue=4}
let c:Color = Color.Green;
let colorName:Color = Color[2]
console.log(colorName) //'Green'
```

`Any`:

不清楚类型的变量指定的类型，这些值可能是来自于动态的内容，不希望进行类型检查器对这些值进行检查

`Void`

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。当一个函数没有任何返回值则会见到其返回类型是`void`

```typescript
const option = ():void =>{
    console.log('hello world!')
}
//声明一个void类型并没有什么意义因为只能给void类型赋值undefined 和null
let name:void = undefined
let age:void = null
```

`null、undefined`

这个两个类型和`void`一样也没有什么实质性用处

```typescript
let u:null = null;
let nLundefined = undefined;
```

`Never`

`never`类型表示是那些永远不存在的值类型，`never`类型是那些总会抛出异常或根本不会返回值的函数表达式或箭头函数表达式的返回值类型

```typescript
function fail():never{
    return error('抛出错误！')
}
```

`Object`

类型断言

```typescript
const name:any = 'hanjingwen'
//方法一
const nameLength = (<string>name)?.length
const myNameLength = (name as string)?.length
```



##### 接口

```typescript
interface myFace{
    name?:string,
    age?:number,
}
const option =(value:myFace):void=>{
    console.log(value?.name)
}
let option({name:'hanjingwen'})//hanjingwen
```

只读属性

```typescript
interface point{
    readOnly x:string,
    readonly y:number
}

const arr:ReadOnlyArray<string> =['hanjingwen','yangjing','admin']
arr[0] = root //error
```

除了存在的width和age的任意其他属性限定

```typescript
interface other{
    width:string,
    age:number,
    [propName:string]:any,
}
```

函数类型

```typescript
interface FuncOption {
    (name:string,age:number):void
}
let option:FuncOption
option = (name:string,age:number)=>{
    console.log(`我的名字叫${name},我今年${age}岁了`)
}
```

接口继承

```typescript
interface parents{
    name:string,
    age:number,
}
interface child extends parents{
    sex:string
}

let person:<child>{}
person.name ='hanjingwen'
person.sex ='女'
```

##### 函数

函数类型

```typescript
const add = (x:number,y:string):string =>{
    return x+y
}
add(x:number,y:string):string{
    return x+y
}     
```

推断类型

如果发现赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript会自动识别出类型

可选参数和默认参数

TypeScript中每个函数的参数都是必须得，但是其实是可以传递null和undefined的，传递给一个函数的参数个数必须与函数期望一致

```typescript
const myFunc =(name:string,age:number,sex:string):number=>{
    return age
}
myFunc('hanjingwen',12)//error
myFunc('12')//error
myFunc('hanjingwen',12,女)//12
```

函数参数可以使用问号表示参数是否是可选的，并且带有问号可选的参数必须放在函数必输参数最后

````typescript
const myfunc = (name:string,age:number,sex?:string) =>{
    return name
}
myFunc(12) //error
myFunc('hanjingwen',12)//'hanjingwen'
````

有默认值初始化值的参数：为参数提供一个默认值当用户没有传递这个参数或者传递的值是`undefined`，带默认值的参数不需要放在必须参数的后面

```typescript
const myFunc = (name:string='hanjingwen',age:number):string=>{
    return name+age
}
myFunc('Bob',12)//'Bob12'
myFunc('amy')
```

剩余参数

```typescript
const buildName =(firstName:string,...rest:string[])=>{
    retrun firstName+''+ rest?.join('')
}
```

##### 泛型

泛型类型将类型可复用，将类型作为参数传入接口等类型限定中

```typescript
interface optionProps<T> {
    arg:T[],
    otherList?:Array<T>
    name?:T 
}

const myFunc:<T> = (arg:T[]):T[]=>{
    return [...arg]
}

let myIdentity:optionProps<string> = myFunc;

```

泛型约束

```typescript
interface myType{
    length:number
}
const myFunc:<T extends myType> = (arg:T):T=>{
    console.log(arg?.length)
}
myFunc({length:10,value:'显示'})
```

##### 枚举

数字枚举：

```typescript
enum Response{
    Yes=1,
    No=0,
}
console.log(Response.Yes)
```

字符串枚举

```typescript
enum Str{
    UP:'UP',
    DOWN:'DOWN',
}
```

若同一个枚举中，上面的数字常量初始化了，但是后面的数字常量没有初始化则会报错

```typescript
enum err{
    X= getSomeValue(),
    Y,//error,X初始化了，但是Y并没有初始化
}
```

##### 类型推论

```typescript
let x = 3; //类型推论为number
let x =[0,1,null];//类型推论为<number|null>
let zoo:Animal[] = [new Rhino(),new Elephant(),new Snake()]
//类型推论为(Rhino|Elephant|Snake)[]
```

##### 类型兼容性

```ty

```











