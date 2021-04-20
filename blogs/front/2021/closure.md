---
title: 闭包使用场景
date: 2021-4-20
tags:
  - JavaScript
  - 前端
categories:
  - 前端
---

### 闭包

#### 什么是闭包

闭包很简单，就是能够访问另一个函数作用域变量的函数，更简单的说，闭包就是函数，只不过是声明在其它函数内部而已。

例如：

```
function getOuter(){
  var count = 0
  function getCount(num){
    count += num
    console.log(count) //访问外部的date
  }
  return getCount //外部函数返回
}
var myfunc = getOuter()
myfunc(1) // 1
myfunc(2) // 3
```

`myfunc` 就是闭包， `myfunc` 是执行 `getOuter` 时创建的 `getCount` 函数实例的引用。 `getCount` 函数实例维护了一个对它的词法环境的引用，所以闭包就是函数+词法环境

当 `myfunc` 函数被调用时，变量 `count` 依然是可用的，也可以更新的

```
function add(x){
    return function(y){
        return x + y
    };
}

var addFun1 = add(4)
var addFun2 = add(9)

console.log(addFun1(2)) //6
console.log(addFun2(2))  //11
add` 接受一个参数 `x` ，返回一个函数,它的参数是 `y` ，返回 `x+y
```

`add` 是一个函数工厂，传入一个参数，就可以创建一个参数和其他参数求值的函数。

```
addFun1` 和 `addFun2` 都是闭包。他们使用相同的函数定义，但词法环境不同， `addFun1` 中 `x` 是 `4` ，后者是 `5
```

即：

- 闭包可以访问当前函数以外的变量
- 即使外部函数已经返回，闭包仍能访问外部函数定义的变量与参数
- 闭包可以更新外部变量的值

所以，闭包可以：

- 避免全局变量的污染
- 能够读取函数内部的变量
- 可以在内存中维护一个变量
