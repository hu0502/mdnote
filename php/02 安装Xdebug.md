安装：https://www.shangmayuan.com/a/c5aa4cccb97b49c59f06492e.html

#### 查看phpinfo

复制phpinfo的内容或查看网页源代码，https://xdebug.org/wizard 

得到分析结果

> ## Instructions
>
> 1. Download [xdebug-3.1.1.tgz](https://xdebug.org/files/xdebug-3.1.1.tgz)
>
> 2. Install the pre-requisites for compiling PHP extensions.
>    On your RedHat system, install them with: `yum groupinstall "Development tools" && yum install php-devel autoconf automake`
>
> 3. Unpack the downloaded file with `tar -xvzf xdebug-3.1.1.tgz`
>
> 4. Run: `cd xdebug-3.1.1`
>
> 5. Run: `phpize` (See the [FAQ](https://xdebug.org/docs/faq#phpize) if you don't have `phpize`).
>
>    As part of its output it should show:
>
>    ```
>    Configuring for:
>    ...
>    Zend Module Api No:      20190902
>    Zend Extension Api No:   320190902
>    ```
>
>    
>
>    If it does not, you are using the wrong `phpize`. Please follow [this FAQ entry](https://xdebug.org/docs/faq#custom-phpize) and skip the next step.
>
> 6. Run: `./configure`
>
> 7. Run: `make`
>
> 8. Run: `cp modules/xdebug.so /www/server/php/74/lib/php/extensions/no-debug-non-zts-20190902`
>
> 9. Update `/www/server/php/74/etc/php.ini` and add the line:
>    `zend_extension = xdebug`
>
> 10. Restart PHP-FPM



==步骤6出现错误：==

./configure   https://blog.csdn.net/u011093975/article/details/105098240/

