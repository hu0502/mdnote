#### 基本类型

##### 类型声明

+ 类型声明是TS非常重要的一个特点

+ 通过类型声明可以指定TS中变量(参数、形参)的类型

+ 指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错

+ 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值

+ 语法

  ```tsx
  let a:string;
  let b:number = 1;
  function fn(c:boolean,d:number):boolean{}
  ```

##### 自动类型判断

+ TS拥有自动的类型的判断机制
+ 当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型，此时也可以省略类型声明

##### 类型

| 类型      | 举例            | 描述                         |
| --------- | --------------- | ---------------------------- |
| number    | 1,-33,2.5       | 任意数字                     |
| string    | ‘hello world’   | 任意字符串                   |
| boolean   | true false      | true或false                  |
| 字面量    | 其本身          | 限制变量的值就是该字面量的值 |
| any       | *               | 任意类型                     |
| void      | 空值(undefined) | undefined                    |
| never     | 没有值          | 不能是任何值                 |
| object    | {name:’胡五块’} | 任意的JS对象                 |
| array     | [1,2,3]         | 任意JS数组                   |
| tuple元组 | [4,5]           | 元组，可以指定不同类型元素   |
| enum      | enum{A,B}       | 枚举，新增类型               |
| unknown   | *               | 类型安全的any                |

不建议使用```any```的原因：它可以赋值给任何变量，间接影响其它变量的类型；（类型不安全）

但```unknown```一旦声明后不可以赋值给其它类型的变量，不会影响其它变量的类型状态。需要赋值给其它变量前要进行类型判断（类型安全）

###### object

语法：{属性名：属性值, 属性名?：属性值}(?表示可选)

对象结构的类型声明

```tsx
let b: { name: string, age: number };
let c: { name: string, [propName: string]: any }//自定义属性
c = { name: 'hahaha', age: 18, gender: '男' }
```

###### 函数结构的类型声明

```tsx
let d: (a: number, b: number) => number;
d = function (n1, n2) {
    return n1 + n2;
}
```

###### 数组

```tsx
let e: string[] //表示字符串数组 
let g: Array<number>;//数值类型的数组
```

类型的别名：

```tsx
type myType = 1 | 2 | 3 | 4 | 5;
let k: myType;//类型的别名 代替内容
let l: myType;
```



##### 类型断言



#### 编译选项

##### 自动编译文件

编译文件时，使用-w指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译

```shell
tsc xx.ts -w
```

##### 自动编译整个项目

+ 如果直接使用tsc指令，则可以自动将当前项目下的所有ts文件编译为js文件
+ 使用tsc指令的前提是在项目根目录下创建一个ts的配置文件tsconfig.json
