---
title: laravel笔记之laravel 安装和数据迁移填充
date: 2017-02-10 16:58:44
tags: laravel
categories: 编程
---

> 本教程示例代码见：https://github.com/johnlui/Learn-Laravel-5

在任何地方卡住，最快的办法就是去看示例代码。

### 开始学习

#### 1.安装

许多人被拦在了学习 Laravel 的第一步：安装。并不是因为安装有多复杂，而是因为【众所周知的原因】。在此我推荐一个 composer 全量中国镜像：http://pkg.phpcomposer.com/ 。启用 Composer 镜像服务作为本教程的第一项小作业请自行完成哦。

镜像配置完成后，在终端（Terminal 或 CMD）里切换到你想要放置该网站的目录下（如 C:\wwwroot、/Library/WebServer/Documents/、/var/www/html、/etc/nginx/html 等），运行命令：

```
composer create-project laravel/laravel learnlaravel5 5.2.31

```
然后，稍等片刻，当前目录下就会出现一个叫 learnlaravel5 的文件夹，安装完成啦~

#### 2.运行

为了尽可能地减缓学习曲线，推荐宝宝们使用 PHP 内置 web 服务器驱动我们的网站。运行以下命令：

```
cd learnlaravel5/public
php -S 0.0.0.0:1024

```

这时候访问 http://127.0.0.1:1024 就是这个样子的：

