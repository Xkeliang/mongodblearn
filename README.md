# mongodblearn
mongodb学习
###MongoDB
    ## —nosql与MongoDB
        #nosql运动背景
            关系型数据库功能复杂、性能开销大、价格昂贵
            避免不需要的复杂性
            更高的吞吐量、高并发
            在商业硬件上的水平扩展能力
            实现了大表的自动分割（sharding）功能、更好的支持分布式处理
            在性能和可靠性之间的折中
            云计算的需求（从中心到分布式）
        #主流nosql
            1、memcashed
                一个存储键/值对的hashmap
                高性能的分布式内存缓存服务器
                客户端通过memcached协议与守护进程通信
                缺乏认证以及安全机制
                协议简单
                基于libevent的事件处理
                内置内存存储方式
                不互相通信的分布式
            2、redis
                一个key/value 存储系统，和memcashed类型
                运行异常快
                数据都是缓存在内存中，有硬盘存储支持的内存数据库
                master-slave 复制
                value数据类型丰富，string、list（链表）、set（集合）和zset（有序集合）
                支持push/pop，允许用户实现消息机制
            3、neo4j
                基于关系的图形数据库
                协议：HTTP/REST
                可独立使用或嵌入到java应用程序
                图形的节点和边都可以带有元数据
                使用多种算法支持路径搜索
                使用键值和关系进行索引
                为读操作进行优化
                支持事务（用java api）
                企业版支持在线备份，高级监控及可靠性
            4、Cassandra
                所用语言：java
                混合型的非关系的数据库
                基于列的数据模型、
                写操作比读操作快
                分布式，基于column的结构化，高伸展性
                应用客户：facebook
            5、HBase
                Hadoop Database      开源
                分布式的、面向列的开源数据库，在Hadoop之上提供了类似于Bigtable的能力、
                采用分布式架构map/reduce
                协议：http/rest ,支持thrift
                支持数十亿行X上百万列
                最佳应用场景：适用于偏好BigTable，并且需要对大数据进行随机、实时访问的场合
                ...
            6、MongoDB
                社区活跃，文档丰富
                所用语言：C++
                特点：保留了SQL一些友好的特性（查询、搜索）
                协议: Custom,binary(BSON)
                Master/slave 复制（支持自动错误恢复，使用sets复制）
                内建分片机制
                支持JavaScript表达式查询
                可在服务器端执行任意的JavaScript函数
                在数据存储时采用内存到文件映射
                对性能的关注超过对功能的要求
                64位系统数据库大小无限制
                空数据库大概占用192Mb
                采用GridFS存储大数据或元数据

    ##MongoDB 
        #一、MongoDB安装
            下载：MongoDB官网：https://www.mongodb.org 
            解压、配置、create the directory  ./data/db
                配置文件mongo.config
                logpath= D:\learn\MongoDB\data\log\mongodb.log
                logappend=false
                dbpath=D:\learn\MongoDB\data\db
                rest=true
            启动mongod -f arg
            关闭mongod --remove
            mongodb的web控制台使用  http://192.168.23.1:28017

            图形化界面：JMongoBrowser
        
        #二、MongoDB  shell
            shell使用及常用命令
                连接：
                 mongo --port 27017
                 mongo  默认端口27017
                 查看数据库：
                 show dbs
                 使用：
                 use 数据库名
                 use 数据库名  --不存在就，延迟建数据库
                 查看集合列表
                 show collections
                 查看用户列表
                 show user
                 创建集合
                 db.集合名.save({'':'','':''})   //json 格式
                 查看集合数据
                 db.集合名.find()
                 db.集合名.find( { a : 1 } )
                 删除数据库
                 db.dropDatabase()  //删除当前
                 db.集合名.drop()

            MongoDB数据工具
                数据库组件
                    mongod
                    mongo
                    mongos
                数据库工具
                    数据库备份：
                    mongodump -h dbhost -d dbname -o dbdirectory
                    数据库恢复
                    mongorestore -h dbhost -d dbname --direcoryperdb dbdirectory
                    导出：
                        mongoexport -d dbs -c account -q {} -f name,addr --csv > account.csv  导出为csv
                        mongoexport -d dbs -c account -q {} -f name,addr  > account.json  导出为json
                    导入：
                        mongoimport -d chacha -c account --type csv --headerline --drop < account.csv  导入csv文件
                        mongoimport -d chacha -c account --type json --headerline --drop < account.json  导入json文件
        #三、MongoDB文档、集合、数据库
            文档是MongoDB中数据的基本单元
                文档中键值对是有序的
                文档的值不一定是字符串也可以是其他数据类型，或者嵌入其他文档
                键是字符串，可以使用任意utf-8
                键不能含有‘\0’
                键不能包含.和$
                以下划线“_”开头的键通常情况下是保留的
                区分大小写
                不能有重复键
            集合    看做没有模式的表
                集合不能为空“”
                不能含有空字符\0
                不能以“system.”开头
                不能包含保留字符$
            数据库   都有自己的集合和权限
                一个MongoDB实例可以承载多个数据库
                数据库名是utf-8字符串
                不能为空
                不能包含"、.、%、/、\和\0
                应全部小写

                保留数据库
                    admin
                    local
                    config  :
            命名空间
                数据库.集合名
        #四、数据类型
            null
            bool
            32位整型（shell不支持转为float64）
            64位整型（shell不支持转为float64）
            64位浮点数
            字符串  utf-8
            符号（shell不支持转为字符串）
            对象id   文档的12字节的唯一id  {"id":Object()}
            日期 从标准纪元开始的毫秒{'date':'new Date()'}
            正则表达式   JavaScript语法
            代码
            二进制数据（shell不支持）
            最大值  （shell不支持）BSON支持的数据类型，可能的最大值
            最小值  （shell不支持）BSON支持的数据类型，可能的最小值
            未定义  undefined
            数组
            内嵌文档

            Objectid 
            12字节
            时间戳          机器号          pID（进程号）              计数器
            4字节           3字节           2字节             3字节

            底层存储  bson    
        
        #五、MongoDB的增删改
            插入：
                db.account.insert({"":"","":""})
                批量插入最大消息为16M
                文档大小不超过4M
                插入原理：驱动会将文档转为bson，检查是否有id键值，传入数据库，数据库解析bson，不做有效性检验，原样存入数据库中
            删除：
                db.account.remove()     //删除数据保留索引
                db.account.drop()       //删除集合性能好
                db.account.remove({"":""})   //条件删除

            更新：
                db.account.update({"":""},{"":"","":""})  //条件-更新内容 
                db.account.update({"":""},{"$set"{"":""}})   //局部修改
                db.account.update({"":""},{"$unset"{"":""}})   //去掉键值
                db.account.update({"":""},{"$inc"{"":""}})   //加减  键值为数字
                db.account.update({"":""},{"$push"{"":""}})   //数组修改器
                db.account.update({"":""},{"$unsetaddToSet"{"":""}})   //避免重复加入
                db.account.update({"":""},{"$pop"{"":""}})   //从数组尾删除n个元素   1，-1
                db.account.update({"":""},{"$pull"{"":""}})   //从数组指定位置删除

                db.account.update({"":""},{"$set"{"":""}},false,true)   //多文档更新
                db.runCommand({getLastError:1})  //查看有多少文档被更新   驱动程序等待数据库返回结果
                
        #六、MongoDB查询
            db.account.find()         //查询所有文档
            db.account.find({"":""})          //条件查询
            指定返回某些键
                db.account.find({},{"userName":1})    //0   不返回，       1   返回
            复合条件查询
                lt、lte、gt、gte、ne   分别对应  <   、<=  、  >、   >= 、  !=
                start = new Date()
                db.account.find({"createTime":{"$gt":start}})
                db.account.find({"age":{"$in":[18,33]}})
                db.account.find({"age":{"$nin":[18,33]}})
                db.account.find({"$or"[{"":""},{"":""}]})
            正则表达式
                db.account.find({"userName":/^b/i})
            查询数组：
                db.account.find({"":""})   //单元素匹配
                db.account.find({"":{$all:["",""]})   //多元素匹配
                db.account.find({"fruit.2":""})   //数组下标位置匹配
                db.account.find({"fruit":{"$size":}})    //数组大小查询
            内嵌文档：
                db.account.find({"userName":{"firstname":"","lastname":""}})   //完全匹配
                db.account.find({"userName.firstname":"","userName.lastname":""})   //点查询发
            
            where查询
                无法使用索引，文档要从bson转javascript对象，查询速度慢
            游标



        #七、MongoDB索引
            创建索引：
                1、db.account.ensureIndex({"userName":1})   //单键索引  1 表示升序
                2、db.account.ensureIndex({"userName":1,"age":-1})  //复合索引
                只有索引前部的查询会得到优化
                索引优化查询的同时，会对增删改带来额外的开销

                唯一索引：
                    db.account.ensureIndex({"userName":1},{"unique":true})
                    默认插入数据时不检查数据唯一性
                    安全插入时会检查数据唯一性
                    id键索引就是唯一索引，并且不能删除
                    也可以创建唯一复合索引，单键可以重复，

                explain可分析查询使用索引的情况，耗时，及扫描文档数的统计
                    db.account.find({"":""}).explain()
                    db.account.find({"":""}.sort({""；""})).explain()
                    db.account.find({"age":{$gt:20,$lt:30}).explain()

                修改索引：
                    db.account.ensureIndex({"":""},{"background":true})
                删除索引：
                    db.runCommand({"dropIndexes":"account","index":""})
                hint()  //强制指定某种索引
                索引管理：
                system.indexes  //系统保留集合

        #八、聚合统计
            统计  count
            db.account.count();
            db.account.count({"":""});  //条件统计

            聚合  distinct
            db.runCommand({"distinct":"","key":""})
            db.runCommand({"distinct":"","key":""}).valuse.length

            分组   group
            db.account.group({key:{"age":true},initial:{number:0},reduce:function(doc.prev){prev.number+=1},cond:{"age":{"$gt":30}}})
                cond   //条件
                reduce  //定义一个方法，每次遇到一个文档要做的事情
        
        #九、命令的工作原理

        #十、固定集合
            事先创建，大小固定
            类似环状，空间不足队列头文件替换
            不能手工删除文档，只能自动替换

            特点：
                插入性能好
                按插入顺序查询，查询速度快

            语法：
                db.createCollection("fixed",{caped:true,size:10000})
                db.createCollection("fixed",{caped:true,size:10000,max:100})   //限制 100条文档，大小10k

            GridFS
                MongoDB中存储大二进制文件的机制
                不用使用单独的文件存储
                可直接利用MongoDB的复制分片机制，使文件存储也具有水平扩展、故障恢复功能
                不产生磁盘碎片，因为MongoDB分配数据文件时以2G为单位

                mongofiles -d bbs put account.json       //存储
                mongofiles -d bbs get account.json        //取出
                mongofiles -d bbs search account.json        //查找
                mongofiles -d bbs delete account.json        //删除
                mongofiles -d bbs list        //查看

        #十一、服务端脚本
            


        
