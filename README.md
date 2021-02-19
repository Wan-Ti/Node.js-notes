# Node.js-notes

# Noede.js

Node.js是一个将多种技术组合起来，让JavaScript也能调用系统接口、开发后端应用。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/417ea63956614710a0294573028ae98b~tplv-k3u1fbpfcp-watermark.image)

* 用libuv进行异步I/O操作；</br>
* 用event loop管理事件处理顺序；</br>
* 用C/C++库高效处理DNS/HTTP...</br>
* 用bindings让JS能和C/C++沟通;</br>
* 用V8运行JS；</br>
* 使用Node.js标准库简化JS代码；</br>

## 文件模块fs

完成一个基于文件的todo工具。

**需求：**

* 功能：

可以列出所有的todo;</br>
可以新增/编辑/删除todo;</br>
可以标记todo为已完成/未完成；</br>

* 命令：

t;</br>
t add 任务名；</br>
t clear;</br>

* 发布到npm

修改package.json
```
"bin":{
   "t":"./cli.js"
 },
 "files":[
   "*.js"
 ],
```

## HTTP模块

**创建项目：**

步骤：

* yarn init -y;</br>
* 新建index.ts;</br>
* 使用命令行或者WebStorm启动；</br>
* yarn add ==dev @types/node安装node声明文件；</br>
* 引入http模块；</br>
* 用http创建server;</br>
* 监听server的request事件；</br>
* server.listen(8888)开始监听8888端口；</br>
* 使用curl -v http://localhost:8888发请求</br>

**使用Node.js获取请求内容：**

get请求：</br>
* request.method获取请求动词;</br>
* request.url获取请求路径(含查询参数);</br>
* request.header获取请求头;</br>
* get请求一般没有消息体/请求体;</br>

post请求：</br>
* curl -v -d "name=wanti"  http://localhost:8888;</br>
* request.on('data',fn)获取消息体;</br>
* request.on('end',fn)拼接消息体;</br>

## Node.js操作数据库

### 使用Docker安装数据库

**Docker安装MySQL:**

* 进入Docker上面的MySQK的主页，选择版本;</br>
* 使用docker run命令启动容器，name是容器的名字，MySQL_ROOT_PASSWORD是密码;</br>
* tag是版本号，我们选用5.7.27;</br>
* 再加一个端口映射-p 3306:3306;</br>
* 最终命令：;</br>

 ` docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:5.7.27`

**有用的Docker命令：**

* 运行`docker ps` 查看容器运行状态;</br>
* 运行`docker kill mysql1`关掉容器;</br>
* 运行`docker container run mysql1`开启刚关掉的容器;</br>
* 运行`docker rm mysql1`删掉容器，必要时可加-f 选项;</br>
* 运行`docker run `启动新容器;</br>

注意：使用Docker运行的容器，默认不会持久化。也就是说如果容器被删掉了，那么数据也没了。

**进入容器运行mysql命令：**

Docker exec 命令：
* docker exec -it mysql1 bash;</br>
* 这句命令会进入容器，容器中有一个Linux系统;</br>
* 然后就可以在这个系统中运行mysql;</br>



**在mysql命令行里执行SQL语句：**

mysql命令：
* mysql -u -root -p 回车，然后输入密码123456;</br>
* 命令show databases;这样可以查看数据库里列表;</br>
* 切记一定要写分号再回车，否则只能Crtl+C重来;</br>
* 命令 use xxx;可使用xxx数据库;</br>
* 命令use sys;使用默认的sys数据库;</br>
* 命令 show tables;查看所有表;</br>
* 命令select * fromCHARCTER_SETS;查看表内容


### Node.js连接数据库

yarn add mysql

#### MySQL数据类型

**五大类**

* 数字类型；
* 字符串类型；
* 时间和日期类型；
* JSON类型；
* 其他特殊类型；

**数字类型：**

bit;</br>
tinyint;</br>
bool,boolean;</br>
smallint;</br>
mediumint;</br>
int;</br>
bigint;</br>
decimal;</br>
float;</br>
double;</br>

