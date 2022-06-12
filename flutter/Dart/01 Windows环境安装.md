### 一、chocolaty 安装

+ https://chocolatey.org/
+ https://chocolatey.org/install

#### 1. 启动Windows PowerShell，以管理员身份运行

#### 2. 输入并等待安装

```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); choco feature enable -n allowGlobalConfirmation
```

#### 3. 验证安装完成

```shell
choco -v //查看版本信息
```



### 二、安装dart

#### 1.以管理员身份运行cmd

```shell
choco install dart-sdk
choco install dartium
```

#### 2.Dart SDK和Dartium

+ Dart SDK 里面有DartVM、核心库、命令行工具，如dart、pub、dartdoc等
+ Dartium是Chromium的一个特殊版本，包含DartVM，使用Dartium不必将代码编译为JS就能调试程序。