![laravel5](https://camo.githubusercontent.com/813c1698e040f3b29cd92f763e6ce1e2dd29c03f/68747470733a2f2f646e2d6c7677656e68616e2d636f6d2e71626f782e6d652f323031362d30352d30392d31343632323831303139323130342e6a7067)

我在本地 hosts 中绑定了 fuck.io 到 127.0.0.1，所以截图中我的域名是 fuck.io 而不是 127.0.0.1，其实他们是完全等价的。

这时候你可能要问了：为什么本宝宝的页面是一片空白？请使用开发者工具查看网络请求，只要是 200 状态就说明运行成功了，空白是因为这个页面引用了 Google Fonts，你懂的~

至于为什么选择 1024 端口？因为他是 *UNIX 系统动态端口的开始，是我们不需要 root 权限就可以随意监听的数值最小的端口。

另外，建议不熟悉 PHP 运行环境搭建的宝宝们不要轻易尝试使用 Apache 或 Nginx 驱动 Laravel，特别是在开启了 SELinux 的 Linux 系统上跑。关于 Laravel 在 Linux 上部署的大坑，本宝宝可能要单写一篇长文分享给宝宝们。

#### 3. 体验牛逼闪闪的 Auth 系统

Laravel 利用 PHP5.4 的新特性 trait 内置了非常完善好用的简单用户登录注册功能，适合一些不需要复杂用户权限管理的系统，例如公司内部用的简单管理系统。

激活这个功能非常容易，运行以下命令：

```
php artisan make:auth

```

访问 http://fuck.io:1024/login，如果你本地已经科学上网，那就能看到以下页面：
![login](https://camo.githubusercontent.com/1c989f7363771f11d916c5112066f3839b3a88aa/68747470733a2f2f646e2d6c7677656e68616e2d636f6d2e71626f782e6d652f323031362d30352d30392d31343632323936353739343437372e6a7067)

4. 连接数据库

接下来我们要连接数据库了，请自行准备好 MySQL 服务哦。

##### a. 修改配置

不出意外的话，learnlaravel5 目录下已经有了一个 .env 文件，如果没有，可以复制一份 .env.example 文件重命名成 .env，修改下面几行的值：

```
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel5
DB_USERNAME=root
DB_PASSWORD=root

```
推荐新建一个名为 laravel5 的数据库，并且使用 root 账户直接操作，降低学习数据库的成本。

数据库配置好之后，在登录界面填写任意邮箱和密码，点击 Login，你应该会得到以下画面：
![error](https://camo.githubusercontent.com/8abbf525301bd53ec10368258e38e69b0174bd42/68747470733a2f2f646e2d6c7677656e68616e2d636f6d2e71626f782e6d652f323031362d30352d30392d31343632333032303236323138312e6a7067)

它说 users 表不存在呀，接下来我们将见识 Laravel 另外一个实用特性。

##### b. 进行数据库迁移（migration）

运行命令：

```
php artisan migrate

```
我们得到了如下结果：

![result](https://camo.githubusercontent.com/2e3c07b19fca3a3be7eb994beccbf2e44d1e3671/68747470733a2f2f646e2d6c7677656e68616e2d636f6d2e71626f782e6d652f323031362d30352d30392d31343632333032323136393039342e6a7067)

数据库迁移成功！赶快打开 http://fuck.io:1024/home 注册一个用户试试吧~

下图是本宝宝注册了一个 username 为 1 用户：

![user1](http://7xlmi4.dl1.z0.glb.clouddn.com/2016-05-09-14623689497675.jpg)

##### c. migration 是啥？

打开 `learnlaravel5/database/migrations/2014_10_12_000000_create_users_table.php `文件，你肯定能一眼看出它的作用：用 PHP 描述数据库构造，并且使用命令行一次性部署所有数据库结构。

#### 5. 使用 Laravel 的“葵花宝典”：Eloquent

Eloquent 是 Laravel 的 ORM，是 Laravel 系统中最强大的地方，没有之一。当初 Laravel 作者在开发第一版的时候花了整整三分之一的时间才搞出来 Eloquent。当然，“欲练此功，必先自宫”，Eloquent 也是 Laravel 中最慢的地方，迄今无法解决。（路由、自动载入、配置分散、视图引发的性能问题都通过缓存几乎彻底解决了）

当然，我们还是要承袭第一版教程中对 Eloquent ORM 的描述：鹅妹子英！

 

##### a. Eloquent 是什么


Eloquent 是 Laravel 内置的 ORM 系统，我们的 Model 类将继承自 Eloquent 提供的 Model 类，然后，就天生具备了数十个异常强大的函数，从此想干啥事儿都是一行代码就搞定。

##### b. 怎么用？

我们使用 Artisan 工具新建 Model 类及其附属的 Migration 和 Seeder（数据填充）类。

运行以下命令：

```
php artisan make:model Article

```
去看看你的 app 目录，下面是不是多了一个 Article.php 文件？那就是 Artisan 帮我们生成的 Model 文件：

```
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    //
}

```
如此简洁有力的代码，隐藏了背后极高的难度和巨大的复杂度，让我们闭上眼睛，静静地感受 Laravel 的优雅吧 (～￣▽￣)～

### 6. 使用 Migration 和 Seeder

接下来我们生成对应 Article 这个 Model 的 Migration 和 Seeder。

##### a. 使用 artisan 生成 Migration

在 learnlaravel5 目录下运行命令：

```
php artisan make:migration create_article_table

```
成功之后打开 `learnlaravel5/database/migrations`，你会发现有一个名为 2*****_create_article_table 的文件被创建了。我们修改他的 up 函数为：


```
public function up()
{
    Schema::create('articles', function(Blueprint $table)
    {
        $table->increments('id');
        $table->string('title');
        $table->text('body')->nullable();
        $table->integer('user_id');
        $table->timestamps();
    });
}

```
这几行代码描述的是 Article 对应的数据库中那张表的结构。Laravel 默认 Model 对应的表名是这个英文单词的复数形式，在这里，就是 articles。接下来让我们把 PHP 代码变成真实的 MySQL 中的数据表，运行命令：

```
php artisan migrate

```
执行成功后，articles 表已经出现在数据库里了：

![article](https://camo.githubusercontent.com/a7675080d45339266815b2802e11d685f97c777a/68747470733a2f2f646e2d6c7677656e68616e2d636f6d2e71626f782e6d652f323031362d30352d30392d31343632373137313131323931372e6a7067)


##### b. 使用 artisan 生成 Seeder

Seeder 是我们接触到的一个新概念，字面意思为播种机。Seeder 解决的是我们在开发 web 应用的时候，需要手动向数据库中填入假数据的繁琐低效问题。

运行以下命令创建 Seeder 文件：

```
php artisan make:seeder ArticleSeeder

```
我们会发现 learnlaravel5/database/seeds 里多了一个文件 ArticleSeeder.php，修改此文件中的 run 函数为：

```
public function run()
{
    DB::table('articles')->delete();

    for ($i=0; $i < 10; $i++) {
        \App\Article::create([
            'title'   => 'Title '.$i,
            'body'    => 'Body '.$i,
            'user_id' => 1,
        ]);
    }
}

```

接下来我们把 ArticleSeeder 注册到系统内。修改 `learnlaravel5/database/seeds/DatabaseSeeder.php `中的 run 函数为：

```
public function run()
{
    $this->call(ArticleSeeder::class);
}

```
由于 database 目录没有像 app 目录那样被 composer 注册为 psr-4 自动加载，采用的是 psr-0 classmap 方式，所以我们还需要运行以下命令把` ArticleSeeder.php `加入自动加载系统，避免找不到类的错误：

```
composer dump-autoload

```
然后执行 seed：

```
php artisan db:seed

```
你应该得到如下结果：
![seed](https://camo.githubusercontent.com/629b71dcd42b94825e9075b5eaada4758b083dbd/68747470733a2f2f646e2d6c7677656e68616e2d636f6d2e71626f782e6d652f323031362d30352d30392d31343632373330353231313336362e6a7067)
这时候刷新一下数据库中的 articles 表，会发现已经被插入了 10 行假数据：

![data](http://7xlmi4.dl1.z0.glb.clouddn.com/2016-05-09-14627305849059.jpg)


#### 原文：https://github.com/johnlui/Learn-Laravel-5/issues/4
