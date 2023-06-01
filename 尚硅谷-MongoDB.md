---
title: 尚硅谷-MongoDB
date: 2023-04-06 16:29:53
tags: MongoDB
---

[尚硅谷MongoDB入门基础教程（一天搞定mongodb）](https://www.bilibili.com/video/BV18s411E78K)

# 1 数据库

>数据库（Database）
* 数据库是按照数据结构来组织、存储和管理数据的仓库。
* 我们的程序都是在内存中运行的，一旦程序运行结束或者计算机断电，程序运行中的数据都会丢失。
* 所以我们就需要将一些程序运行的数据持久化到硬盘之中，以确保数据的安全性。而数据库就是数据持久化的最佳选择。
* 说白了，数据库就是存储数据的仓库。

## 1.1 数据库分类

数据库主要分成两种：

* 关系型数据库
  * MySQL、Oracle、DB2、SQL Server …… 
  * 关系数据库中全都是表 
* 非关系型数据库 
	* MongoDB、Redis …… 
	* 键值对数据库
	* 文档数据库MongoDB

# 2 MongoDB

## 2.1 简介

* MongoDB是为快速开发互联网Web应用而设计的数据库系统。
* MongoDB的设计目标是极简、灵活、作为Web应用栈的一部分。
* MongoDB的数据模型是面向文档的，所谓文档是一种类似于JSON的结构，简单理解MongoDB这个数据库中存的是各种各样的JSON。（BSON）

## 2.2 三个概念

* 数据库（database）数据库是一个仓库，在仓库中可以存放集合。 
* 集合（collection）  集合类似于数组，在集合中可以存放文档。 
* 文档（document）  文档数据库中的最小单位，我们存储和操作的 内容都是文档

## 2.3 基本操作

在MongoDB中，数据库和集合都不需要我们手动创建，当我们创建文档时，如果文档所在的集合或数据库不存在，她会自动创建数据库和集合！

- 基本指令

  - show dbs：显示当前所有数据库
  - show database：显示当前所有数据库
  - use：数据库名：进入到指定的数据库中（可以不存在）
  - db：表示我们当前所处的数据库
  - show collections：显示我们数据库中所有的集合

- CRUD操作

  - 向数据库中插入文档

    db.insert(doc)：向集合中插入文档

    向test数据库中的stus集合中插入一个新的学生对象

    db.stus.insert ( {name：“孙悟空”，age：18，gender：”男“} )

  - 查询集合中的所有文档

    db..find()

## 2.4 插入文档

> db.collection.insert()

- 向集合中插入一个或多个文档
- 当我们向集合中插入文档时，如果没有给文档指定_id属性，则数据库会自动给文档添加_id
  该属性用来作为文档的唯一标识
- _id可以自己指定，如果我们指定了，数据库就不会再添加了,如果自己指定_id必须也确保唯一性
  - db.collection.insertOne()：插入一个文档对象

- db.collection.insertMany()：插入多个文档对象

```js
db.stus.insert({name:"我很帅",age:28,gender:"男"})
db.stus.find()db.stus.insert([
    {name:"沙和尚",age:36,gender:"男"},
    {name:"白骨精",age:16,gender:"女"},
    {name:"蜘蛛精",age:14,gender:"女"}
])
```

- $push：用于向数组中添加一个新的元素

  ```javascript
  db.user.update({username:"tangseng"},{$push:{"hobby.movies":"Interstellar"}})
  1
  ```

- $addToSet：向数组中添加一个新元素（类似于向set集合中添加，如果数组中已经存在了该元素，则添加失败，因为不可重复）

  ```javascript
  db.user.update({username:"tangseng"},{$addToSet:{"hobby.movies":"Interstellar"}})
  ```

## 2.5 查询文档

> db.collection.find（）

- find（）用来查询集合中所有符合条件的文档
- find（）可以接收一个对象作为条件参数
  - { }：表示所有文档
  - { 属性：值 }：查询属性是指定值的文档
  - 返回值是一个数组
- db.collection.findOne()
  - 用来查询集合中符合条件的第一个文档
  - 返回的是一个文档
- db.stus.find({}).count()：查询所有结果的数量

```javascript
db.stus.find({name:"我很帅"})
db.stus.findOne({name:"我很帅"})
db.stus.find({name:"我很帅"})[0]
db.stus.find({}).length()
```

## 2.6 修改文档

- db.collection.update(查询条件，新对象)
  - update（）默认情况下会使用新对象来替换旧对象
  - update（）默认只会修改一个对象

如果需要修改指定的属性，而不是替换，需要使用 “修改操作符” 来完成修改

- $set：可以用来修改文档中的指定属性
- $unset：可以用来删除文档的指定属性

db.collection.updateMany()：同时修改多个符合条件的文档

db.collection.updateOne()：修改一个符合条件的文档

db.collection.replaceOne()：替换一个符合条件的文档

- MongoDB的文档的属性值也可以是一个文档，当一个文档的属性值是文档时，我们称这个文档为内嵌文档

- MongoDB支持直接通过内嵌文档的属性进行查询，如果要查询内嵌文档可以则可以通过==.的形式来匹配，且属性名必须使用引号==,双引号单引号都可以

  ```javascript
  db.user.find("hobby.movies"："hero")；
  ```

```javascript
db.stus.find()

db.stus.updateOne({name: "沙和尚"}, {age: 28})

db.stus.updateOne({"_id": ObjectId("5f86edc1048d21081bd45f3b")}, {$set: {gender: "男", address: "流沙河"}})

db.stus.updateOne({"_id": ObjectId("5f86edc1048d21081bd45f3b")}, {$unset: {address: "流沙河"}})

db.stus.updateMany({"name": "我很帅"}, {$set: {address: "高老庄"}})

db.stus.find()

db.stus.updateOne({"name": "我很帅"}, {$set: {address: "呵呵呵"}}, {multi: true})
```

## 2.7 删除文档

- db.collection.remove（）
  - 可以根据条件来删除文档，传递条件的方式和find（）一样
  - 能删除符合条件的所有文档，默认删除多个
  - 如果第二个参数传递一个true，则只会删除一个
  - 如果只传递一个{ }作为参数，则会删除集合中的所有文档
- db.collection.deleteOne（）
- db.collection.deleteMany（）
- db.collection.drop（）：删除集合（如果最后一个集合没了，数据库也没了。。。）

```javascript
db.stus.remove({age:28},true)
db.stus.remove({}) //性能差
db.stus.drop()
db.dropDatabase()
```

- 一般数据库中的数据都不会删除，所以删除的方法很少调用，一般会在数据中添加一个字段，来表示数据是否被删除

## 2.8 练习1

```js
//1.进入my_test数据库
use my_test

//2.向数据库的user集合中插入一个文档  
db.users.insert({
    username:"sunwukong"
});

//3.查询user集合中的文档
db.users.find();

//4.向数据库的user集合中插入一个文档   
db.users.insert({
    username:"zhubajie"
});
   
//5.查询数据库user集合中的文档
db.users.find();

//6.统计数据库user集合中的文档数量
db.users.find().count();

//7.查询数据库user集合中username为sunwukong的文档
db.users.find({username:"sunwukong"});

//8.向数据库user集合中的username为sunwukong的文档，添加一个address属性，属性值为huaguoshan
db.users.update({username:"sunwukong"},{$set:{address:"huaguoshan"}});


//9.使用{username:"tangseng"} 替换 username 为 zhubajie的文档
db.users.replaceOne({username:"zhubajie"},{username:"tangseng"});    
    
//10.删除username为sunwukong的文档的address属性
db.users.update({username:"sunwukong"},{$unset:{address:1}});


//11.向username为sunwukong的文档中，添加一个hobby:{cities:["beijing","shanghai","shenzhen"] , movies:["sanguo","hero"]}
//MongoDB的文档的属性值也可以是一个文档，当一个文档的属性值是一个文档时，我们称这个文档叫做 内嵌文档
db.users.update({username:"sunwukong"},{$set:{hobby:{cities:["beijing","shanghai","shenzhen"] , movies:["sanguo","hero"]}}});
db.users.find();

//12.向username为tangseng的文档中，添加一个hobby:{movies:["A Chinese Odyssey","King of comedy"]}
db.users.update({username:"tangseng"},{$set:{hobby:{movies:["A Chinese Odyssey","King of comedy"]}}})

//13.查询喜欢电影hero的文档
//MongoDB支持直接通过内嵌文档的属性进行查询，如果要查询内嵌文档则可以通过.的形式来匹配
//如果要通过内嵌文档来对文档进行查询，此时属性名必须使用引号 
db.users.find({'hobby.movies':"hero"});

//14.向tangseng中添加一个新的电影Interstellar
//$push 用于向数组中添加一个新的元素
//$addToSet 向数组中添加一个新元素 ， 如果数组中已经存在了该元素，则不会添加
db.users.update({username:"tangseng"},{$push:{"hobby.movies":"Interstellar"}});
db.users.update({username:"tangseng"},{$addToSet:{"hobby.movies":"Interstellar"}});
db.users.find();

//15.删除喜欢beijing的用户
db.users.remove({"hobby.cities":"beijing"});

//16.删除user集合
db.users.remove({});
db.users.drop();

show dbs;

//17.向numbers中插入20000条数据 7.2s
for(var i=1 ; i<=20000 ; i++){
    db.numbers.insert({num:i});
}

db.numbers.find()

db.numbers.remove({});


//0.4s
var arr = [];

for(var i=1 ; i<=20000 ; i++){
    arr.push({num:i});
}

db.numbers.insert(arr);
```



## 2.9 练习2

```js
//18.查询numbers中num为500的文档
db.numbers.find({num:500})

//19.查询numbers中num大于5000的文档
db.numbers.find({num:{$gt:500}});
db.numbers.find({num:{$eq:500}});

//20.查询numbers中num小于30的文档
db.numbers.find({num:{$lt:30}});

//21.查询numbers中num大于40小于50的文档
db.numbers.find({num:{$gt:40 , $lt:50}});

//22.查询numbers中num大于19996的文档
db.numbers.find({num:{$gt:19996}});

//23.查看numbers集合中的前10条数据
//limit()设置显示数据的上限
db.numbers.find().limit(10);
//在开发时，我们绝对不会执行不带条件的查询
db.numbers.find();

//24.查看numbers集合中的第11条到20条数据
/*
    分页 每页显示10条
        1-10     0
        11-20    10
        21-30    20
        。。。
        
        skip((页码-1) * 每页显示的条数).limit(每页显示的条数);
        
    skip()用于跳过指定数量的数据    
    
    MongoDB会自动调整skip和limit的位置
*/
db.numbers.find().skip(10).limit(10);

//25.查看numbers集合中的第21条到30条数据
db.numbers.find().skip(20).limit(10);
/*MongoDB会自动调整skip和limit的位置 所以等效于上一条语句*/
db.numbers.find().limit(10).skip(10);
```

## 2.10 文档间的关系

- 一对一（one to one）

  - 夫妻

  - 在MongoDB中，可以通过内嵌文档的形式来体现出一对一的关系

    ```javascript
    db.WifeAndHusband.insert([
        {
            wife:"黄蓉",
            husband:{
                name:"郭靖"
            }
        },
        
        {
            wife:"潘金莲",
            husband:{
                name:"武大郎"
            }
        }
        
    ])
    ```
  
- 一对多（one to many）/ 多对一（many to one）

  - 一对多：父母和孩子、用户和订单、文章和评论，也可以通过内嵌文档的方式来映射一对多的关系（将1的那个属性设置为多的里面的字段）

    ```javascript
    db.order.insert({
    	list:["watermelor"],
        user_id:ObjectId("5f87b1deda684b252c2fc7a5")
    })
    
    var user_id = db.users.findOne({username:"swk"})._id
    //查询孙悟空的订单
    db.order.find({user_id:user_id})
    ```
  
- 多对多（many to many）：分类和商品，通过内嵌文档的方式

  ```js
  db.teacher.insert([
  	{name:"洪七公"},
      {name:"黄药师"},
      {name:"龟仙人"}
  ])
  
  db.stus.insert([
      {
          name:"郭靖",
          tech_ids:[
              ObjectId("5f87b4b6da684b252c2fc7a8"),
              ObjectId("5f87b4b6da684b252c2fc7a9")
          ]
      },   
      {
          name:"孙悟空",
          tech_ids:[
              ObjectId("5f87b4b6da684b252c2fc7a8"),
              ObjectId("5f87b4b6da684b252c2fc7a9"),
              ObjectId("5f87b4b6da684b252c2fc7aa")
          ]
      }
  ])
  ```

# 12. 练习

```javascript
//查询工资小于1000或者大于2000的员工
db.emp.find( $or:[ {sal:{$lt:1000}},{sal:{$gt:2500}} ])

//为所有工资小于1000的增加400
db.emp.find({sal:{$lte:1000}},	{$inc:{$sal:400}})
```

# 13. sort和投影

- find（）查询文档时，默认情况是按照_id的值进行升序排列

- sort（）可以用来指定文档的排序的规则，需要传递一个属性来指定排序规则，1表示升序，-1表示降序

  ```javascript
  db.users.find({}).sort({sale:1})
  db.users.find({}).sort({sale:1,qq:1}) //先指定sale的升序 再qq的降序
  12
  ```

- limit、skip、sort可以任意顺序的调用

- 查询时，我们可以在第二个参数的位置来设置查询结果的投影

  ```javascript
  db.users.find({},{sale:1})
  ```
