1. 拉取镜像
``docker pull mongo``
可以查看镜像是否下载成功
``docker images | grep mongo``
应该会有如下的显示
``mongo                  latest         7177e01e8c01        2 months ago        393MB``
months ago 和 393MB 取决于镜像的拉取时间和对应版本的大小.

2. 使用 docker 安装 mongodb
``docker run --name mongodb -v ~/docker/mongo:/data/db -p 27017:27017 -d mongo``
执行上述命令之后, 一个挂载了 mongo镜像的容器就开始运行了 其中 * `--name` 设置了容器的名字 * `-v` 设置了路径的映射, 将本地路径映射到容器中. 此处, 路径可以自定义 * `-p` 设置了端口的映射, 将容器的27017(右侧) 映射到了本地的27017(右侧)

3. 进入容器.
``docker exec -it mongodb bash``
上述命令的意思如下: 使用交互的形式, 在 名字为 `mongodb` 的容器中实行 `bash`这个命令

4. `mongodb`的使用
   1.用户的创建和数据库的建立
用户的创建 * 输入以下命令进入 `mongo`
`mongo`

* 创建用户
````
# 进入 admin 的数据库
use admin
# 创建管理员用户
db.createUser(
   {
     user: "admin",
     pwd: "123456",
     roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
   }
 )
 # 创建有可读写权限的用户. 对于一个特定的数据库, 比如'demo'
 db.createUser({
     user: 'test',
     pwd: '123456',
     roles: [{role: "read", db: "demo"}]
 })
 ````
**数据库的建立**
``use demo;``

  2. mongo 是否正常启动的校验
先写入一条数据
`db.info.save({name: 'test', age: '22'})`
查看写入的数据
``db.info.find();``
结果如下
``
{ "_id" : ObjectId("5c973b81de96d4661a1c1831"), "name" : "test", "age" : "22" }``
> 其中的`_id`应该会和笔者的不同
远程连接的开启
在 `mongodb` 的容器当中
```
#更新源
``apt-get update``
# 安装 vim
apt-get install vim
# 修改 mongo 配置文件
vim /etc/mongod.conf.orig
```

>将其中的
`bindIp: 127.0.0.1`
注释掉`# bindIp: 127.0.0.1` 或者改成`bindIp: 0.0.0.0` 即可开启远程连接