**字符串类型：**

char(100);</br>
varchar(100);</br>
binary(1024);</br>
varbinary(1024);</br>
blob;</br>
text;</br>
enum('v1','v2');</br>
set('v1','v2');</br>

**时间和日期类型：**

date;</br>
time;</br>
datetime;</br>
timestamp;</br>
year;</br>

## 数据库

### 关系型数据库的范式

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dc1ac864ba2f4648926728dff0a7850c~tplv-k3u1fbpfcp-watermark.image)

#### 第一范式1NF 

**定义：**

字段不可再分。



#### 第二范式2NF:

**定义：**

在1NF的基础上，要有键(键可由多个字段组合)；</br>
所有字段分别完全依赖于键；</br>
如果键子多个字段组合，则不允许部分依赖于该键；

**依赖关系：**

给出键，就能唯一确定字段的值；</br>
例如给出学号，就能唯一确定姓名，反之则不行；</br>
则称姓名依赖于学号

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/91d51acc9bee41658f9f4c2375a0f558~tplv-k3u1fbpfcp-watermark.image)

#### 第三范式3NF：

**定义：**

一个表里不能有两层依赖；</br>
给出学号，就能确定系名：系名依赖于学号；</br>
给出系名，就能确定系主任：系主任依赖于系名；</br>

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/babcc70414fc438d959e4154c57851ad~tplv-k3u1fbpfcp-watermark.image)

### 总结：

**第一范式** 

属性不可分割

**第二范式** 

字段完全依赖于键

**第三范式**

字段没有间接依赖于键

**BC范式**

键中的属性也不存在间接依赖

## 数据库设计经验 

**高内聚：**

把相关的字段放到一起，不相关的分开建表；
如果两个字段能够单独建表，那就单独建表；

**低耦合：**

如果两个表之间有弱关系；</br>
一对一可放在一个表，也可两个表加外键；</br>
一对多一般用在外键；</br>
多对多一般建中间表；

### MySQL存储引擎

命令 SHOW ENGLINES；

**常见的**

* InnDB-默认，目前版本是新版InnDB;</br>
* MyISAM-拥有较高的;</br>
* Memory-内存中，快速访问数据;</br>
* Archive-只支持insert和select;</br>

**InnoDB**

InnoDB是事务性数据库的首选，支持事务、遵循ACID，支持行锁和外键。


###  Stream

**Stream-流：**

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d94f5973cded4493ac597b82bb8d581d~tplv-k3u1fbpfcp-watermark.image)

stream是水流，但默认的是没有水；</br>
stream.write可以让水流中有水(数据)；</br>
每次写的小数据叫做chunk(块)；</br>
产生数据的一段叫做source(源头)；</br>
得到数据的一段叫做sink(水池)</br>

### 管道：

两个流可以用一个管道相连，stream的末尾连接上stream2的开端。只要stream1有数据，就会流到stream2中。

常见代码：</br>
`stream.pipe(stream2)`

链式操作：</br>

`a.pipe(b).pipe(c)等价于a.pipe(b),b.pipe(c)`

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c6c38fb418f44cbe9c3cc3a43d1d94c8~tplv-k3u1fbpfcp-watermark.image)


**管道可以通过时间实现：**

```
// stream1 一有数据就塞给 stream2
stream1.on('data',(chunk)=>{
   stream2.write(chunk)
})
//stream1 停了，就停掉stream2
stream1.on('end',()=>{
   stream2.end()
})
```

**Stream对象的原型链：**

`s = fs.createReadStream(path)`

那么它的对象层级为：</br>
自身属性(由`fs.ReadStream`构造)；</br>
原型：`stream.Readable.prototype`;</br>
二级原型：`stream.Stream.prototype ` ；</br>
三级原型：` events.EventEmitter.prototype `；</br>
四级原型：` Object.prototype ` ；</br>

Stream对象都继承了EventEmitter;</br>

