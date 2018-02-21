---
title: 《mongodb入门（一）：基础操作学习》 #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2017-10-16 22:30:50 #文章生成时间，一般不改，当然也可以任意修改
categories: mongodb #分类
tags: [前端] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: mongodb入门
---

## mongodb入门

<!-- more -->

1. mysql和mongodb关键概念区分

| 术语/概念        | sql    |  mongodb  |
| --------   | -----:   | :----: |
| 数据库        | database      |   database    |
| 数据库表/集合        | table      |   collection    |
| 数据记录行/文档        | row      |   document    |
| 数据字段/域       | column      |   field    |
| 索引        | index      |   index    |
| 表连接        | table joins      |   不支持    |
| 主键        | primary key      |   primary key;mongodb自动将_id字段设置为主键    |
	     
2. 基础命令：
	1. show dbs	（显示所有数据库）
	2. show tables 	（显示所有集合）
	2. db	（显示当前数据库）
	3. use database （切换数据库，也可创建数据库）
3. 删除数据库：
	1. db.dropDatabase()	删除数据库
	2. db.collection.drop()	删除集合
4. 插入文档
	1. db.collectionName.insert(document)	向数据库中插入数据。若不存在名为collectionName的集合，则会自动创建该集合并插入文档。
	2. db.collectionName.find()	查看已插入文档
	3. 3.2版本后的几种语法：
		1. db.collectionName.insertOne({"a":3})	向指定集合插入一条文档数据
		2. db.collectionName.insertMany({"b":3},{"c",4})	向指定集合中插入多条数据
5. 更新文档
	1. update方法： ```db.collection.update(< query>,< update>,{upsert: < boolean>multi: < boolean>,writeConcern: < document>})```参数说明：
        1. query:updata的查询对象
        2. update: update的更新对象和更新操作符号
        3. upsert:可选，如果不存在update的记录，是否插入objNew.true为插入，默认是false,不插入
        4. multi:可选，如果这个参数为true，就把按条件查出来多条记录全部更新，默认时false
        5. writeConcern:可选，抛出异常级别
    2. save方法：通过传入文档替换原有文档```db.collection.save(< document>,{writeConcern: < document>})```示例如下：```>db.col.save({"_id" : ObjectId("56064f89ade2f21f36b03136"),"title" : "MongoDB","description" : "MongoDB 是一个 Nosql 数据库","by" : "Runoob","url" : "http://www.runoob.com","tags" : ["mongodb","NoSQL"],"likes" : 110})```可见，必须要用"_id"字段来标记被替换的文档；
    3. 3.2版本后的几种语法：
        1. db.collectionName.updateOne()
        2. db.collectionName.updateMany()
6. 删除文档
    1. remove方法：```db.collection.remove(< query>,<justOne>)```2.6版本以后的语法格式如下：```db.collection.remove(< query>,{justOne: < boolean>,writeConcern: < document>})```参数说明：
        1. query：可选，删除的文档的条件
        2. justOne： 可选，设置为true或1,则只删除一个文档。
        3. writeConcern：可选，抛出异常的级别
    2.  remove() 方法已经过时了，现在官方推荐使用 deleteOne() 和 deleteMany() 方法。
        1. 如删除集合下全部文档：`db.collectionName.deleteMany({})`
        2. 删除 status 等于 A 的全部文档：`db.collectionName.deleteMany({"status":"A"})`
        3. 删除 status 等于 D 的一个文档：`db.collectionName.deleteOne({"status":"D"})`
7. 查询文档
    1. 使用find()方法：`db.collection.find(query, projection)`参数说明：
        1. query ：可选，使用查询操作符指定查询条件
        2. projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
        3. pretty() 方法以格式化的方式来显示所有文档。如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：`db.col.find().pretty()`
        4. 等于：直接键值对 `db.col.find({"by":"菜鸟教程"}).pretty()`
        5. 小于：lt `db.col.find({"likes":{$lt:50}}).pretty()`
        6. 小于或等于：lte `db.col.find({"likes":{$lte:50}}).pretty()`
        7. 大于：gt `db.col.find({"likes":{$gt:50}}).pretty()`
        8. 大于或等于：gte `db.col.find({"likes":{$gte:50}}).pretty()`
        9. 不等于：ne `	db.col.find({"likes":{$ne:50}}).pretty()`
        10. AND条件：多个键用逗号隔开 `db.col.find({key1:value1, key2:value2}).pretty()`
        11. OR条件：通过关键字$or `db.col.find({$or: [{key1: value1}, {key2:value2}]}).pretty()`
8. Limit与Skip方法
    1. Limit方法用于取指定数量的数据记录`db.COLLECTION_NAME.find().limit(NUMBER)`，例如：`db.col.find({},{"title":1,_id:0}).limit(2)，`结果为：`{ "title" : "PHP 教程" }{ "title" : "Java 教程" }`，其中第一个 {} 放 where 条件，为空表示返回集合中所有文档。第二个 {} 指定那些列显示和不显示 （0表示不显示 1表示显示)。
    2. skip方法，可以用来跳过指定数量的数据，同样接收一个数字参数作为跳过的记录条数。`db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)`
    3. skip和limit结合可以实现分页：`db.COLLECTION_NAME.find().skip(10).limit(10)`跳过前面的十条返回十条数据