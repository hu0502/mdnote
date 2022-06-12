==Method 1== 

**Put router before static.**

```javascript
app.use(app.router);
app.use(express.static(__dirname + '/static'));
```

**Add ‘/\*’ handler** (don’t forget to call next())

```javascript
app.get('/*', function(req, res, next){
  res.setHeader('Last-Modified', (new Date()).toUTCString());
  next();
});
```



==Method 2==

```javascript
app.use(function(req, res, next){
  res.header("Cache-Control", "no-cache, no-store, must-revalidate");
  res.header("Pragma", "no-cache");
  res.header("Expires", 0);
  next();
});
```



==Method 3==

```javascript
app.disable('etag');
```

