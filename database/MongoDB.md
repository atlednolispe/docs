
## 开启密码访问

```bash
net:
  port: 27017
  bindIp: 0.0.0.0 

security:
    authorization: enabled
```

## 创建用户

```bash
$ mongo

> use admin

db.createUser(
     {
       user:"admin",
       pwd:"secret",
       roles:[{role:"root",db:"admin"}]
     }
  )

use admin
db.auth('', '')

use kuajingdianshang

> use test

> db.createUser(
    {
      user: "test",
      pwd: "secret",
      roles: ["readWrite"]
    }
)
``` 

## 导出导入数据库文件

```bash
$ mongoexport -d kuajingdianshang -c shenzhen_51job_clients -o kjds_sz

$ mongoimport -u kuajingdianshang -p password -d kuajingdianshang -c shenzhen_51job_clients kjds_sz
```

## 查看指定字段不同的值的个数

```bash
> db.clients.distinct('co_id').length
```


##  get json in robo3T

```javascript
records = [];
var cursor = db.getCollection('foo').find({}, {});
while(cursor.hasNext()) {
    records.push(cursor.next())
}
print(tojson(records));
````

# 在不同数据库之间复制collection

```javascript
db.clients.find().forEach( function(x){db.getSiblingDB('kuajingdianshang'）.ningbo_clients.insert(x)} );
```

## 更新某个key对应的dict的一部分

```javascript
db.test.update({co_id: 2}, {$set: {'key1.key2': 'value'}})
```

## 简单查询数据

```bash
db.collection1.find({'detail.phone': null}).sort({_id: 1}).limit(10)
```
