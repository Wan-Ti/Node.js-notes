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
