# HTML中使用ts

`npm i -g typescript`

`tsc -w`

> 创建tsconfig.json文件
>
> ```json
> {
> 
> 	"include": [], //要解析的文件路径
> 	// 相对路径。**表示所有的目录 * 表示所有的文件，例："./src/**/*.ts" 意思就是src下的所有目录下的所有的ts文件
> 	"exclude": [], //不需要解析的文件路径
> 
> 	"eompilerOptions": {
> 		//项目配置
> 
> 
> 	}
> 
> }
> ```
>
> 

# 一、数据类型

## 1.1 Boolean类型

```typescript
let flag: boolean = false;
// 和Java一样定义变量时需要定义该数据类型
// 只不过Java是类型在前边 比如：boolean flag = false;
```



## 1.2 number数值类型

在ts中数值类型可以是整数、单浮点小数、或双浮点数

`  let num: number = 123 ;`



## 1.3 string字符串类型

`let str: string = 'String';`



## 1.4 symbol类型

```typescript
const sym = Symbol();
let obj = {
  [sym]: "semlinker",
};

console.log(obj[sym]); // semlinker
// Symble对象用来创建一个独立的内存地址来存储一个数据
```



## 1.5 Array类型

```typescript
const numArr: number[] = [1, 2, 3, 4, 5];// 数组类型
const numArr2: Array<string> = ['a', 'b', 'c']; // 用Array来创建数组
console.log(numArr2);
```



## 1.6 enum 类型

```typescript
 enum Enum {
     A, B = 'b', C = 1, D
 }
// 和对象的方式一样
// 如果是混合的数据类型，开头不能动
// 在enum 的对象中，可以多种基本数据类型的数据

let str1: Enum = Enum.A;
/*
这里将str1的类型定义为了Enum
那么赋值的时候str1的数据必须是Enum中取出来的数据
*/
```



## 1.7 any 和 unknown 类型

`任何数据类型`

`let arr: any || unknown = ['asdf',5,2,3,19,false]`



## 1.8 tuple 类型

```typescript
let tuple: [string, boolean]; // JavaScript中不存在tuple元组，这个是typescript专属的。
// 用来约束数组中的数据类型
tuple = ['str', false];
console.log(tuple); // ['str',false]
tuple = [false, 'str']
console.log(tuple); // 语法上是错误的
```



## 1.9 void类型

```typescript
// 在定义变量时，这个类型只能是undefined！

// 只要作用就是用于函数的返回值
// 当一个函数没有返回值的时候，它的返回值就是void空
function tryVoid(): void {
    console.log('触发了这个函数');
    // 这里没有return返回值所以需要的返回值是void空
}
tryVoid(); // '触发了这个函数'
```



## 1.10 null 和 undefined类型

这个不用多说，null为定义了但是值是空的，undefined是没有定义



## 1.11 object 类型

`let obj2: object = { name: '123', id: 1 };`

> 和JavaScript相同，可以在前边加上一个类型的约束
>
> 任何不是基本数据类型的都是对象



## 1.12 类型别名

`type 名称 = 类型 | 类型 ···`

> 例

```typescript
type aType = string | number
let a: aType = 'str'
```



## 1.13泛型

在 TypeScript 中，泛型（Generics）是一种强大的工具，可以应用于多种情况，不仅仅局限于数组。以下是一些其他常见的泛型用法：

1. **函数泛型：** 可以将泛型应用于函数，使函数能够接受不同类型的参数。

   ```
   typescriptCopy code
   function identity<T>(arg: T): T {
     return arg;
   }
   
   const result: number = identity<number>(42);
   ```

2. **接口泛型：** 可以在接口中使用泛型，以使接口能够处理不同类型的数据。

   ```
   typescriptCopy code
   interface Pair<T, U> {
     first: T;
     second: U;
   }
   
   const pair: Pair<number, string> = { first: 1, second: "two" };
   ```

3. **类泛型：** 可以在类中使用泛型，以处理不同类型的数据。

   ```
   typescriptCopy code
   class Box<T> {
     value: T;
   
     constructor(value: T) {
       this.value = value;
     }
   }
   
   const numberBox = new Box<number>(42);
   ```

4. **Promise 泛型：** 在异步编程中，可以使用 Promise 泛型指定异步操作的结果类型。

   ```
   typescriptCopy code
   const asyncFunction = (): Promise<number> => {
     return new Promise((resolve) => {
       setTimeout(() => resolve(42), 1000);
     });
   };
   
   asyncFunction().then(result => console.log(result));
   ```

5. **React 组件泛型：** 在 React 中，可以使用泛型为组件传递不同的 props 类型。

   ```
   typescriptCopy code
   interface MyComponentProps<T> {
     data: T;
   }
   
   const MyComponent = <T extends {}>({ data }: MyComponentProps<T>) => {
     // 使用泛型类型 T 处理 data
   };
   
   <MyComponent data={{ key: "value" }} />;
   ```

# 二、TypeScript 断言

## 2.1 类型断言

> 当定义了一个任意数据类型的变量想要获取这个变量中的方法时编辑器检测不到当前的数据类型
>
> 例如：`let str: any = 'string'`现在这个变量只是你自己知道是字符串，编辑器是检测不到这个是字符串的所以`str.length`虽然可以获取到长度但是编辑器不提示。现在就需要有类型断言的出现了



### 2.1.1 尖括号语句，以弃用

```typescript
let str: any = 'string';
console.log( (<string>str).length )

console.log( (str as string).length )// 这样编辑器就能识别这个是一个字符串，也就提示代码了
```



### 2.1.2 as 语法

```typescript
let str: any = 'string';
console.log( (str as string).length )// 这样编辑器就能识别这个是一个字符串，也就提示代码了
```



## 2.2  非空断言

> 在实际的场景中，现在有一个函数，函数有一个形参这个形参要赋值给函数内部的变量
>
> 例：
>
> ```typescript
> function (str: string | null | undefined){
>     let newStr: string;
>     // 要判断str是否为有效的值（非null非undefined）。用传统的方式
>     if (str){
>         newStr = str;
>     }else{
>         newStr = '无效'
>     }
>     
>     // 这种看起来不高级，所以非空断言来了
>     newStr = str!; // 在参数后添加! 就可以排除错误
> }
> ```



# 三、函数

> 函数的**参数和返回值**都可以设置**类型**

## 3.1 可选参数 \ 默认值

`function fn (a?:string){}`

> 在参数后加**问号**就是可选参数



`function fn(a:string = 'abc')`

> =等号赋值



> 默认值和可选不能同时存在



## 3.2 剩余参数

```typescript
function fn (...aggs: any[]){
    // aggs是剩余的多个参数，为一个列表
    // aggs => [1,2,3]
}
fn(1,2,3)
```

  

# 四、接口

