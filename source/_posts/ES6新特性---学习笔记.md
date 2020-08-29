---
title: ES6新特性 
index_img: /img/javascript_mini.png
excerpt: 记录了ES6的新特性以及我的一些理解
---

## 1.概述

+ **ECMAScript**是一种由ECMA国际在标准ECMA-262中定义的**脚本语言规范**。

+ [查看ECS6兼容性支持](http://kangax.github.io/compat-table/es6/)

## 2.let关键字

+ 变量不能重复声明

+ 块级作用域

  ```js
  {
  	let a=100;
  }  
  console.log(a);
  //以上代码会报错，在作用域外部无法访问到'a'变量
  //如果是var变量就可以访问到(变量提升)
  ```

+ 不存在变量提升

  **var:**

  ```js
  console.log(a); 
  var a=100;  
  // 输出 undefined
  // var变量进行变量提升，上述代码相当于在代码最开始进行变量声明 var a; 但未进行赋值
  ```
  **let:**

  ```js
  console.log(a); 
  let a=100;  
  // 报错
  ```

+ 不影响作用域链

  ```js
  {
  	let a=100;
  	function fn() {
  		console.log(a);
  	}
  	fn();
  }
  // 程序能够正常运行
  ```
  
## 2.const关键字

+ 常量

+ 一定要赋初始值

+ 常量的值不能修改

+ 块级作用域

  ```js
  {
  	const a=100;
  }  
  console.log(a);
  // 报错
  ```

+ 对于数组和元素的修改，不算做对常量的修改

  ```js
  const FRUIT = ['apple','banana','watermelon'];
  FRUIT.push('pear');
  console.log(FRUIT);
  // 不会报错，输出["apple", "banana", "watermelon", "pear"]
  // FRUIT常量对应的地址没有发生改变，改变的是地址里面的内容
  ```

  

## 3.解构赋值

+ ES6允许按照一定模式从**数组**和**对象**中提取值，对**变量**进行**赋值**

+ 数组的解构

  ```js
  const A = ['a','b','c','d'];
  let [one, two, three, four] = A;
  console.log(one);
  console.log(two);
  console.log(three);
  console.log(four);
  // 输出 a, b, c, d
  // 相当于对4个变量进行了赋值操作
  // 如果两边数量不相等，将会按顺序进行赋值，没有对应的变量或数据则不进行操作
  ```

+ 对象的解构

  ```js
  const zhang = {
      name: '张三',
      age: 24,
      sayHi: function() {
          console.log("Hi");
      }
  };
  let {name, age, sayHi} = zhang;
  console.log(name);
  console.log(age);
  console.log(sayHi);
  // 相当于赋值操作
  // 对象的解构赋值需要对应的属性名相同
  // 即便两边数量不相等，则寻找同名属性进行赋值
  ```

  ```js
  // 其中方法解构能够较好的降低代码重复率
  // 在上方代码块中，若不进行方法解构，调用函数时必须写为 zhang.sayHi();
  // 解构赋值之后，则可以写为 sayHi();
  ```

## 4.模板字符串

+ ES6引入了新的字符串声明方式    **反引号**---**``**

+ 使用``也可以声明字符串，效果同''  ""

  ```js
  let str = `I'm a string`;
  ```
  
+ 内容中可以直接出现换行符

  ```js
  let str = 'a
  		   b
             c';
  // 单引号和双引号这么写是不允许的，会报错
  // 想要完成换行效果需要写为 let str = 'a' + '\n' + 'b' + '\n' + 'c';
  ```

  ```js
  let str = `a
  		   b
  		   c`;
  // 反引号声明的字符串中允许出现换行符
  ```

+ 变量拼接

  ```js
  // 单引号和双引号
  let zhang = "张三";
  let hi = "你好，";
  // 进行拼接
  let sayHi = hi + zhang;
  console.log(sayHi);
  // 输出 你好，张三
  ```
  ```js
  // 反引号    在``中使用  ${}  模板
  let zhang = "张三";
  let sayhi = `你好，${zhang}`;
  console.log(sayhi);
  // 输出 你好，张三
  ```
  

## 5.对象的简化写法

+ ES6中允许在大括号里面，直接写入变量和函数，作为对象的属性和方法

  ```js
  let name = "张三"；
  let sayHi = function() {
      console.log('Hello');
  }
  const zhang = {
      name,	// 等效于 name: "张三"
      sayHi,	// 等效于 sayHi: function() { console.log('Hello'); }
      study(){
          console.log('我在学习');
      }		// 等效于 study: function() { console.log('我在学习'); }
  }
  ```

## 6.箭头函数

+ ES6允许使用箭头 => 来定义函数

  ```js
  let sayHi = (value) => {
      value = "Hello";
      console.log(value);
  }
  ```

+ ==**this**的指向是静态的，**this**始终指向函数声明时所在作用域下的**this**的值==

  ```js
  // 普通函数
  function getName1(){
      console.log(this.name);
  }
  
  // 箭头函数
  let getName2 = () => {
      console.log(this.name);
  }
  
  //设置window对象的name属性 
  window.name = 'zhangsan';
  const person = {
      name: 'lisi'
  }
  
  // 直接调用
  getName1();		// 输出 zhangsan 
  getName2();		// 输出 zhangsan
  
  // call方法调用  call可以改变函数内部的this值
  getName1.call(person);	// 输出lisi
  getName2.call(person);	// 输出zhangsan
  //箭头函数的this始终指向函数声明时所在作用域下的this值
  ```

+ 不能作为构造函数实例化对象

  ```js
  let Person = (name, age) => {
      this.name = name;
      this.age = age;
  }
  let zhangsan = new Person('zhangsan', 24);
  // 报错
  // 箭头函数不能作为构造函数
  ```

+ 不能使用 **arguments** 变量

  ```js
  let fn = () => {
      console.log(arguments);
  }
  fn(1,2,3);
  // 报错
  ```

+ 简写

  + 省略小括号

    ```js
    // 当形参有且只有一个时可以省略小括号
    let add = num => {
        return num + num;
    }
    console.log(add(9));
    ```

  + 省略大括号，当代码体只有一条语句的时候，将大括号省略，**return**也要一起省略，此时语句的执行结果就是函数的返回值

    ```js
    let add = num => num + num;
    console.log(add(9));
    ```

+ 箭头函数**适用**于与**this**无关的回调，例如定时器，数组的方法回调，不**适用**于与**this**有关的回调，例如事件的回调函数，对象的方法函数

## 7.函数参数的默认值设置

+ 形参的初始值		具有默认值的参数，一般位置要靠后

  ```js
  function add(a, b, c){
      return a + b + c;
  }
  let result = add(1,2);
  console.log(result);	// 输出为 NaN
  ```

  ```js
  function add(a, b, c=10){
      return a + b + c;
  }
  let result1 = add(1,2);
  let result2 = add(1,2,3);
  console.log(result1);	// 输出为 13
  console.log(result2);	// 输出为 6
  ```

+ 可以与解构赋值结合使用

  ```js
  function connect(host, username, password, port){
  	console.log(host);
      console.log(username);
      console.log(password);
      console.log(port);
      // 能够输出对应值
  }
  connect({
      host: 'localhost',
      username: 'root',
      password: 'root',
      port: 1234
  })
  ```

  ```js
  function connect(host='127.0.0.1', username, password, port){
  	console.log(host);	// 输出 127.0.0.1
      console.log(username);
      console.log(password);
      console.log(port);
  }
  connect({
      username: 'root',
      password: 'root',
      port: 1234
  })
  ```

## 8.**rest**参数

+ ES6引入**rest**参数，用于获取函数的实参，用以代替**arguments**

+ **rest**参数可以用到箭头函数中

+ **rest**参数 ... 放于函数**形参**中

  ```js
  // ES5中arguments参数演示
  function data() {
      console.log(arguments);
  }
  data('first','second','third');
  // arguments是一个对象
  
  
  // ES6中rest参数演示
  function data(...args) {
      console.log(args);
  }
  data('first','second','third');
  // args是一个数组
  ```

+ **rest**参数必须放到参数的最后

  ```js
  function fn(a, b, ...args) {
  	console.log(a);		// 输出 1
  	console.log(b);		// 输出 2
  	console.log(args);	// 输出[3, 4, 5, 6]
  }
  fn(1, 2, 3, 4, 5, 6);
  ```

## 9.扩展运算符

+ 扩展运算符   **...**

+ 能将**数组**转换为**逗号**分隔的**参数序列**

+ 扩展运算符 ...  放于函数**实参**中

  ```js
  const query = ['first', 'second', 'third'];
  function splitQuery() {
      console.log(arguments);
  }
  splitQuery(...query); 	// 等同于 splitQuery('first', 'second', 'third')
  ```

+ 应用

  + 数组的合并

    ```js
    const first = ['one', 'two'];
    const second = ['three', 'four'];
    
    // ES5中
    const result1 = first.concat(second);
    console.log(result1);	// 输出 ["one", "two", "three", "four"]
    
    // ES6中
    const result2 = [...first, ...second];
    console.log(result2);	// 输出 ["one", "two", "three", "four"]
    ```

  + 数组的克隆

    ```js
    const first = ['A', 'B','C'];
    const cloned = [...first];
    console.log(cloned);	// 输出 ["A", "B", "C"]
    ```

  + 将伪数组转为数组

    ```js
    function data() {
        console.log(arguments);	 // 这里输出的是一个伪数组
        console.log([...arguments]);	// 这里输出的是一个数组
    }
    data('first','second','third');
    ```

## 10.Symbol

+ ES6引入了一种新的**原始数据类型**，表示独一无二的值

+ Symbol是Javascript语言的第七种数据类型，是一种类似于字符串的数据类型

+ 特点

  + Symbol的值是唯一的，用来解决命名冲突的问题
  + Symbol值不能与其他数据进行运算
  + Symbol定义的对象属性不能使用 for...in 循环遍历，但是可以使用 **Reflect.ownKeys** 来获取对象的所有键名

+ 创建Symbol

  ```js
  let s1 = Symbol();
  let s2 = Symbol('hello');	// 括号内的字符串起描述的作用
  let s3 = Symbol('hello');
  console.log(s2 === s3);	// 输出 fasle，这里的s2和s3是两个不同的数据
  
  // Symbol.for创建
  let s4 = Symbol.for('hello');
  let s5 = Symbol.for('hello');
  console.log(s4 === s5);	// 输出true，这里的s4和s5是相同的数据
  
  // 不能与其他数据进行运算
  let result1 = s1 + 100;	// 四则运算，报错
  let result2 = s1 > 100;	// 比较，报错
  let result3 = s1 + 'hi'; // 字符串拼接，报错
  ```

+ Javascript语言的八种数据类型

  + undefined
  + string
  + object
  + null
  + number   
  + boolean
  + symbol
  + BigInt(新增)

+ Symbol创建对象属性

  + 向对象中添加方法

    ```js
    let game = {
        name: '俄罗斯方块',
        up: function() {
            console.log('up');
        }
    }
    
    //现在要向game对象中添加方法，但是在实际开发中要花费时间去确定game对象中是否已经存在要添加的方法，或者要分析game对象的结构，如果game对象的结构较为复杂，则会花费大量的事件
    
    // 使用Symbol向对象中添加方法(1)
    // 先声明一个对象
    let methods = {
        up: Symbol('up'),
        down: Symbol('down')
    };
    game[methods.up] = function() {
        console.log('向上');
    };
    game[methods.down] = function() {
        console.log('向下');
    }
    // 调用添加的方法
    game[methods.up]();	// 输出 向上
    game[methods.down]();	// 输出 向下
    ```

    ```js
    // 使用Symbol向对象中添加方法(2)
    let say = Symbol('say');
    let zibao = Symbol('zibao');
    let game = {
        name: '狼人杀',
        [say]: function() {
            console.log('我可以发言');
        },
        [zibao]: function() {
            console.log('我可以自爆');
        }
    }
    
    // 调用
    game[say]();	// 输出 我可以发言
    ```

## 11.迭代器iterator

+ 迭代器**Iterator**是一种接口，为各种不同的数据结构提供统一得到访问机制

+ **iterator**接口是对象的一个属性，即Symbol.iterator

+ 任何数据结构只要部署**Iterator**接口，就可以完成遍历操作

+ ES6创造了一种新的遍历命令  **for...of**  循环，Iteraotr接口主要供  **for...of**  消费

+ 原生具备**iterator**接口的数据（可用**for...of**遍历）

  + Array
  + Arguments
  + Set
  + Map
  + String
  + TypedArray
  + NodeList

  ```js
  const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
  // 使用for...of遍历数组
  for(let v of xiyou){
      console.log(v);	// 输出键值：唐僧, 孙悟空, 猪八戒, 沙僧
  }
  // 使用for...in遍历数组
  for(let v in xiyou){
      consoole.log(v) // 输出键名：0, 1, 2, 3
  }
  ```

+ 工作原理

  + 创建一个指针**对象**，指向当前数据结构的起始位置

    ```js
    const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
    
    let iterator = xiyou[Symbol.iterator]();
    ```

  + 第一次调用对象的**next**方法，指针自动指向数据结构的第一个成员

    ```js
    console.log(iterator.next());	// 输出 {value: "唐僧", done: false}
    ```

  + 接下来不断调用**next**方法，指针一直往后移动，直到指向最后一个成员

  + 每调用**next**方法返回一个包含**value**和**done**属性的对象

    ```js
    console.log(iterator.next());	// 输出 {value: "孙悟空", done: false}
    console.log(iterator.next());	// 输出 {value: "猪八戒", done: false}
    console.log(iterator.next());	// 输出 {value: "沙僧", done: false}
    console.log(iterator.next());	// 输出 {value: undefined, done: true}
    								// done属性为true则说明遍历或循环已完成
    ```

+ 应用---自定义遍历对象

  ```js
  // 声明一个对象
  const banji = {
      name: '1班',
      students: ['Marry', 'Jhon', 'Brown', 'Kelly'],
      [Symbol.iterator]() {
          let index = 0;
          return {	// 创建(返回)一个指针对象
              next: () => {	// 添加next方法
                  //  next要返回一个包含value和done的对象
                  if(index < this.students.length){
                      const result = { value: this.students[index], done: false};
                      index++;
                      return result;
                  } else {
                      return { value: undefined, done: true}
                  }
              }
          }
      }
  }
  
  // 遍历这个对象
  for(let v of banji){
      console.log(v);
  }	
  // 输出  Marry  Jhon   Brown   Kelly
  ```

## 12.生成器generator

+ 生成器是一个特殊的**函数**

+ 生成器**函数**是ES6提供的一种**异步编程**解决方案，语法行为与传统函数完全不同

  ```js
  // 定义
  function * gen() {
      console.log('hello');
  }
  let iterator = gen();
  console.log(iterator);	// 从此处结果可以看出生成器函数的返回结果是一个迭代器对象
  
  // 调用函数内部代码
  iterator.next();	// 输出hello
  ```

+ 函数代码的分隔符 **yield**

  ```js
  function * gen() {
      console.log('第一块代码');
      yield '引号内容';
      console.log('第二块代码');
      yield '引号内容可写可不写';
      console.log('第三块代码');
      yield '引号内容可以被遍历出来';
      console.log('第四块代码');
  }
  
  let iterator = gen();
  // 调用代码块
  iterator.next();	// 输出 第一块代码
  iterator.next();	// 输出 第二块代码
  iterator.next();	// 输出 第三块代码
  iterator.next();	// 输出 第四块代码
  // 调用代码块的返回值
  console.log(iterator.next());	// 先运行代码块，然后返回{ value: "引号内容", done: false}
  console.log(iterator.next());	// 先运行代码块，然后返回{ value: "引号内容可写可不写", done: false}
  console.log(iterator.next());	// 先运行代码块，然后返回{ value: "引号内容可以被遍历出来", done: false}
  console.log(iterator.next());	// 先运行代码块，然后返回{ value: undefined, done: true}
  ```

+ 生成器函数的参数传递

  ```js
  function * gen(arg) {
      console.log(arg);
      let one = yield 111;
      console.log(one);
      let two = yield 222;
      console.log(two);
      let three = yield 333;
      console.log(three);
  }
  let iterator = gen('AAA');	// 首次传参要在这里传入
  iterator.next();	// 输出 AAA，如果上一行代码没有传入参数，在这里传入的话，输出undefined
  iterator.next('BBB');	// 第二次调用next方法传入的参数将作为第一次 yield 语句返回的结果  						 // 执行第二段代码块，输出 BBB
  iterator.next('CCC');	// 第三次调用next方法传入的参数将作为第二次 yield 语句返回的结果  						 // 执行第三段代码块，输出 CCC
  iterator.next('DDD');	// 第四次调用next方法传入的参数将作为第三次 yield 语句返回的结果  						 // 执行第四段代码块，输出 CCC
  ```

+ 实例1---回调地狱

  ```js
  // 异步编程
  // 需求：1s后控制台输出 111， 2s后输出 222， 3s后输出 333
  
  // 回调地狱
  setTimeout(() => {
      console.log(111);
      setTimeout(() => {
          console.log(222);
          setTimeout(() => {
              console.log(333);
          },3000)
      },2000)
  },1000);
  // 上述代码，需要进行的异步任务越多，代码缩进越多，导致代码可读性差，调试困难---回调地狱
  
  // 生成器函数实现
  function one() {
      setTimeout(() => {
          console.log(111);
          iterator.next();	// 第一次调用完next后接着调用下一次
      },1000)
  };
  function two() {
      setTimeout(() => {
          console.log(222);
          iterator.next();	// 第二次调用完next后接着调用下一次
      },2000)
  };
  function three() {
      setTimeout(() => {
          console.log(333);
      },3000)
  };
  function* gen() {
      yield one();
      yield two();
      yield three();
  }
  // 调用生成器函数
  let iterator = gen();
  iterator.next();
  ```

+ 实例2---模拟获取

  ```js
  // 模拟获取---用户数据/订单数据/商品数据
  function getUsers() {
    setTimeout(() => {
        let data = "用户数据";
        iterator.next(data); // 第二次调用生成器函数，传入的数据会作为第一个yield的返回值
    }, 1000)
  }
  function getOrders() {
    setTimeout(() => {
        let data = "订单数据";
        iterator.next(data); // 第三次调用生成器函数，传入的数据会作为第二个yield的返回值
    }, 1000);
  }
  
  function getGoods() {
    setTimeout(() => {
        let data = "商品数据";
        iterator.next(data); // 第四次调用生成器函数，传入的数据会作为第三个yield的返回值
    }, 1000);
  }
  
  // 在实际应用中需要先有用户数据，才能有订单数据，再有商品数据，所以不能直接
  // getUsers();getOrders();getGoods();
  
  function * gen () {
    let users = yield getUsers();
    console.log(users);		// 输出 用户数据
    let orders = yield getOrders();
    console.log(orders);	// 输出 订单数据
    let goods = yield getGoods();
    console.log(goods);		// 输出 商品数据
  }
  let iterator = gen();
  iterator.next();	// 第一次调用生成器函数
  ```
  

## 13.Promise

+ **Promise**是ES6引入的异步编程的新解决方案

+ 语法上**Promise**是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果

  ```js
  // 实例化一个Promise对象	对象一共有三种状态：初始化，成功，失败
  const p = new Promise();	// 此时p的状态为 初始化
  ```

  ```js
  // 实例化时Promise对象接收一个函数参数，函数往往封装一个异步操作
  const p = new Promise(function(resolve, reject) {
      setTimeout(() => {
          // 模拟成功
          let data = "数据库中的用户数据";
          resolve(data);	// 调用resolve后，设置p的状态为 成功
          
          // 模拟失败
          let err = "数据读取失败";
          reject(err);	// 调用reject后，设置p的状态为 失败
      },1000)
  });
  
  // 可以调用promise对象的then方法
  // then方法接收两个函数参数,第一个是成功之后的回调，第二个是失败之后的回调
  p.then(function(value) {
      // 对象的异步操作中调用resolve即成功后，then方法就会执行第一个回调函数
      console.log(value);
  }, function(reason) {
      console.error(reason);
  });
  ```

+ 实例---使用Promise封装AJAX请求

  ```js
  // 1.创建对象
  const xhr = new XMLHttpRequest();
  const p = new Promise((resolve, reject) => {
      // 2.初始化
  	xhr.open('GET', 'https://api.apiopen.top/getJoke');
  
  	// 3.发送
  	xhr.send();
  
  	// 4.绑定事件
  	xhr.onreadystatechange = function() {
   	   // 判断
   	   if(xhr.readyState === 4) {
    	      // 判断响应状态码---200到300之间为成功
     	     if(xhr.status >= 200 && xhr.status < 300) {
     	         resolve(xhr.response);
       	 } else {
       	       reject(xhr.status);
           }
     	   }
  	}
  }).then(function(value){
      console.log(value);
  }, function(reason){
      console.log(reason);
  });
  ```

+ **then**方法的返回对象

  ```js
  // 创建Promise对象
  const p = new Promise((resolve, reject) => {
     setTimeout(() => {
         resolve('用户数据');
     },1000); 
  });
  
  // 调用then方法
  // then方法的返回结果是Promise对象，对象状态由回调函数的执行结果决定
  // 1.如果回调函数中返回的结果是非promise类型的属性，且状态为成功，则返回对象PromiseStatus为		 fulfilled，返回值PromiseValue为成功的值
  const result = p.then(value => {
      console.log(value);
      // 返回非promise类型的值
      return 123;
  }, reason => {
      console.warn(reason);
  });
  console.log(result); // 若成功：PromiseStatus为fulfilled，PromiseValue为123
  ```

  ```js
  // 创建Promise对象
  const p = new Promise((resolve, reject) => {
     setTimeout(() => {
         resolve('用户数据');
     },1000); 
  });
  
  // 调用then方法
  // then方法的返回结果是Promise对象，对象状态由回调函数的执行结果决定
  // 2.如果回调函数中返回的结果是promise类型的属性，且状态为成功，则返回对象PromiseStatus为	//   返回的promise对象的状态值（成功/失败），返回值PromiseValue为返回的promise对象成功/失败  //   的值
  const result = p.then(value => {
      console.log(value);
      // 返回promise类型的值
      return new Promise((resolve, reject) => {
          resolve('OK');// 若p成功：PromiseStatus为fulfilled，PromiseValue为OK
          reject('error');// 若p成功：PromiseStatus为rejected，PromiseValue为error
          // 失败还可以写为抛出错误   throw 'error';  结果一样
      });
  }, reason => {
      console.warn(reason);
  });
  console.log(result); 
  ```

+ then方法的链式调用

  ```js
  p.then(value => {}, reason => {}).then(value => {}, reason => {}).then(value => {}, reason => {});
  // 链式调用也是解决回调地狱的一种方法
  ```

+ catch方法

  ```js
  const p = new Promise((resolve, reject) => {
      setTimeout(() => {
          // 设置p的状态为 失败，并设置失败的值
          reject('error');
      }, 1000);
  });
  // then方法
  // p.then(reason => {
  //     console.error(reason);
  // });
  // catch方法
  p.catch(reason => {
      console.error(reason);
  });
  ```

## 14.Set

+ ES6提供了新的数据结构**Set**(集合)

  ``` js
  // 声明一个集合
  let s = new Set();
  console.log(s, typeof s); // 输出 Set[0]{}    "object"
  ```

+ **Set**类似于数组，但成员的值都是唯一的

  ```js
  let s = new Set(['A', 'B', 'C', 'D', 'C']);
  console.log(s);	// 输出 Set[4]{"A", "B", "C", "D"}
  // 传入数据时会自动去重
  ```

+ 集合的属性和方法
  + **size**    返回集合的元素个数
  
    ```js
    let s = new Set(['A', 'B', 'C', 'D']);
    console.log(s.size); // 输出 4
    ```
  
  + **add**    增加一个新的元素，返回当前集合
  
    ```js
    s.add('E');
    console.log(s); // 输出 Set[5]{"A", "B", "C", "D", "E"}
    ```
  
  + **delete**    删除元素，返回boolean值
  
    ```js
    let result = s.delete('D');
    console.log(result); //输出 true 
    console.log(s); // 输出 Set[4]{"A", "B", "C", "E"}
    console.log('H'); // 输出false
    ```
  
  + **has**    检测集合中是否包含某个元素，返回boolean值
  
    ```js
    console.log(s.has('A')); // 输出true
    console.log(s.has('G')); // 输出false
    ```
  
  + **clear**    清空集合，返回undefined
  
    ```js
    s.clear();
    console.log(s); // 输出 Set(0) {}
    ```
  
+ 集合(Set)实现了iterator接口，所以可以使用==扩展运算符==和==for...of==进行遍历

  ```js
  const s = new Set(['A', 'B', 'C']);
  for (let v of s){
      console.log(v);	// 输出 A B C
  }
  ```

+ 实例1---数组去重

  ```js
  let arr = [1, 2, 3, 4, 5, 3, 1, 2, 6, 3, 5];
  let result = [...new Set(arr)]; // 使用扩展运算符将集合转为数组
  ```

+ 实例2---交集

  ```js
  let arr1 = [1, 2, 3, 4, 5, 2, 4, 3];
  let arr2 = [4, 2, 5, 6, 9];
  
  let result = [...new Set(arr1)].filter(item => {
      let s2 = new Set(arr2);
      if( s2.has(item)) {
          return true;
      } else {
          return false;
      }
  })
  
  // 简化为下面的写法
  let result = [...new Set(arr1)].filter(item => new Set(arr2).has(item));
  ```

+ 实例3---并集

  ```js
  let arr1 = [1, 2, 3, 4, 5, 2, 4, 3];
  let arr2 = [4, 2, 5, 6, 9];
  
  let union = [...arr1, ...arr2];
  let result = [...new Set(union)];
  
  // 简化为下面的写法
  let result = [...new Set([...arr1, ...arr2])];
  ```

+ 实例3---差集

  ```js
  let arr1 = [1, 2, 3, 4, 5, 2, 4, 3];
  let arr2 = [4, 2, 5, 6, 9];
  
  // 差集就是交集的逆运算
  let result = [...new Set(arr1)].filter(item => !new Set(arr2).has(item));
  ```

## 15.Map

+ ES6提供了**Map**数据结构

  ```js
  // 声明Map
  let m = new Map();
  ```

+ **Map**类似于对象，也是键值对的集合。但是"键"的范围不限于字符串，各种类型的值(包括对象)都可以当作键

+ **Map**的属性和方法

  + **size**    返回**Map**的元素个数

    ```js
    let m = new Map();
    console.log(m.size); // 输出 0
    ```

  + **set**    添加一个新元素，返回当前**Map**

    ```js
    m.set('name', 'zhangsan');
    
    // 键可以是各种类型的字符串
    let  key = {
        name: 'fruit'
    }
    m.set(key, ['apple','pear']);
    ```

  + **delete**   删除一个元素，返回boolean值

    ```js
  m.delete('name');
    ```
  
  + **get**    返回键名对象的键值

    ```js
  let result = m.get('name');
    console.log(result);
    ```
  
  + **has**    检测**Map**中是否包含某个元素，返回boolean值
  
    ```js
    m.has('name');
    ```
  
  + **clear**    清空集合，返回undefined
  
    ```js
    m.clear();
    ```
    
  + **keys**    将Map对象的每个元素的键构成一个**iterator**对象，将其返回
  
    ```js
    const map = new Map([['1', 'a'], ['2', 'b']]);
    console.log(map.keys()); // ['1', '2']
    ```
  
  + **values**    将Map对象的每个元素的值构成一个**iterator**对象，将其返回
  
    ```js
    const map = new Map([['1', 'a'], ['2', 'b']]);
    console.log(map.values()); // ['a', 'b']
    ```
  
+ **Map**也实现了iterator接口，所以可以使用==扩展运算符==和==for...of==进行遍历

  ```js
  for (let v of m){
      console.log(v);
  }
  ```

## 16.Class类

+ ES6提供了更接近传统语言的写法，引入了**Class**(类)这个概念，作为对象的模板

+ 基本上，ES6的**class**可以看作只是一个语法糖，他的绝大部分功能，ES5都可以做到，新的**class**写法只是让对象原型的写法更加清晰，更像面向对象编程的语法而已

  ```js
  // ES5中通过构造函数实例化对象
  
  // 构造手机
  function Phone(brand, price) {
      this.brand = brand;
      this.price = price;
  }
  
  // 添加方法
  Phone.prototype.call = function() {
      console.log('我可以打电话');
  }
  
  // 实例化对象
  let Huawei = new Phone('华为', 999);
  // 调用内部的方法
  Huawei.call();
  ```

  ```js
  // 用ES6的class语法实现上面的例子
  class Phone{
      // 构造方法，名字时固定的，在使用new实例化对象时自动调用该函数
      constructor(brand, price){
          this.brand = brand;
          this.price = price;
      };
      // 添加方法必须是下面的格式，不能使用ES5的格式
      call() {
          console.log('我可以打电话');
      };
  }
  
  // 实例化对象
  let Huawei = new Phone('华为', 999);
  ```

+ 类的静态成员

  ```js
  // ES5
  function Phone() {
  }
  // 构造函数也是一个对象，可以给构造函数添加属性
  // 这里的name和call是构造函数的属性，不是实例化对象的属性，这样的属性称为静态成员
  Phone.name = '手机';
  Phone.call = function() {
    console.log('我可以打电话');
  }
  // 实例化对象
  let Huawei = new Phone();
  
  // 实例化对象与构造函数的属性的属性不共通
  console.log(Huawei.name); // 返回 undefined
  Huawei.call();  // 报错
  
  // 实例化对象与构造函数原型对象的属性共通
  Phone.prototype.size = '5.5inch';
  console.log(Huawei.size); // 返回 5.5inch  
  ```
  
  ```js
  // ES6
  class Phone{
    // 静态属性
    static name = '手机';
    static call(){
        console.log('我可以打电话');
    }
  }
  // 实例化
  let nokia = new Phone();
  console.log(nokia.name); // 输出 undefined
  console.log(Phone.name); // 输出 手机
  ```
  
  
  
+ 对象继承

  ```js
  // ES5
  // 构造手机
  function Phone(brand, price) {
      this.brand = brand;
      this.price = price;
  }
  
  Phone.prototype.call = function() {
      console.log('我可以打电话');
  }
  
  // 构造智能手机
  // 智能手机应该有手机的一切属性，不必要再写重复的代码，可以使用继承
  function SmartPhone(brand, price, color, size) {
      // 继承
      Phone.call(this, brand, price);
      // 初始化独有的属性
      this.color = color;
      this.size = size;
      // 设置子级构造函数的原型
      SmartPhone.prototype = new Phone;
      // 校正
      SmartPhone.prototype.constructor = SmartPhone;
      
      // 声明子类的方法
      SmartPhone.prototype.photo = function() {
          console.log('我可以拍照');
      }
  }
  
  // 实例化对象
  const xiaomi = new SmartPhone('小米', 2999, '白色', '5.5inch');
  ```

+ 类继承

  ```js
  // ES6
  // 构造手机(父类)
  class Phone {
      // 构造方法
      constructor(brand, price) {
          this.brand = brand;
          this.price = price;
      }
      // 父类的成员属性
      call() {
          console.log('我可以打电话');
      }
  }
  
  // 构造智能手机(子类)
  class SmartPhone extends Phone {
      // 构造方法
      constructor(brand, price, color, size){
          super(brand, price); // super即为父类的构造方法函数
          					 // 等于ES5的Phone.call(this, brand, price)
          this.color = color;
          this.size = size;
      }
      
      // 子类独有的方法 
      photo() {
          console.log('我可以拍照');
      }
      // 子类对父类方法的重写
      call() {
          console.log('我可以视频通话');
      }
  }
  // 实例化对象
  const xiaomi = new SmartPhone('小米', 2999, '白色', '5.5inch');
  ```

+ **get**和**set**

  ```js
  class Phone{
      // 当调用price属性时就会调用此函数
      get price(){
          console.log('price属性被读取了');
          return 'aaa';
      }
      // 当修改price属性时就会调用此函数，必须要有参数
      set price(value){
          console.log('price属性被修改了');
      }
  }
  let xiaomi = new Phone();
  console.log(xiaomi.price); // 返回 price属性被读取了  和  aaa
  					  // 前者是执行了get,后者是s.price的返回值
  xiaomi.price = 'free'; // 此时会输出 price属性被修改了
  ```

## 17.数值扩展

+ **Number.EPSILON**

  + **Number.EPSILON**是javascript表示的最小精度

  + **Number.EPSILON**的值接近于2.220446049250313e-16，即2的-52次方

  + 引入一个这么小的量的目的，在于为浮点数计算，设置一个误差范围

    ```js
    // 浮点数计算是不精确的
    console.log(0.1 + 0.2); // 输出 0.30000000000000004
    console.log(0.1 + 0.2 === 0.3); // 输出fasle
    
    // 判断两个浮点数是否相等
    function equal(a, b) {
        if(Math.abs(a - b) < Number.EPSILON){
            return true;
        } else {
            return fasle;
        }
    }
    
    console.log(equal(0.1 + 0.2, 0.3)); // 输出true
    ```

+ 二进制和八进制和十六进制

  + 二进制  0b

    `let b = 0b1010;`

  + 八进制  0o

    `let o = 0o777;`

  + 十六进制  0x

    `let x = 0xff;`

+ **Number.isFinite**

  ```js
  // 判断一个数是否为有限数
  console.log(Number.isFinite(100)); // 输出 true
  ```

+ **Number.isNaN**

  ```js
  // 检测一个数值是否为NaN
  console.log(Number.isNaN(123)); // 输出 false
  ```

+ **Number.parseInt**        **Number.parseFloat**

  ```js
  // 将字符串转为数字
  console.log(Number.parseInt('123456abc')); // 输出 123456
  console.log(Number.parseFloat('1.23456abc')); // 输出 1.23456
  ```

+ **Number.isInteger**

  ```js
  // 判断一个数是否为整数
  console.log(Number.isInterger(5.1)); // 输出 false
  ```

+ **Math.trunc**

  ```js
  // 将数字的小数部分抹掉
  console.log(Math.trunc(1.234)); // 输出 1
  ```

+ **Math.sign**

  ```js
  // 判断一个数是正数负数或0
  console.log(Math.sign(0)); // 输出 0
  console.log(Math.sign(-4)); // 输出 -1
  console.log(Math.sign(5)); // 输出 1
  ```

## 18.对象方法扩展

+ **Object.is**

  ```js
  // 判断两个值是否完全相等
  console.log(Object.is(123, 124)); // 输出 false
  
  // 与 === 相似但有不同
  console.log(Object.is(NaN, NaN)); // 输出 true
  console.log(NaN === NaN); // 输出 false
  ```

+ **Object.assign**

  ```js
  // 对象的合并
  const config1 = {
      host: 'localhost',
      port: 8080,
      user: 'root',
      password: 'root'，
      test1: 'test1'
  }
  const config2 = {
      host: '127.0.0.1',
      port: 5500,
      user: 'admin',
      password: 'admin',
      test2: 'test2'
  }
  
  // Object.assign(被覆盖的对象/模板, 用来覆盖的对象)
  const result = Object.assign(config1, config2);
  console.log(result); // 重名的属性直接覆盖,config1有而config2没有的属性不变
  					 // config2有而config1没有的属性也会被添加
  ```

+ **Object.setPrototypeOf**        **Object.getPrototypeOf**

  ```js
  const school = {
      name: '北大'
  }
  const city = {
      xiaoqu: '北京'
  }
  // 设置原型对象
  Object.setPrototypeOf(school, city);
  console.log(school);
  // 获取原型对象
  console.log(Object.getPrototypeOf(school));
  ```

  

## 19.模块化语法

+ **export**        用于规定模块的对外接口
+ **import**        用于输入其他模块提供的功能