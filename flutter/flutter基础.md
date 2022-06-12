#### 目录结构

##### 目录结构

| 文件夹       | 作用                           |
| :----------- | ------------------------------ |
| android      | android平台相关配置和代码      |
| ios          | ios平台相关配置和代码          |
| lib          | flutter相关代码                |
| test         | 测试代码                       |
| pubspec.yaml | 项目配置文件，包含库的依赖管理 |

##### 入口文件

```dart
//main.dart
void main() => runApp(MyApp());
//runApp 入口方法
```



#### Container和Text组件

##### Container

| 名称       | 功能                                                         |
| :--------- | ------------------------------------------------------------ |
| alignment  | topCenter：顶部居中对齐<br>topLeft：顶部左对齐<br/>topRight：顶部右对齐<br/>center：水平垂直居中对齐<br/>centerLeft：垂直居中水平居左对齐<br/>centerRight：垂直居中水平居右对齐<br/>bottomCenter：底部居中对齐<br/>bottomLeft：底部居左对齐<br/>bottomRight：底部居右对齐<br/> |
| decoration | decoration                                                   |
| margin     | EdgeInsets.all(20.0)                                         |
| padding    | EdgeInsets.all(10.0)                                         |
| transform  | 旋转                                                         |



##### Text

| 名称            | 功能                                                         |
| --------------- | ------------------------------------------------------------ |
| textAlign       | 文本对齐方式(center居中，left居左，right右对齐，justify两端) |
| textDirection   | 文本方向(**ltr**从左至右，**rtl**从右至左)                   |
| overflow        | 文字超出屏幕之后处理方式(clip裁剪，fade渐隐，ellipsis省略)   |
| textScaleFactor | 字体显示倍率                                                 |
| maxLines        | 文字显示最大行数                                             |
| style           | 字体的样式设置                                               |



#### 图片

| 名称                 | 类型      | 说明                                                         |
| -------------------- | --------- | ------------------------------------------------------------ |
| alignment            | Alignment | 图片对齐方式                                                 |
| color colorBlendMode |           | 设置图片背景颜色，通常和colorBlendMode配合一起使用，可以是图片颜色和背景色混合(BlendMode) |
| **fit**              | Boxfit    | **fit**属性用来控制图片的拉伸和挤压，根据**父容器设置**，相对父容器大小<br>**BoxFit.fill**：全图显示，图片会被拉伸，并充满父容器<br>**BoxFix.contain**：全图显示，显示原比例，可能会有空隙<br>**BoxFix.cover**：显示可能拉伸，可能裁切 |



##### 网络图片

```dart
//设置图片圆形
//方法一
return Center(
    child: Container(
        width: 300,
        height: 300,
        decoration: BoxDecoration(
            color: Colors.yellow,
            borderRadius: BorderRadius.circular(150),
            image: DecorationImage(
                image: NetworkImage("https://images.pexels.com/photos/10447149/pexels-photo-10447149.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500"),
                fit: BoxFit.cover
            )
        ),
    ),
);
//方法二
return Center(
    child: Container(
        child: ClipOval(
            child: Image.network(
                "https://images.pexels.com/photos/10447149/pexels-photo-10447149.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500",
                width: 150,
                height: 150,
                fit: BoxFit.cover,
            ),
        ),
    ),
);
```

##### 本地图片

```dart
return Center(
    child: Container(
        child: ClipOval(
            child: Image.asset(
                "asset/images/px_start.png",//本地图片路径，要在配置文件中配置
                width: 150,
                height: 150,
                fit: BoxFit.cover,
            ),
        ),
    ),
);
//pubspec.yaml
 assets:
    - asset/images/
```



#### 列表

+ 垂直列表
+ 垂直图文列表
+ 水平列表
+ 动态列表
+ 矩阵式列表



##### ListView 

###### 列表ListView参数

| 名称            | 类型               | 说明                                               |
| --------------- | ------------------ | -------------------------------------------------- |
| scrollDirection | Axis               | Axis.horizontal 水平列表<br>Axis.vertical 垂直列表 |
| padding         | EdgeInsetsGeometry | 内边距                                             |
| resolve         | bool               | 组件反向排序                                       |
| children        | List<Widget>       | 列表元素                                           |

```dart
//非动态列表
return ListView(
    padding: EdgeInsets.all(10),
    children: <Widget>[
        ListTile(
            leading: Icon(
                Icons.ac_unit_sharp,
                color: Colors.green,
                size: 40,
            ),
            title: Text(
                '华北黄淮持续高温',
            ),
            subtitle: Text("北方今年首轮大范围高温拉开序幕"),
            trailing: Icon(Icons.arrow_forward_ios_sharp,size: 15),
        )
    ],
);
```

###### 动态列表数据渲染

