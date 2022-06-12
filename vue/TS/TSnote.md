### 安装

```shell
npm install -g typescript #install
tsc -v #version
```

### 数据类型

#### 数据注解

1. TS是面向对象静态数据类型的语言，静态是指：当确定了类型的变量，则变量的类型将不可改变。类型的声明方式为：变量：类型名称 ==> 类型注解

   ```tsx
   let num:number = 100;
   num = 300;//再次赋值时必须是数值整型
   ```

2. 对函数的参数设置类型注解，函数本身返回值也可以设置类型注解

   ```tsx
   //参数是number，返回值类型是string
   function info(x:number) : string
   {
       return x + '';
   }
   ```

3. 数组注解

   ```tsx
   let arr :number[];
   arr = [1,2,3];//数组内的元素都是整型
   ```

4. 元组注解

   ```tsx
   let arr : [number,string,boolean];
   arr = [1,'2',false];
   ```

5. 特殊类型

   + any (任意值)

     ```tsx
     let other1 :any = 100;
     other1 = 'Mr.Lee';
     console.log(typeof other1);//string
     ```

   + null (不存在的对象)

   + undefined (声明但未初始化赋值)

   + void (空值，无返回)

     ```tsx
     function info() :void{
         consloe.log(123);//不能有return
     }
     ```

6. 常规类型

   + boolean(布尔)

     ```tsx
     let flag:boolean = Boolean(0);
     console.log(flag);//false
     ```

   + 字符串(string)

### 枚举

+ 枚举是包裹关联变量的一种声明方式，代码更优雅

  ```tsx
  enum Weeks {
      One = 1,
      Two,
      Three,
      Four
  }
  ```

+ 枚举的值和枚举的名之间存在映射关系，可以通过值获取枚举的名

+ 常数枚举：与普通枚举的区别：常数枚举在编译时被删除

  ```tsx
  const enum Info{
      name = '111',
      age = 100
  }
  console.log(Info.age)
  ```



### 字面量类型

```tsx
//创建一个字面量类型
let myName : 'Mr.A' = 'Mr.A';
```



### 联合类型

一般用于普通类型的多种类型声明，用“|”表示

```tsx
//声明一个变量，值可以是多种类型
let gender : '男'| '女'| '保密';//联合类型
gender = '男';
//联合类型传参
function info(other : number | string){
    return other;
}
console.log(info('Mr.Lee'));
```



### 类型推断和断言

#### 类型推断

1. 不使用类型注解的时候，TS自行对变量的类型进行推断

   ```tsx
   let other = 100;
   // other = '111';//error 已固定为number类型
   console.log(other);//100
   ```

2. 声明变量时没有进行有效赋值时，默认为any类型

3. 对于函数的参数和返回值，TS可以进行类型推断，只不过参数需要关闭严格模式(没有必要)

#### 类型断言

```tsx
function myInfo(info:number | string):number {
    //断言为string
    if((info as string ).length ){
        return (info as string).length;
    }else{
        return info.toString().length;
    }
}
```

### 类

#### 类的继承

1. TS支持多重继承（祖孙），不支持多继承（一个子类继承多个父类）

   ```tsx
   class Woman extends Person1 extends Person2 //error
   //第一重继承
   class Woman extends Person    
   //第二重继承
   class Sister extends Woman
   ```



#### 类的重写

类方法分为构造方法和普通方法，都存在重写机制。

子类的构造方法重写时：使用super关键字调用父类的构造方法

```tsx
class Man2 extends Person2{
    height: number
    //构造方法的重写
    constructor(name:string ,age:number, height:number){
        //super 关键字调用父类构造方法 获取值
        super(name,age);
        this.height = height;
    }
    //普通方法的重写 -- 采用super.run()调用父类run()方法
    run() : string {
        return super.run() +'; 身高为：' + this.height
    }
}
```



#### 类方法的重载

类方法的重载：方法相同但传递参数不同从而执行不同的一种操作。

```tsx
class Person3{
    name:string 
    age:number | undefined
    //构造方法的重载
    // ?: 表示参数可选 此时内置了一个联合类型，需要在声明参数时指定联合类型
    constructor(name : string, age ?: number){
        this.name = name //调用的成员字段
        this.age = age
    }
    //普通方法的重载
    run(flag ?: boolean):string {
        if(flag){
            return '无法显示'
        }
        if(this.age === undefined){
            return this.name +'的年龄为：保密'
        }
        return this.name +'的年龄为：'+ this.age
    }
}
let p3 = new Person3('myName',100)
console.log(p3.run(true));//无法显示
let p4 = new Person3('Miss Wang')
console.log(p4.run(false));//Miss Wang的年龄为：保密
```



#### 类成员的修饰符

##### 成员方法修饰符

+ 在不加修饰符的情况下，成员字段和方法默认是公共public完全可见的状态
+ 在其设置一下不同的可见性，共有三种方案：
  + ```public```：默认设置，公有可见性；
  + ```protected```：受保护的，自身和子类可以访问；
  + ```private```：私有的，只能自身访问

