---
title: thinkphp框架RBAC权限控制和auth权限控制类
date: 2017-03-01 19:41:07
tags: ThinkPHP
categories: 编程
---

## thinkphp框架RBAC权限控制和auth权限控制类

去某家公司面试，技术面试官提出一个关于RBAC的问题，想了很久没能答上来，可想而知面试的结果会怎样，但问题仍然存在，什么是RBAC权限控制？

回家后突然想起来，RBAC的英文Role Based Access Control 的缩写，中文是基于角色的权限管理，相比这个概念并不陌生，项目中权限管理是必不可少的一环，而公司框架本身早就封装好了该功能，实际开发中很少会二次开发，因此有些生疏。回去查了资料，得知TP框架中的RBAC类是3.1支持的写法，到了更优秀的版本3.2后，TP官方去除了此方法，封装了一个更加简洁的类（auth类）。看到这就不再陌生了，因为我一开始接触的TP版本就是3.2，很早以前使用过auth类，并不陌生，真是有点好笑，只知诸葛亮，不知诸葛孔明。

RBAC权限管理种，用户通过角色与权限进行关联[其架构灵感来源于操作系统的GBAC（GROUP-Based Access Control）的权限管理控制]。简单的来说，一个用户可以拥有若干角色，每一个角色拥有若干权限。这样，就构造成“用户-角色-权限”的授权模型。在这种模型中，用户与角色之间，角色与权限之间，一般者是多对多的关系。

![](http://www.lyblog.net/usr/uploads/2014/09/image9.png)

在许多的实际应用中，系统不只是需要用户完成简单的注册，还需要对不同级别的用户对不同资源的访问具有不同的操作权限。且在企业开发中，权限管理系统也成了重复开发效率最高的一个模块之一。而在多套系统中，对应的权限管理只能满足自身系统的管理需要，无论是在数据库设计、权限访问和权限管理机制方式上都可能不同，这种不致性也就存在如下的憋端：

维护多套系统，重复造轮子，时间没用在刀刃上
用户管理、组织机制等数据重复维护，数据的完整性、一致性很难得到保障
权限系统设计不同，概念理解不同，及相应技术差异，系统之间集成存在问题，单点登录难度大，也复杂的企业系统带来困难
RBAC是基于不断实践之后，提出的一个比较成熟的访问控制方案。实践表明，采用基于RBAC模型的权限管理系统具有以下优势：由于角色、权限之间的变化比角色、用户关系之间的变化相对要慢得多，减小了授权管理的复杂性，降低管理开销；而且能够灵活地支持应用系统的安全策略，并对应用系统的变化有很大的伸缩性；在操作上，权限分配直观、容易理解，便于使用；分级权限适合分层的用户级形式；重用性强。

**更多关于thikphpRBA详细的配置使用过程，均可参考如下地址，作者写的十分详细** [跳转到此](https://www.lyblog.net/detail/552.html)

TP框架3.1版本种RBAC类虽然功能完善，但是许多人觉得配置起来十分麻烦，于是到了3.2的时代，auth类就应运而生了。

TP内置的auth权限类位于：`/ThinkPHP/Library/Think/Auth.class.php`,执行里面的sql生成3张表`auth_rule`、`auth_group`、`auth_group_access`；
然后自己再建一张users表；当然起其他的名字也是可以的；不过是需要在配置项中说明；

先对各表的作用简单介绍；

```
users：用户表；
auth_group：用户组表；比如说超级管理员组、普通管理员组、编辑等等；同时记录每个管理组有哪些权限；
auth_group_access：用户、群组关联表；比如说用户1属于超级管理员、用户2属于普通管理员和编辑；
auth_rule：权限表；具体的每条权限是什么；

```
如果还没看过权限管理；那建议先看源代码；透彻学习一样东西；最好的方法就是研究源代码；
这里重点不是要讲auth的原理；而是要给一个auth的demo；

git源代码：https://github.com/baijunyao/thinkphp-bjyadmin

1：先下载项目并安装；
完成后分别点超级管理员登录和文章管理员登录；
你会发现他们的权限是不同的；看到的后台菜单是不一样的； 

![](http://baijunyao.com/Upload/image/ueditor/20160514/1463186682405819.jpg)

2：菜单管理

![](http://baijunyao.com/Upload/image/ueditor/20160504/1462291919136306.jpg)

为了控制每种管理员都能看到那些菜单；所以要有菜单的管理；操作的是demo中的admin_nav表

3：权限管理

![](http://baijunyao.com/Upload/image/ueditor/20160504/1462291929640930.jpg)

具体的每项权限的名称和内容；我这里一般都是和菜单对应的；但是会比菜单管理多出一些；对比两张图即可看出来；多出来的一般都是些对菜单的增删改；
操作的是demo中的auth_rule表；

4：用户组管理

！[](http://baijunyao.com/Upload/image/ueditor/20160504/1462291949928045.jpg)

这里就是增加管理组；并为每个管理组分配权限了；选中的就表示有权限看到或者操作了；

![](http://baijunyao.com/Upload/image/ueditor/20160504/1462291962384468.jpg)

5：管理员列表
![](http://baijunyao.com/Upload/image/ueditor/20160514/1463186286405414.jpg)
把所有的管理员都列出来；可以添加管理员或者修改管理员的管理组；

![])(http://baijunyao.com/Upload/image/ueditor/20160514/1463186391769769.jpg)

当构建好这样一个结构后；权限管理简单其实只需要AdminBaseController.class.php中如下一段代码就完成了；

/Application/Common/Controller/AdminBaseController.class.php

```
$auth=new \Think\Auth();
$rule_name=MODULE_NAME.'/'.CONTROLLER_NAME.'/'.ACTION_NAME;
$result=$auth->check($rule_name,$_SESSION['user']['id']);
if(!$result){
    $this->error('您没有权限访问');
}
```
这也是在 thinkphp的目录结构设计经验总结中讲述 /Application/Common/Controller中建各种BaseController的原因；

> 是不是感觉很简洁，几行代码就解决问题了。~~

注：关于RBAC详细的视频教程，在以下链接可以找到,[雪狐网基于ThinkPHP3.1.3版本框的RBAC权限管理视频教程](http://www.thinkphp.cn/topic/3425.html)