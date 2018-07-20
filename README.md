# symfony 4.1

创建symfony项目
```
composer create-project symfony/skeleton my-project
```

为了方便开发，建议使用自带的WebServer进行测试，这样路由规则才会正确生效。
```
composer require symfony/web-server-bundle --dev
```
## 查看symfony版本信息
```
php bin/console

Symfony 4.1.1 (kernel: src, env: dev, debug: true)
```


## 查看当前路由表

```
php bin/console debug:router
```

## Service依赖注入

在./src/Service中创建MyService类。

在service.yaml添加自动加载路径。

在控制器方法或构造方法中以参数形式注入该类。

列出你能用来自动加载注入的类和接口
```
php bin/console debug:autowiring
```
显示应用里包含的服务
```
php bin/console debug:container
```

## 日志

```
composer require symfony/monolog-bundle
```
默认情况下，配置已自动生成，不需要任何后续配置，即可直接使用。

为了方便管理日志，避免单个日志太大，可以将monolog.yaml中type设为rotating_file，日志会按日期自动划分。

>http://symfony.com/doc/current/logging.html#how-to-rotate-your-log-files

## 单元测试

### 安装和启动PHPUnit

本机安装phpunit二进制文件。
```
$ wget http://phar.phpunit.cn/phpunit-6.2.phar
$ chmod +x phpunit-6.2.phar
$ sudo mv phpunit-6.2.phar /
usr/local/bin/phpunit
$ phpunit --version
PHPUnit x.y.z by Sebastian Bergmann and contributors.
```
执行以下语句将自动安装phpunit及其相关套件
```
php bin/phpunit
```
编写测试用例后，运行测试
```
phpunit
```
输出调试信息，例如当一个测试开始执行时输出其名称。
```
phpunit --debug
```
生成代码覆盖率报告
```
phpunit --coverage-html CoverageReportDir
```

### 在测试用例中使用容器中的服务

`self::bootKernel();`可以启动Symfony内核。

`self::$container->get()`可以从当前容器取出服务。

具体可取出的服务见一下命令输出的列表。
```
php bin/console debug:autowiring
```
示例代码
```php
<?php
namespace App\Tests\Service;

use Symfony\Bundle\FrameworkBundle\Test\KernelTestCase;

class SomeTest extends KernelTestCase
{
    public function setUp()
    {
        self::bootKernel();
    }

    public function testMyFunc()
    {
        $service = self::$container->get('App\Service\MyService');
        $this->assertTrue($service->func());
    }
}
```
