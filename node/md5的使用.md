express中md5的使用方法：

```javascript
npm install utility --save-dev


var express=require("express");
var utility=require("utility");
var app=express();


app.get("/",function(req,res){
    var name=req.query.name;
    console.log("receive name info:"+name);
    var sha1Value=utility.sha1(name);
    res.send("your name is :"+sha1Value);
    var md5Value=utility.md5(name);
   res.send("your name is :"+md5Value);
});


app.listen(3000,function(){
    console.log("server is running ......");    
});
```

