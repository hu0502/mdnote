## Node
### 模块系统
把独立的文件视为一个独立的模块，模块间可以相互调用。

模块系统是Node应用程序的基本组成部分。
1. 自定义模块

   引入路径中有```./```、```../```、```/```等本地路径的模块为自定义模块，非```node_modules```中的文件目录

2. 第三方模块

   ```javascript
   const deepcopy = require('deepcopy')
   ```

3. 核心模块

   fs、path、http、url、buffer等

   ```javascript
   const fs = require('fs')
   const fs = require('node:fs') //加载内置
   ```



#### 作用域

#### 模块加载方式

| CommonJS 规范                   | ESM规范                        |
| ------------------------------- | ------------------------------ |
| require 导入                    | import 导入                    |
| exports  \|  module.exports导出 | export  \| export default 导出 |

##### ESM

```.mjs```扩展名，或者修改```package.json```中的```type```值为```module```

```javascript
//utils.js 修改type值方法常用
export const deepcopy = () => {
    return 'abc';
}
//index.js
import { deepcopy } from './utils.js'
console.log(deepcopy());//abc
```



##### CommonJS

```javascript
//utils.js
const deepcopy = () => {
    return 'a b c';
}
exports.deepcopy = deepcopy //方法1
module.exports = { //方法2
    deepcopy
}

//index.js
const  { deepcopy } = require('./utils')
console.log(deepcopy());//a b c
```



### path模块

处理文件或目录路径

+ basename()：获取文件的基本名

+ dirname()：返回文件或目录所在的路径

+ extname()：返回文件的扩展名

+ isAbsolute()：判断路径是否是绝对路径

+ format()：从对象组合出完整路径

  parse()：从路径解析出信息对象

+ join()：连接

+ resolve()：将路径或者路径片段解析成绝对路径

  

### Buffer

NodeJs使用```Buffer```对二进制数据进行处理。

```Buffer```是一个内存空间——缓冲区，对临时的输入输出的饿数据进行处理。Buffer类创建存放二进制数据的缓冲区，是Node中的全局变量。



#### Buffer的创建

##### alloc(length,value)

第一个参数 指定buffer的长度 number类型的数值
第二个参数 指定buffer的值 以十六进制显示

```javascript
Buffer.alloc(6,1)
Buffer.alloc(6,16)
Buffer.alloc(6,'点赞')
//value填充的值长度不够时会循环填充
//输出
<Buffer 01 01 01 01 01 01>
<Buffer 10 10 10 10 10 10>
<Buffer e7 82 b9 e8 b5 9e>
```

##### allocUnsafe(length)

可能会创建未初始化的数据，建议使用```alloc()```

参数：指定新Buffer的长度。

##### from(value)

传入参数类型：(不能为number类型)

+ string
+ buffer
+ arrayBuffer
+ array类数组



#### Buffer实例

##### fill()

用指定值对buffer进行填充。

```javascript
/*
    参数1：要填充的值
    参数2：下标索引位置，以哪个位置开始填充
    参数3：下表索引，表示到哪个位置结束，不含结束位置
*/
const buf = Buffer.alloc(10)
buf.fill('a',2,4)
```

##### write()

将指定值写入到buffer中

```javascript
/*
    参数2：接收开始位置 下表索引
    参数3：规定最大的写入长度
*/
buf.write('abc',2,2)//<Buffer 00 00 61 62 00 00 00 00 00 00>
```

##### toString()

将buffer解析成字符串

##### subarray()

从原buffer截取一段返回新的buffer

##### indexOf()

返回检索内容第一次出现的位置索引
