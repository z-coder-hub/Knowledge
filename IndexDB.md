### 什么是IndexDB

1. 它是浏览器的存储方式
2. 因为webstorage的存储大小有限制，当满足不了开发需求是选择使用IndexDB存储
3. 它是一个异步的API，要通过不同的API监听一些回调
4. 它的存储大小在50~100MB之间，具体在浏览器支持多少



### 使用

#### 创建IndexDB对象

```js
let request = window.indexDB.open("数据库名称", 版本号) // 版本号一般不用指定

request.error = (event)=>{
    // 如果连接成功不会运行该回调函数
}

request.onupgradeneeded = (event)=>{
    // 版本更新之后就会触发，首次会触发一次
    // 但是输出没有效果 比如console.log
    // 主要的作用就是创建数据表
    const db = event.target.result
    db.createObjectStore("数据表名称", option)
    // option 主要配置主键(keyPath)、自增(autoIncrement)
}

request.succese = (event)=>{
    // 成功之后的回调
    // 具体参数，输出event.target.result
}
```



#### 添加 add

```js
// transaction中第二个参数用来规定是否可添加、修改、删除。不填的话用于获取数据
const transaction = db.transaction("数据表", "readwrite")
const store = transaction.objectStore('数据表')
const request = store.add({...})
request.onsuccess = (event)=>{
    // 成功之后的回调
}
```



#### 查询get | getAll

```js
const transaction = db.transaction("数据表")
const store = transaction.objectStore('数据表')
const request = store.get(keyPath的值) | store.getAll()
request.onsuccess = (event)=>{
    // 成功之后的回调
    // 数据
    let data = event.target.result
}
```



#### 删除 delete

```js
const transaction = db.transaction("数据表", "readwrite")
const store = transaction.objectStore('数据表')
const request = store.delete(keyPath的值)
request.onsuccess = (event)=>{
    // 成功之后的回调
}
```



#### 修改 put

```js
const transaction = db.transaction("数据表", "readwrite")
const store = transaction.objectStore('数据表')
const request = store.put({id:1, name:'修改', age:20})// 直接覆盖某一个字段
```





