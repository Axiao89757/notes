# 一、Promise

## 1. 为什么而需要promise

> 需求

通过ajax请求id，通过id请求username，用过username请求Email

> 回调地狱

在回调函数中嵌套回调。弊端：嵌套函数存在耦合性，一旦有所改动就会牵一发而动全身；嵌套函数一多，就很难处理错误。

**总结：**当一次操作依赖于上一次操作的结果时，就会出现回调地狱。回调地狱不利于维护。故引入Promise来解决回调地狱问题。

## 2. Promise的基本使用

Promise是一个构造函数，通过new关键字实例化对象

> 语法

```js
new Promise((resolve, reject)=>{})
```

- Promise接受一个函数作为参数
- 在参数函数中接收两个参数
  - resolve：
  - reject：

### 1）promise三种状态

第一种状态：pending（准备，待解决，进行中）

第二种状态：fulfilled（已完成，成功）

第三种状态：rejected（已完成，失败）

### 2）promise状态的改变

总是由pending过度到fulfilled或者rejected，并且不可逆，一次性。

- 初始化：处于 pending

- 调用resolve：过度到 fulfilled

- 调用rejecte：过度到 rejected

### 3）promise的结果

> 示例

```js
const p = new Promise((resolve, reject) => {
    // 通过resolve，传递参数，改变当前promise对象的结果
    resolve('成功的结果')
    // reject('失败的结果')
})
console.dir(p)
```

## 3. Promise的方法

### 1）then方法

```js
const p = new Promise((resolve, reject) => {
    // 通过resolve，传递参数，改变当前promise对象的结果
    resolve('成功的结果')
    // reject('失败的结果')
})

// then方法
// 参数
// 1. 是一个函数
// 2. 还是一个函数
// 返回值：是一个promise对象
p.then(()=>{
    // 当promise的状态是fulfilled时，执行
    console.log('成功时调用')
}, ()=>{
    // 当promise的状态是rejected时，执行
    console.log('失败时调用')
})
console.dir(p)
```
```js
const p = new Promise((resolve, reject) => {
    // 通过resolve，传递参数，改变当前promise对象的结果
    resolve('成功的结果')
    // reject('失败的结果')
})

// then方法
// 参数
// 1. 是一个函数
// 2. 还是一个函数
// 返回值：是一个promise对象
p.then((value)=>{
    // 当promise的状态是fulfilled时，执行
    console.log('成功时调用', value)
}, (err)=>{
    // 当promise的状态是rejected时，执行
    console.log('失败时调用', err)
})
console.dir(p)
```

- 在then方法的参数函数中，通过形参使用promise对象的结果

> then方法返回一个新的Promise对象，状态是pending

```js
const p = new Promise((resolve, reject) => {
    // 通过resolve，传递参数，改变当前promise对象的结果
    resolve('成功的结果')
    // reject('失败的结果')
})

// then方法
// 参数
// 1. 是一个函数
// 2. 还是一个函数
// 返回值：是一个promise对象
const t = p.then((value)=>{
    // 当promise的状态是fulfilled时，执行
    console.log('成功时调用', value)
}, (err)=>{
    // 当promise的状态是rejected时，执行
    console.log('失败时调用', err)
})
console.dir(t)

// 链式操作
//new Promise((res, rej) => {}).then().then().then()...
```

> promise的状态不改变，then里面的方法不会执行

```js
new Promise((res, rej) => {
    
}).then((value) => {
    console.log('成功')
    
}, (reasion) => {
    console.log('失败')
})
```

> 在then方法中，通过return将返回的promise实例状态改为fulfilled
```js
const p = new Promise((res, rej) => {
    res(123)
})

const t = p.then((value) => {
    console.log('成功')
    // 使用return可以将t实例的状态改变成fulfilled
    //return 123
    // 如果这里的代码出错，会将t实例的状态改编为rejected
    //console.log(a)
}, (reasion) => {
    console.log('失败')
})

t.then((value) => {
    console.log('成功2', value)
    
}, (reasion) => {
    console.log('失败')
})

```

### 2）catch方法

> 示例

```js
const p = new Promise((res, rej) => {
    // reject()
    // console.log(a)
    throw new Error('出错了')
})

// 1. 当promise的状态改为rejected时，执行
// 2. 当promise执行体中出现代码错误时，执行
p.catch((reasion) => {
    console.log('失败'， reason)
})
console.log(p)
```

# 二、async  和 await

```js
async function main() {
    var data1 = await query(sql1)
    // todo 用data1拼接一个sql2
    var data2 = await query(sql2)
    // todo 用data2拼接一个sql3
    var data3 = await query(sql3)
}
```

如此即可有条理的解决回调地狱

[可以参考这个](https://www.cnblogs.com/liquanjiang/p/11409792.html)

- async 修饰的方法，声明了函数为异步函数，如果有await，判断接收的是否为promise，是则阻塞代码
- await 用来接收promise或者普通值，如果是promise对象，则阻塞代码，如果是普通值，顺序执行下去

```js
async function fun1() {
    var a = await new Promise((res, rej) => {
        setTimeout( () => {
            console.log("1")
            res(2)
        }, 1000)
    })

    console.log(a)
    console.log("3")
}
fun1()
// 输出结果：过1s打印 1 2 3

async function fun2() {
   const a = await 1
   setTimeout(function(){
        console.log("2")
    }, 3000)

   console.log(a)
}
fun2();
// 输出结果：打印 1 过3s打印 2

```