**支持的事件和方法**

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/738c1cca504e481a9026b613b87d9094~tplv-k3u1fbpfcp-watermark.image) 

### Readable Stream

**静止态paused和流动态flowing**

* 默认处于paused态;</br>
* 添加data事件监听，它就变为flowing态;</br>
* 删掉data事件监听，它就变为paused态;</br>
* pause()可以将它变为paused;</br>
* resume()可以将它变为flowing;</br>

### Writable Stream

**drain流干了事件**

表示可以加点水（加入事件），我们调用stream.wrote(chunk)的时候，可能会得到false，意思是书写太快，数据被积压了。这个时候就不能继续write,需要监听drain.等drain事件触发了，才能继续write.

**finish事件**

调用stream.end()之后，而且缓冲区数据都已经传给底层系统之后，触发finish事件。

### 创建一个Wirtable Stream

```
const { writeable } = require("stream");

const outStream = new Writeable({
   write( chunk,encoding,callback) {
     console.log(chunk.toString());
     callback();
   }
});

process.stdin.pipe(outStream);
//保存文件为 writable.js,然后用node运行
//不管你输入什么，都会得到相同的结果
```

### 创建一个Readable Stream

```
const { Writeable } = require("stream");

cosnt outStream = new Writeable({
  write(chunk,encoding,callback) {
    console.log(chunk.toString());
    callback();
  }
});

process.stdin.pipe(outStream);

//保存文件为writable.js,然后用node运行；
//不管你输入什么，都会得到相同的结果
```

### 创建一个Duplex Stream

```
const { Readable } =require("stream");

const inStream = new Readable();

inStream.push("ABCDEFGHIJKLM");
inStream.push("NOPQRSTUVWXYZ");

inStream.push(null); // No more data

inStream.pipe(process.stdout);

// 保存文件为readable.js 然后用node 运行
// 先把所有数据都push进去，然后pipe
```
```
const { Readable } = require("stream");

const inStream = new Rendable({
  read(size) {
  this.push(String.fromCharCode(this.currentCharCode++));
    if (this.currentCharCode > 90){
      this.push(null);
     }
   }
 });
 
 inStream.currentCharCode = 65
 
 inStream.pipe(process.stdout)
 //保存文件为readable2.js 然后用node运行
 //这次的数据是按需供给的，对方调用read才会给一次数据
```

### 创建一个Duplex Stream

```
const { Duplex } = require("stream");

const inoutStream = new Duplex({
  write(chunk,encoding,callback){
     console.log(chunk.toString());
     callback();
  },
  
  read(size) {
   this.push(String.fromCharCode(this.currentCharCode++));
     if (this.currentCharCode > 90) {
        this.push(null);
     }
  }
});

inoutStream.currentCharCode = 65;

process.stdin.pipe(inooutStream).pipe(process.stdout);


```


### 创建一个Transform Stream

```
const {Transform } = require("stream");

const upperCaseTr = new Transform({
   transform(chunk,encoding,callback) {
      this.push(chunk.toString().toUpperCase());
      callback();
     }
});

process.stdin.pipe(upperCaseTr).pipe(process.stdout);


```

### 内置的Transform Stream

```

const fs = require("fs");
const zlib = require("zlib");
const fike = process.argv[2];

fs.createReadStream(file)
.pipe(zlib.createGzip())
.pipe(fs.createWriteStream(file + ".gz"));
```

```
const fs = require("fs");
const zlib = require("zlib");
const fike = process.argv[2];

fs.createReadStream(file)
   .pipe(zlib.createGzip())
   .on("data",() => process.stdout.write("."))
   .pipe(fs.createWriteStream(file + ".zz"))
   .on("finish",() => console.log("Done"));
```

### Node.js中的Stream

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4ad2f714c63643c0aa36b9afc98d5e9b~tplv-k3u1fbpfcp-watermark.image)

<a href="https://juejin.cn/post/6844903635617316877">面试高级前端工程师必问之流-stream </a>



