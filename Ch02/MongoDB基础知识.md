## 2.0 三个概念
> * 文档</br>
   文档有一个特殊字段_id, 在文档所属集合中唯一; 文档是键值对的一个有序集。
> * 集合
> * 数据库

## 2.1 文档
> * 类型敏感
> * 大小写敏感
> * 不允许重复键

## 2.2 集合
> * 相同类型的文档放在一个集合里
> * 动态模式
> * 集合的命名规范
> * GridFS使用子集合来存储文件的元数据

## 2.3 数据库
> * 数据库名最大64字节,区分大小写,数据库名就是相应的数据文件名
> * 特殊数据库<br/>
> > * admin: 相当于root数据库，如果将一个用户添加到admin数据库，此用户将自动获得所有数据库的权限。运行一些特殊的服务端指令如列出所有数据库，关闭服务器
> > * local: 不可复制，存储所有本地集合备份
> > * config: 用于存储分片信息

## 2.4 启动
> * 启动指令: mongod, 默认端口 27017

## 2.5 MongoDB Shell
### 2.5.1 运行Shell
> * 启动Shell指令: mongo
```$xslt
> mongo
```
> * 命令行执行js

### 2.5.2 MongoDB客户端
> * db指令: 查看当前指向哪个数据库
```
> db
```
> * db.[集合名].insert(文档变量)
> * db.[集合名].findOne([查询文档])  检索出集合中满足查询条件的第一个文档
> * db.[集合名].find([查询文档])   检索出集合中满足查询条件的文档
```
db.blog.find({"title": "My Blog Post"});
```

> * db.[集合名].update([查询文档], 文档变量)  刷新集合中的某个文档
```
> post.comments = [] 
> db.blog.update({"title": "My Blog Post"}, post) 
> db.blog.find()
```

> * db.[集合名].remove([查询文档])  删除集合中满足查询条件的文档
```$xslt
> 
```

## 2.6 数据类型
### 2.6.1 基本的数据类型
> * 文档=JSON
> * 其他通用数据类型
> > * null : 用于表示空值或不存在的字段
```$xslt
{"x": null}
```
> > * 布尔型 : 
```$xslt
{"x": true}
```
> > * 数值
```$xslt
{"x": 3.14}
{"x": NumberInt("3")}
{"x": NumberLong("3")}
```
> > * 字符串
```$xslt
{"x": "foobar"}
```
> > * 日期
```$xslt
{"x": new Date()}
```
> > * 正则表达式
```$xslt
{"x": /foobar/i}
```
> > * 数组
```$xslt
{"x":["a", "b", "c"]}
```
> > * 内嵌文档
```$xslt
{"x": {"foo": "bar"}}
```
> > * 对象id
```
对象ID是一个12字节的ID, 是文档的唯一标识
{"x": ObjectId()}
```
> > * 二进制数据
```$xslt
不能直接在shell中使用,可以将非UTF-8字符保存到数据库中
```
> > * 代码
```$xslt
查询和文档中可以包含任意JS代码
{"x": function(){/* ... */}}
```

### 2.6.2 日期

### 2.6.3 数组
数组既能作为有序对象（列表,栈，队列),也能作为无序对象(数据集)来操作
```$xslt
{"things": ["pie", 3.14]}
```

## 2.7 使用MongoDB Shell
```$xslt
mongo 127.0.0.1:27017/myDB    连接指定位置的数据库
mongo --nodb      不连接数据库启动mongodb
```
### 2.7.1 Shell小贴士
```$xslt
help
```
### 2.7.2 使用shell执行脚本
```$xslt
mongo script1.js script2.js script3.js
mongo --quiet 127.0.0.1:27017/myDB script1.js script2.js script3.js
load("script1.js")
use foo
db.getSisterDB("foo")
show dbs
db.getMongo().getDBs()
show collections
db.getCollectionNames()

run("pwd")
run("ls","-l","/home/ciicit")
```

### 2.7.3 .mongorc.js
在用户主目录下创建.mongorc.js，可以在这个脚本中创建一些自己需要的全局变量，也可以重写内置的函数，移除一些比较危险的shell函数。
如下所示：
```$xslt
var no = function(){
    print("Not allowed")
}
db.dropDatabase = DB.prototype.dropDatabase = no;
DBCollection.prototype.drop = no;
DBCollection.prototype.dropIndex = no;
```
```$xslt
禁止.mongorc.js
mongo --norc
```

### 2.7.4 定制Shell提示
可以在.mongorc.js中定制提示.
```$xslt
    prompt = function() {
        return (new Date()) + "> ";
    };
    
    prompt = function(){
        if(typeof db == 'undefined'){
            return '(nodb)>'
        }  
        
        // 检查最后的数据库操作
        try{
            db.runCommand({getLastError: 1});
        }  
        catch(e){
            print(e);
        }
        
        return db + "> ";
    };
    
```

### 2.7.5 编辑复合变量
```$xslt
   EDITOR = "vim"
   var wap = db.blog.findOne({title: "My Blog Post"}) 
   edit wap
```

### 2.7.6 集合命名注意事项
```$xslt
db.version()
db["version"]()
```