```dart
//引入外部数据
//方法一 直接返回
class HomeContent extends StatelessWidget{
  List<Widget> _getData(){
    var tempList = listData.map((value) {
      return ListTile(
        leading: Image.network(
          value["imageUrl"],
          fit: BoxFit.cover,
          width: 50,
          height: 50,
        ),
        title: Text(value["title"]),
        subtitle: Text(value["author"]),
      );
    });
    return tempList.toList();
  }

  @override
  Widget build(BuildContext context) {
    return ListView(
      children: this._getData(),//执行此方法，把数组赋值给了children
    );
  }
}

//方法二 使用builder
class HomeContent extends StatelessWidget{
  Widget _getListData(context,index){
    return ListTile(
      leading: Image.network(
          listData[index]["imageUrl"],
          width: 50,
          height: 50,
          fit:BoxFit.cover
      ),
      title: Text(
        listData[index]["title"]
      ),
      subtitle: Text(
        listData[index]["author"]
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: listData.length,
      itemBuilder: this._getListData//加()表示执行此方法，所以不需要写成this._getListData()
    );
  }
}
```



##### GridView列表



#### 自定义导航栏

bottomNavigationBar 常见属性

| 属性名       | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| items        | List<BottomNavigationBarItem> 底部导航条按钮集合             |
| iconSize     | icon                                                         |
| currentIndex | 默认选中第几个                                               |
| onTap        | 选中变化回调参数                                             |
| fixedColor   | 选中的颜色                                                   |
| type         | BottomNavigationBarType.fixed<br>BottomNavigationBarType.shifting |

AS**快捷键：**stful 快速生成StatefulWidget

##### 实现底部菜单栏

+ 目录结构

  ![flutter_01_tabs](H:\code\mdnote\asset\flutter_01_tabs.png)

+ 实现

  ```dart
  //01. main.dart
  import 'package:flutter/material.dart';
  import 'pages/Tabs.dart';
  
  void main() => runApp(MyApp());
  class MyApp extends StatelessWidget{
    @override
    Widget build(BuildContext context){
      return MaterialApp(
        debugShowCheckedModeBanner: false,
        home: Tabs()//Tabs
      );
    }
  }
  
  //02. 定义Tabs
  //Tabs.dart
  import 'package:flutter/material.dart';
  import 'tabs/Home.dart';
  import 'tabs/Category.dart';
  import 'tabs/My.dart';
  
  class Tabs extends StatefulWidget {
    const Tabs({Key? key}) : super(key: key);
    @override
    _TabsState createState() => _TabsState();
  }
  
  class _TabsState extends State<Tabs> {
    //定义一个私有变量
    int _currentIndex = 0;
    List _pageList = [
      HomePage(),
      CategoryPage(),
      MyPage()
    ];
  
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text('Flutter Demo'),
        ),
        body:this._pageList[this._currentIndex],
        bottomNavigationBar: BottomNavigationBar(
          // type: BottomNavigationBarType.fixed,//四个tabbar要添加此配置
          currentIndex: _currentIndex,
          onTap: (int index){
            //使用setState重新渲染组件
            setState(() {
              this._currentIndex = index;
            });
          },
          items: [
            BottomNavigationBarItem(
                icon:Icon(Icons.home),
                label:"首页"
            ),
            BottomNavigationBarItem(
                icon:Icon(Icons.category),
                label:"分类"
            ),
            BottomNavigationBarItem(
                icon:Icon(Icons.person),
                label:"我的"
            )
          ],
        ),
      );
    }
  }
  
  //03. Home.dart 其它同理
  import 'package:flutter/material.dart';
  class HomePage extends StatefulWidget {
    const HomePage({Key? key}) : super(key: key);
    @override
    _HomePageState createState() => _HomePageState();
  }
  class _HomePageState extends State<HomePage> {
    @override
    Widget build(BuildContext context) {
      return Text("我是首页");
    }
  }
  
  ```

  

#### 路由

页面跳转，在Flutter中通过Navigator组件管理路由导航，并提供了管理堆栈的方法，如Navigator.push和Navigator.pop。

配置路由的两种方式：

##### 普通路由

```dart
import '../Search.dart';
ElevatedButton.icon(
    icon: Icon(Icons.send),
    label: Text("发送"),
    onPressed:(){
        Navigator.of(context).push(
            MaterialPageRoute(
                builder: (context)=> SearchPage() //页面跳转
            )
        );
    },
),
//此时Navigator传入的context必须是StatefulWidget的
```

###### 普通路由传参

+ StatelessWidget

  ```dart
  //跳转到页面B
  class FormLessPage extends StatelessWidget {
    String title = '';//step 1
    FormLessPage({this.title = '表单默认标题'});//step 2
  
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text(this.title),//step 3
        ),
        body: ListView(
          children: <Widget>[
            ListTile(title: Text("我是表单传参页面"))
          ],
        ),
      );
    }
  }
  
  ```

+ StatefulWidget

  ```dart
  //从页面A
  ElevatedButton(
      child: Text("跳转传参"),
      onPressed: (){
          Navigator.of(context).push(
              MaterialPageRoute(
                  builder:(context)=> FormPage(title:"跳转传参") //step0
              )
          );
      },
  )
      
  //跳转到页面B
  class FormPage extends StatefulWidget {
    final String title ;//step1
    const FormPage({Key? key, required this.title}) : super(key: key);//step2
    @override
    _FormPageState createState() => _FormPageState();
  }
  
  class _FormPageState extends State<FormPage> {
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: Text(widget.title),//step3
        ),
        body: ListView(
          children:<Widget> [
            ListTile(title: Text("我是表单页面"),),
          ],
        ),
      );
    }
  }
  ```

  

##### 命名路由





