---
title: JavaScript异步执行的几种方式
tags:
  - JavaScript
categories:
  - JavaScript
date: 2019-07-08 11:29:03
---

- setTimeout() / setInterval()

```
setTimeout(func, 0}

// HTML5 标准规定这个函数第二个参数不得小于 4 毫秒，不足会自动增加。
```

- setImmediate() 

```
setImmediate(func)
// Node API
```

- process.nextTick()

```
process.nextTick(func)
// Node API
```

- Promise
  
```
new Promise(resolve => {
  // code
  resolve()
})
  .then(func)
```

- Generator
  
```
function *gen() {
  yield 1
  yield 2
  yield 3
}

let g = gen()
g.next()  // {value: 1, done: false}
g.next()  // {value: 2, done: false}
g.next()  // {value: 3, done: false}
g.next()  // {value: undefined, done: true}
```

- async/await

```
function fn1() {
  console.log('Function1')
}

function fn2() {
  return new Promise(resolve => {
    console.log('Function2')
    resolve()
  })
}

function fn3() {
  console.log('Function3')
}

async function asyncFn() {
  fn1()
  await fn2()
  fn3()
}

asyncFn()

// Function1
// Function2
// Function3
```