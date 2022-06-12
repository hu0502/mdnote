`next({...to,replace:true})：`

replace是一个布尔类型，默认为false，如果设置为true，导航不会留下history记录，点击浏览器回退按钮不会再回到这个路由。

next不能使用this 因为进入此路由时组件还没有被挂载 可以使用`vm =>{}...`

