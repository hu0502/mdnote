### 上传本地代码

#### 1、GitHub创建一个新的repository

#### 2、进入目录 并git bash，建立git仓库

```bash
git init
```

#### 3、将项目文件添加到仓库中

```bash
git add .
```

#### 4、提交到仓库

```bash
git commit -m "注释"
```

####5、将本地仓库关联到GitHub

```bash
git remote add origin 仓库地址
```

#### ~~6、上传GitHub之前pull一下（存疑~~

```bash
git pull origin master
```

#### 7、上传到远程仓库

```bash
git push -u origin master
```



### 更新代码

#### 1、查看当前git仓库状态

```bash
git status
```

#### 2、更新全部

```bash
git add *
```

#### 3、输入更新说明

```bash
git commit -m "更新说明"
```

#### 4、先git pull，拉取当前分支最新代码

```bash
git pull
```

#### 5、push到远程master分支上

```bash
git push origin master
```



### 将本地已有项目上传GitHub新建的仓库

//admin密钥 ： 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

https://blog.csdn.net/geoffrey00/article/details/118992633