设置可见性的目的：为了保护类中的属性和方法不被污染

```tsx
class Person{
    protected name:string 
    protected age : number
    //构造方法
    protected constructor(name : string, age : number){
        this.name = name //调用的成员字段
        this.age = age
    }
    //普通方法
    run():string {
        return this.name +'的年龄为：'+ this.age
    }
}
class Man extends Person{
    public constructor(name : string, age : number){ 
        super(name,age);
    }
    //子类无法访问父类私有(private)构造方法
    //子类访问父类构造方法需要父类中参数声明为protected类型 且 父类构造方法必须为public
    run():string {
        return this.name +'的年龄为：'+ this.age
    }
}
//let p = new Person('myName',100)//error
let m = new Man('myName',100)
console.log(m.run());
```



#### 类成员的存取器

##### 成员字段访问

```tsx
//不常用 使用成员存取器
class Person {
    private name : string
    private age  : number
    //set get
    setName(name :  string){
        this.name = name;
    }
    getName() :string {
        return this.name
    }
}
p.setName('Miss.Wang');
p.getName()
```

##### 成员字段存取器*

+ 成员字段有专门的setter和getter方法来处理私有成员字段的取值和赋值（以属性的形式来调用）
+ 原有的成员字段最好改成相对应的固有名称让其一看就知道是私有成员
+ 类的内部都使用```this.私有成员```，外部用setter和getter的仿成员

```tsx
class Person {
    //声明和构造的语法糖
    constructor(private _name:string,private _age:number){}
    //setter getter
    set name(name:string){
        this._name = name;
    }
    get name(){
        return this._name;
    }
    run() : string {
        return this._age + '的年龄为：' + this._age
    }
}
let p = new Person('Mr.Lee', 100)
p.name = 'Miss.Wang'; //调用的是set name()的方法
```



#### 静态成员和方法

不需要实例化，一般用于各种工具类。

在```static```后加```readonly```表示当前静态成员只读，类似常量。

```readonly```和```const```

最简单判断该用readonly还是const的方法是看要把它作为变量使用还是作为一个属性。作为变量使用时用const，作为属性使用时用readonly。

```tsx
 class Person {
     constructor(private _name:string,private _age:number){}
     static readonly PI :number = 3.14;
     static getPI(){
         return '圆周率：'+this.PI;
     }
 }
console.log(Person.PI);//直接使用类名.调用
console.log(Person.getPI());
```



#### 抽象类的使用

1. 抽象类可用于顶层制定和设计标准，让子类继承时实现具体细节；

2. 抽象类设计相关成员和方法，不用于具体使用，所以无法实例化

3. 子类必须实现抽象类的抽象方法，严格按照标准

4. 抽象类虽然可以给子类制定标准，但自己也可以实现一些通用的公共行为

   ```tsx
   abstract class Person {
       constructor(protected name:string,protected age:number) {}
       //需要被子类实现的方法--抽象方法
       //定义了一个规范，所有子类必须实现run()方法
       abstract run():string;
       //抽象类的公共方法
       abc(){
           return 'abc'
       }
   }
   //子类
   class Man extends Person{
       constructor (name:string,age:number){
           super(name,age)
       }
       //子类实现抽象类的run方法
       run(): string {
           return this.name + '的年龄为：' + this.age
       }
   }
   let m = new Man('Miss.Wang',30)
   console.log(m.run());
   console.log(m.abc());
   ```

   

### 接口

#### 基本使用

接口类似于抽象类，但更加彻底，只提供标准却完全不实现细节。具体表现在成员字段不可以赋值初始化，方法不可以实现。

1. 继承只能继承一个父类，接口可以实现多个接口

2. 接口还可以继承另一个接口，从而进行合并，子类中通过接口设置可选参数决定是否完全实现。

   ```tsx
   interface Person {
       name ?: string//可选
       age ?: number //可选
       run ?() : string//可选
   }
   interface Abc extends Person {
       def () : string
   }
   class Man implements Abc {
       def () : string {
           return 'def'
       }
   }
   ```



#### 对象的接口类型

```tsx
interface Person {
    //[attrName : string] : any //自定义属性 string表示对象属性的类型 any表示对象中value值的类型
    [attrName : string] : number | string 
}
//对象字面量
let man: Person = {
    name :'Miss.Wang',
    age : 18,
    gender : '女'
}
```



### 泛型

参数类型不明确时，使用泛型提高灵活性。

#### 泛型的变量

#### 泛型的数组

```tsx
let arr1: Array<number> = [1, 2, 3, 4, 5]
//1
function setArr(value: any): Array<any> {
    return [value, value, value]
}
console.log(setArr('m'));
//2
function setArr1<T>(value : T) : Array<T> {
    let result : T[] = [value,value,value]
    return result
}
console.log(setArr1<string>('m'))
```

##### 多个泛型

```tsx
function more<T, P>(name: T, age: P): [T, P] {
    return [name, age];
}
console.log(more<string,number>('Miss.Wang',20));
```

