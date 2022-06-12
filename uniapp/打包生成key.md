+ java环境下管理员运行cmd

  ```shell
  keytool -genkey -alias keyName -keyalg RSA -keysize 2048 -validity 36500 -keystore keyName.keystore
  ```

  

+ 查看key

  ```shell
  keytool -list -v -keystore jinkeDoctor.keystore
  ```

  

+ uniapp HBuilder云打包：证书文件不是有效地keystore文件

  在云打包时提示`证书文件不是有效地keystore文件`，更改密钥库类型解决

  ```shell
  keytool -importkeystore -srckeystore ./test.keystore -destkeystore ./test.keystore -deststoretype JKS
  ```

  

  

