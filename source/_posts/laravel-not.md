---
title: laravel数据库以及CURD
date: 2017-02-08 14:23:52
categories: 编程 
tags: laravel
---
### db facade(原始查找)
>使用facade就是使用原始的sql语句来查询修改数据库
```
DB::select('sql 语句')
DB::insert('insert into student (name,age) values(?,?),['jeremy',22]')
DB::update('sql 语句')
DB::delete('sql 语句'
```
### 查询构造器
>使用laravel操作数据库的必备技能

#### 查询构造器简介及新增数据
- larevel查询构造器(query bulider)提供方便/流畅的接口,用来建立及执行数据库查找语句
- 使用PDO 参数绑定,以保护应用程序免于sql注入因此传入的参数不需额外转意特殊字符


```
//插入新数组返回bool
DB::table('tablename')->insert(['age'=>18,'name'=>'jeremy'])
//插入新数据得到自增id
DB::table('tablename')->insertGetId(['age'=>18,'name'=>'jeremy'])
//一次插入多个数据数组
 DB::table('tablename')->insert([
		 'age'=>18,'name'=>'jeremy1'],
		 'age'=>19,'name'=>'jeremy2'],
		 'age'=>17,'name'=>'jeremy3'],
		 'age'=>16,'name'=>'jeremy4'],
		 )
```
#### 更新数据
```
DB::table('tablename')
	->where('id',12)
	->update(['age'=>30])
```
#### 删除数据
```
DB::table('tablename')
	->where('id','>=',12)
	->delete()
```
#### 查询数据
```
//获得所有数据
DB::table('tablename')->get();
//返回第一条记录
DB::table('tablename')
->orderBy('id','desc')
->first();
//返回结果结果集中某个字段pluck
DB::table('tablename')
->orderBy('id','desc')
->pluck('name');
//返回结果结果集中制定的字段并使用某个键为下标
DB::table('tablename')
->orderBy('id','desc')
->lists('name','id');

//select查询
DB::table('tablename')
->select('id','name','age')
->get();
//chunk 每次查询n个数据并回调
DB::table('tablename')->chunk(1000,function($res){	
	if($res){
	return false;
	}
});

```




### Eloquent ORM(Object Relational Model )

>操作laravel数据库最常用的方式

 - Laravel 所自带的Eloquent ORM是一哥优美 简洁的ActiveRecord实现,用来实现数据库操作
 - 每个数据表都有一个与之对应的'模型(Model)' 用于和数据表交互

```php
 class Student extends Model{
	 //模型与数据库表关联
	protected $table='student';
	//指定id
	protected $primaryKey='id';
	
}
```
查询方法

```php
//返回所有集合对象
$res = student::all();
//返回一个集合对象
$res = student::find(1001);
//findOrFail查询不到抛出异常
$res = student::findOrFail(1001);

```
`查询构造器的使用`

```
//查询所有
Student::get();
//查询一条
Student::where('id','>','1001')
->orderBy('id')
->first();
//查询n条
Student::where('id','>','1001')
->orderBy('id')
->chunk(2000,function($res){
	var_dump($res)
});
```
`增加数据`
```
//此处不允许批量赋值
Student::create(
	['name'=>'jeremy'],
	['age'=>12],
	['sex'=>1],
)
//在模型出加上允许批量赋值的字段名字
protected $fillable=['name','age']
```
```
//没有该数据则新增此数据
Student::firstOrCreate()

//没有该数据调用save()后新增此数据
Student::firstOrNew()->save();
```
`修改数据`
```
//第一种
$student=Student::find(1020);
$student->name='jeremy';
$student->save();
//第二种
Student::where('id',12)
->update(['name'=>'jeremy']);
```

`删除数据`
```
//通过模型删除
$student= Student::find(1021);
$student->delete();
//通过主键删除
Student::destroy(1020,1021);

//通过条件删除
Student::where('id','>',1004)-delete();
```

`pluck`

获取一列的值
若你想要获取一个包含单个字段值的数组，你可以使用 pluck 方法。在这个例子中，我们将取出 roles 数据表 title 字段的
数组：
```

$titles = DB::table('roles')->pluck('title');
foreach ($titles as $title) {
echo $title;
}
```
你也可以在返回的数组中指定自定义的键值字段：
```

$roles = DB::table('roles')->pluck('title', 'name');
foreach ($roles as $name => $title) {
echo $title;
}
```
