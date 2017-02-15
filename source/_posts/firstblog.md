---
title: windows系统下安装Composer
date: 2017-02-08 13:25:28
categories: 编程
tags: laravel
---
### Composer介绍
Composer 是 PHP 的一个依赖管理工具。它允许你申明项目所依赖的代码库，它会在你的项目中为你安装他们。Composer 不是一个包管理器。是的，它涉及 "packages" 和 "libraries"，但它在每个项目的基础上进行管理，在你项目的某个目录中（例如 vendor）进行安装。默认情况下它不会在全局安装任何东西。因此，这仅仅是一个依赖管理。

	Composer下载
	下载地址：getcom poser.org    （去掉空格）
	中文镜像：phpcom poser.com  （去掉空格）

![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/195921o2z605a55amqa0sa.png)
![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/200038h15g31yj3g81f1cw.png)

下载成功

![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/200051x4vpp55tnvs22ijw.png)

准备安装软件，双击软件就可以安装此软件 默认安装装就可以了，并会自动搜索PHP.exe的安装路径。

![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/200115pkkwe9i9irzkv3ca.png)

选择NEXT，稍等一会，下载相关组建
![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/200313dwreyedxfdasfjoz.png)

显示此页面表示安装完成！如果报错 就检查 PHP扩展的OpenSSL 有没有打开
![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/200343xgdse66jstrh8jwj.png)
验证是否成功。打开win+R -> cmd 输入 composer,显示如下界面 表示安装成功！![enter image description here](http://bbs.houdunwang.com/data/attachment/forum/201603/17/200400utwp9i3qaipka3w7.png)

### 启用镜像服务的方式有两种：
	系统全局配置： 即将配置信息添加到 Composer 的全局配置文件 config.json 中。
	单个项目配置： 将配置信息添加到某个项目的 composer.json 文件中。
例1：修改 composer 的全局配置文件（推荐方式）
打开命令行窗口（windows用户）或控制台（Linux、Mac 用户）并执行如下命令：

```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
上述命令将会在当前项目中的 composer.json文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：
```json
  "repositories": {
  "packagist": { 
   "type": "composer", 
   "url": "https://packagist.phpcomposer.com"
   }
}
```
以 laravel 项目的 composer.json 配置文件为例，执行上述命令后如下所示（注意最后几行）：
```
{
    "name": "laravel/laravel",
    "description": "The Laravel Framework.",
    "keywords": [
        "framework",
        "laravel"
    ],
    "license": "MIT",
    "type": "project",
    "require": {
        "php": ">=5.5.9",
        "laravel/framework": "5.2.*"
    },
    "require-dev": {
        "fzaninotto/faker": "~1.4",
        "mockery/mockery": "0.9.*",
        "phpunit/phpunit": "~4.0",
        "symfony/css-selector": "2.8.*|3.0.*",
        "symfony/dom-crawler": "2.8.*|3.0.*"
    },
    "autoload": {
        "classmap": [
            "database"
        ],
        "psr-4": {
            "App\\": "app/"
        }
    },
    "autoload-dev": {
        "classmap": [
            "tests/TestCase.php"
        ]
    },
    "scripts": {
        "post-root-package-install": [
            "php -r \"copy('.env.example', '.env');\""
        ],
        "post-create-project-cmd": [
            "php artisan key:generate"
        ],
        "post-install-cmd": [
            "php artisan clear-compiled",
            "php artisan optimize"
        ],
        "pre-update-cmd": [
            "php artisan clear-compiled"
        ],
        "post-update-cmd": [
            "php artisan optimize"
        ]
    },
    "config": {
        "preferred-install": "dist"
    },
    "repositories": {
        "packagist": {
            "type": "composer",
            "url": "https://packagist.phpcomposer.com"
        }
    }
}
```

```
//转换成中文镜像
composer config -g repo.packagist composer https://packagist.phpcomposer.com
//安装版本
composer create-project laravel/laravel your-project-name 5.0.*
```


