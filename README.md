# JS设计模式

## 1. 面向对象

### 1.1 类

- 类，及模板
- 属性
- 行为

### 1.2 对象

- 类的实例

```js
class Person{
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  eat() {
    alert(`${this.name} eat something`)
  }
  speak() {
    alert(`My name is ${this.name}, age ${this.age}`)
  }
}

let zhang = new Person('zhang', 20)
zhang.eat()
zhang.speak()
let wang = new Person('wang', 21)
wang.eat()
wang.speak()
```

### 1.3 三要素

#### 1.3.1 继承

- 子类继承父类
- 继承可以将公共方法抽离出来，提高复用，减少冗余

```js
// 父类
class People{
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  eat() {
    alert(`${this.name} eat something`)
  }
  speak() {
    alert(`My name is ${this.name}, age ${this.age}`)
  }
}
// 子类
class Student extends People {
  constructor(name, age, number) {
    super(name, age)
    this.number = number
  }
  study() {
    alert(`${this.name} study`)
  }
}
let xiaoming = new Student('xiaoming', 10, 'A1')
xiaoming.study()
alert(xiaoming.number)
xiaoming.eat()
let xiaohong = new Student('xiaohong', 11, 'A2')
xiaohong.study()
xiaohong.speak()
```

#### 1.3.2 封装

- 数据的权限和保密
- public：完全开放
- protected： 对子类开放
- private：对自己开放
- 减少耦合，不该外露的不外露
- 利于数据、接口的权限管理
- ES6目前不支持，一般认为_开头的属性是private

```ts
// 父类
class People {
    name
    age
    protected weight // 受保护的属性，只有自己或者子类可以访问
    constructor(name, age) {
        this.name = name
        this.age = age
        this.weight = 120
    }
    eat() {
        alert(`${this.name} eat something`)
    }
    speak() {
        alert(`My name is ${this.name}, age ${this.age}`)
    }
}
// 子类
class Student extends People {
    number
    private girlfriend
    constructor(name, age, number) {
        super(name, age)
        this.number = number
        this.girlfriend = 'xiaoli'
    }
    study() {
        alert(`${this.name} study`)
    }
    getWeight() {
        alert(`weight ${this.weight}`)
    }
}
let xiaoming = new Student('xiaoming', 10, 'A1')
xiaoming.getWeight()
alert(xiaoming.name)
```

#### 1.3.3 多态

- 同一接口的不同实现
- JS应用极少
- 需要结合java等语言的接口、重写、重载等功能

- 保持子类的开放性和灵活性
- 面向接口编程
- （JS引用极少，了解即可）

## 2. UML类图

### 2.1 关系

- 泛化，表示继承
- 关联，表示引用

## 3. SOLID五大设计原则

### 3.1 S - 单一职责原则

- 一个程序只做好一件事
- 如果功能过于复杂就拆分开，每个部分保持独立

### 3.2 O - 开放封闭原则

- 对扩张开发，对修改封闭
- 增加需求时，扩展新代码，而非修改已有代码
- 这是软件设计的终极目标

### 3.3 L - 李氏置换原则

- 子类能覆盖父类
- 父类能出现的地方子类就能出现
- JS中使用较少(弱类型&继承使用较少)

### 3.4 I - 接口独立原则

- 保持接口的单一独立，避免出现“胖接口”
- JS中没有接口(typescript例外)，使用较少
- 类似于单一职责原则，这里更关注接口

### 3.5 D - 依赖倒置原则

- 面向接口编程，依赖与抽象而不依赖与具体
- 使用方只关注接口而不关注具体类的实现
- JS中使用较少(没有接口&弱类型)

## 4. 设计模式

- 创建型
  - 工厂模式（工厂方法模式，抽象工厂模式，建造者模式）
  - 单例模式
  - 原型模式
- 结构型
  - 适配器模式
  - 装饰器模式
  - 代理模式
  - 外观模式
  - 桥接模式
  - 组合模式
  - 享元模式
- 行为型
  - 策略模式
  - 模板方法模式
  - 观察者模式
  - 迭代器模式
  - 职责链模式
  - 命令模式
  - 备忘录模式
  - 状态模式
  - 访问者模式
  - 中介者模式
  - 解释器模式

### 4.1 工厂模式

#### 4.1.1 介绍

- 将new操作单独封装
- 遇到new时，就要考虑是否使用该工厂模式

#### 4.1.2 UML类图和代码示例

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F.png)

```js
class Product {
  constructor(name) {
    this.name = name
  }
  init() {
    alert('init')
  }
  fn1() {
    alert('fn1')
  }
  fn2() {
    alert('fn2')
  }
}

class Creator {
  create(name) {
    return new Product(name)
  }
}

// 测试
let creator = new Creator()
let p = creator.create('p1')
p.init()
p.fn1()
```

#### 4.1.3 场景

- jQuery - $('div')
  - $('div')和new $('div')有何区别？
    - 第一：书写麻烦，jQuery的链式操作将成为噩梦
    - 第二：一旦jQuery名字发生变化，僵尸灾难性的

```js
class jQuery{
  constructor(selector) {
    let slice = Array.prototype.slice
    let dom = slice.call(document.querySelectorAll(selector))
    let len = dom ? dom.length : 0
    for(let i=0; i<len; i++) {
      this[i] = dom[i]
    }
    this.length = len
    this.selector = selector || ''
  }
  append(node) {

  }
  addClass(name) {

  }
  html(data) {

  }
}

window.$ = function (selector) {
  return new jQuery(selector)
}
```

- React.createElement
- vue异步组件

#### 4.1.4 设计原则验证

- 构造函数和创建者分离
- 符合开放封闭原则

### 4.2 单例模式

#### 4.2.1 介绍

- 系统中被唯一使用
- 一个类只有一个实例

#### 4.2.2 代码示例

```js
class SingleObject {
  login() {
    console.log('login...');
  }
}

SingleObject.getInstance = (function() {
  let instance
  return function() {
    if(!instance) {
      instance = new SingleObject()
    }
    return instance
  }
})()

let obj1 = SingleObject.getInstance()
obj1.login()
let obj2 = SingleObject.getInstance()
obj2.login()

console.log('obj1 === obj2', obj1 === obj2);

```

#### 4.2.3 场景

- jQuery只有一个$
- 模拟登陆框

```js
class LoginFrom {
  constructor() {
    this.state = 'hide'
  }
  show() {
    if(this.state === 'show') {
      alert('已经显示')
      return
    }
    this.state = 'show'
    console.log('登录框显示成功')
  }
  hide() {
    if(this.state === 'hide') {
      alert('已经隐藏')
      return
    }
    this.state = 'hide'
    console.log('登录框隐藏成功')
  }
}

LoginFrom.getInstance = (function() {
  let instance
  return function() {
    if(!instance) {
      instance = new LoginFrom()
    }
    return instance
  }
})()

// 测试
let login1 = LoginFrom.getInstance()
login1.show()

let login2 = LoginFrom.getInstance()
login2.show()
// login2.hide()
console.log('login1 === login2', login1 === login2);
```

- 其他
  - 购物车(和登录框类似)
  - vuex和redux中的store

#### 4.2.4 设计原则验证

- 符合单一职责原则，只实例化唯一的对象
- 没法具体体现开放封闭原则，也没有违反开放封闭原则

### 4.3 适配器模式

#### 4.3.1 介绍

- 旧接口与使用者不兼容
- 中间加一个适配器转换接口

#### 4.3.2 UML类图和示例代码

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F.png)



```js
class Adaptee {
  specificRequest() {
    return '德国标准插头'
  }
}

class Target {
  constructor() {
    this.adaptee = new Adaptee()
  }
  request() {
    let info = this.adaptee.specificRequest()
    return `${info} - 转换器 - 中国标砖插头`
  }
}

// 测试
let target = new Target()
let res = target.request()
console.log(res)

```

#### 4.3.3 场景

- 封装旧接口
- vue computed

#### 4.3.4 设计原则验证

- 将旧接口和使用者进行分离
- 符合开放封闭原则

### 4.4 装饰器模式

#### 4.4.1 介绍

- 为对象添加新功能
- 不改变其原有的结构和功能

#### 4.4.2 UML类图和示例代码

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E8%A3%85%E9%A5%B0%E5%99%A8%E6%A8%A1%E5%BC%8F.png)

```js
class Circle {
  draw() {
    console.log('画一个图形')
  }
}

class Decorator {
  constructor(circle) {
    this.circle = circle
  }
  draw() {
    this.circle.draw()
    this.setRedBorder(circle)
  }
  setRedBorder(circle) {
    console.log('设置红色边框')
  }
}

// 测试
let circle = new Circle()
circle.draw()
console.log('----分割线----')
let dec = new Decorator(circle)
dec.draw()
```

#### 4.4.3 场景

- ES7装饰器

  - 配置环境、安装插件

    - 安装插件`npm isntall babel-plugin-transform-decorators-legacy --save-dev`

    - 配置.babelrc文件

      ```json
      {
        "presets": ["es2015", "latest"],
        "plugins": ["transform-decorators-legacy"]
      }
      ```

    - 需要注意的是，该插件专门用于Babel6.x，如果是Babel7，可使用`@babel/plugin-proposal-decorators`

      ```json
      {
        "plugins": [
          ["@babel/plugin-proposal-decorators", { "legacy": true }],
        ]
      }
      ```

    - 简单使用

      ```js
      @testDec
      class Demo {
        // ...
      }
      
      function testDec(target) {
        target.isDec = true
      }
      alert(Demo.isDec) // true
      ```

      

  - 装饰类

    - 装饰器的原理

      ```js
      // 装饰器的原理
      @decorator
      class A {}
      // 等同于
      class A {}
      A = decorator(A) || A
      ```

    - 带参数

      ```js
      function testDec(isDec) {
        return function (target) {
          target.isDec = isDec
        }
      }
      
      @testDec(false)
      class Demo {
      
      }
      
      alert(Demo.isDec)
      ```

    - mixins

      ```js
      function mixins(...list) {
        return function (target) {
          Object.assign(target.prototype, ...list)
        }
      }
      
      const Foo = {
        foo() {
          alert('foo')
        }
      }
      
      @mixins(Foo)
      class MyClass {
      
      }
      
      let obj = new MyClass()
      obj.foo()
      ```

  - 装饰方法

    - readonly只读

      ```js
      function readonly (target, name, descriptor){
        /**
         * descriptor 属性描述对象(Object.defineProperty 中会用到)，原来的值如下
         * {
         *    value: specifiedFunction,
         *    enumerable: false, // 可枚举
         *    configurabel: true, // 可配置
         *    writable: true // 可些
         * }
         */
        descriptor.writable = false
        return descriptor
      }
      
      class Person {
        constructor() {
          this.first = 'A'
          this.last = 'B'
        }
      
        @readonly
        name() {
          return `${this.first} ${this.last}`
        }
      }
      
      let p = new Person()
      console.log(p.name())
      // 不能去修改name(), 否则会报错
      // p.name = function () {
      //   alert(100)
      // }
      ```

    - log日志

      ```js
      function log (target, name, descriptor){
        let oldValue = descriptor.value
        descriptor.value = function () {
          console.log(`calling ${name} width`, arguments)
          return oldValue.apply(this, arguments)
        }
        return descriptor
      }
      
      class Math {
        @log
        add(a, b) {
          return a + b
        }
      }
      
      let math = new Math()
      const result = math.add(2, 4)
      console.log('result', result)
      
      ```

    - target，name， descriptor参数

      ![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/target%2Cname%2Cdescriptor%E5%8F%82%E6%95%B0.png)

- core-decorators

  - 第三方开源lib

  - 提供常用的装饰器

    - 只读readonly

      ```js
      import { readonly } from 'core-decorators'
      
      class Person {
        @readonly
        name() {
          return 'zhang san'
        }
      }
      
      let p = new Person()
      alert(p.name())
      p.name() = function() {
        alert(111)
      }
      ```

    - deprecate

      ```js
      import { deprecate  } from 'core-decorators'
      
      class Person {
        @deprecate('即将废用', {url: 'www.imooc.com'})
        name() {
          return 'zhang san'
        }
      }
      
      let p = new Person()
      p.name()
      ```

      ![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/deprecate.png)

  - [查阅文档](https://github.com/jayphelps/core-decorators)

#### 4.4.4 设计原则验证

- 将现有对象和装饰器进行分离，两者独立存在
- 符合开放封闭原则

### 4.5 代理模式

#### 4.5.1 介绍

- 使用者无权访问目标对象
- 中间加代理，通过代理做授权和控制
- 示例：
  - 科学上网：github
  - 明星经纪人

#### 4.5.2 UML类图和示例代码

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.png)

```js
class RealImg {
  constructor(fileName) {
    this.fileName = fileName
    this.loadFromDisk() // 初始化即从硬盘中加载，模拟
  }
  display() {
    console.log('display...' + this.fileName)    
  }
  loadFromDisk() {
    console.log('loading...' + this.fileName)    
  }
}

class ProxyImg {
  constructor(fileName) {
    this.realImg = new RealImg(fileName)
  }
  display() {
    this.realImg.display()
  }
}

// 测试
let proxyImg = new ProxyImg('1.png')
proxyImg.display()
```

#### 4.5.3 场景

- 网页事件代理

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div id="div1">
      <a href="#">a1</a>
      <a href="#">a2</a>
      <a href="#">a3</a>
      <a href="#">a4</a>
      <a href="#">a5</a>
    </div>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      var div1 = document.getElementById('div1')
      div1.addEventListener('click', function(e) {
        var target = e.target
        if(target.nodeName === 'A') {
          alert(target.innerHTML)
        }
      })
    </script>
  </body>
  </html>
  ```

- jQuery $.proxy

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div id="div1">
      <a href="#">a1</a>
      <a href="#">a2</a>
      <a href="#">a3</a>
      <a href="#">a4</a>
      <a href="#">a5</a>
    </div>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      $('#div1').click(function () {
        var _this = this
        setTimeout(function() {
          $(_this).css('background-color', 'yellow')
        }, 1000)
      })
    </script>
  </body>
  </html>
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div id="div1">
      <a href="#">a1</a>
      <a href="#">a2</a>
      <a href="#">a3</a>
      <a href="#">a4</a>
      <a href="#">a5</a>
    </div>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      $('#div1').click(function () {
        var fn = $.proxy(function() {
          $(this).css('background-color', 'yellow')
        }, this)
        setTimeout(fn, 1000)
      })
    </script>
  </body>
  </html>
  ```

- ES6 Proxy

  ```js
  let star = {
    name: '张XX',
    age: 25,
    phone: 'star: 139000011111'
  }
  
  // 经纪人
  let agent = new Proxy(star, {
    get: function(target, key) {
      if (key === 'phone') {
        // 返回经纪人自己的电话
        return 'agent: 16899997777'
      }
      if (key === 'price') {
        // 明星不报价，经纪人报价
        return 120000
      }
      return target[key]
    },
    set: function(target, key, val) {
      if (key === 'customPrice') {
        if(val < 100000) {
          // 最低10w
          throw new Error('价格太低')
        } else {
          target[key] = val
          return true
        }
      }
    }
  })
  
  // 测试
  console.log(agent.name);
  console.log(agent.age);
  console.log(agent.phone);
  console.log(agent.price);
  agent.customPrice = 150000
  console.log(agent.customPrice);
  agent.customPrice = 90000
  console.log(agent.customPrice);
  ```

#### 4.5.4 设计原则验证

- 代理类和目标类分离，隔离开目标类和使用者
- 符合开放封闭原则

#### 4.5.5 代理模式 vs 适配器模式

- 适配器模式：提供一个不同的接口（如不同版本的插头）
- 代理模式：提供一模一样的接口

#### 4.5.6 代理模式 vs 装饰器模式

- 装饰器模式：扩展功能，原有功能不变且可直接使用
- 代理模式：显示原有功能，但是经过限制或者阉割之后的

### 4.6 外观模式

#### 4.6.1 介绍

- 为子系统中的一组接口提供了一个高层接口
- 使用者使用这个告辞接口
- 示例：
  - 去医院看病，接待员去挂号、门诊、划价、取药

#### 4.6.2 UML类图

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F.png)

#### 4.5.3 场景

```js
function bindEvent(elem, type, selector, fn) {
  if( fn === null) {
    fn = selector
    selector = null
  }

  // *******  
}

// 调用
bindEvent(elem, 'click', '#div1', fn)
bindEvent(elem, 'click', fn)
```



#### 4.5.4 设计原则验证

- 不符合单一职责原则和开放封闭原则，因此谨慎使用，不可滥用

### 4.7 观察者模式

#### 4.7.1 介绍

- 发布 & 订阅
- 一对多
- 示例：
  - 点咖啡，点好之后坐等被叫

#### 4.7.2 UML类图和示例代码

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F.png)

```js
// 主题，保存状态，状态变化之后触发所有观察者对象
class Subject {
  constructor() {
    this.state = 0
    this.observers = []
  }
  getState() {
    return this.state
  }
  setState(state) {
    this.state = state
    this.notifyAllObservers()
  }
  notifyAllObservers() {
    this.observers.forEach(observer => {
      observer.update()
    })
  }
  attach(observer) {
    this.observers.push(observer)
  }
}

// 观察者
class Observer {
  constructor(name, subject) {
    this.name = name
    this.subject = subject
    this.subject.attach(this)
  }
  update() {
    console.log(`${this.name} update, state: ${this.subject.getState()}`)
  }
}

// 测试
let s = new Subject()
let o1 = new Observer('o1', s)
let o2 = new Observer('o2', s)
let o3 = new Observer('o3', s)
s.setState(1)
s.setState(2)
s.setState(3)
```

#### 4.7.3 场景

- 网页事件绑定

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <button id="btn1">btn</button>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      $('#btn1').click(function (){
        console.log(1)      
      })
      $('#btn1').click(function (){
        console.log(2)      
      })
      $('#btn1').click(function (){
        console.log(3)      
      })
    </script>
  </body>
  </html>
  ```

- Promise

- jQuery callbacks

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      // 自定义事件，自定义回调
      var callbacks = $.Callbacks() // 注意大小写
      callbacks.add(function (info) {
        console.log('fn1', info)      
      })
      callbacks.add(function (info) {
        console.log('fn2', info)      
      })
      callbacks.add(function (info) {
        console.log('fn2', info)      
      })
      callbacks.fire('gogogo')
      callbacks.fire('fire')
    </script>
  </body>
  </html>
  ```

- nodejs 自定义事件

  - 简单示例

    ```js
    const EventEmitter = require('events').EventEmitter
    
    const emitter1 = new EventEmitter()
    // 监听 some 事件
    emitter1.on('some', info => {
      console.log('fn1', info)  
    })
    // 监听 some 事件
    emitter1.on('some', info => {
      console.log('fn2', info)  
    })
    // 触发 some 事件
    emitter1.emit('some', 'xxxxx')
    ```

  - 继承

    ```js
    const EventEmitter = require('events').EventEmitter
    
    // 继承
    class Dog extends EventEmitter {
      constructor(name) {
        super()
        this.name = name
      }
    }
    
    let simon = new Dog('simon')
    simon.on('bark', function() {
      console.log(this.name, ' barked-1')
    })
    simon.on('bark', function() {
      console.log(this.name, ' barked-2')
    })
    setInterval(function () {
      simon.emit('bark')
    }, 1000)
    ```

  - stream 用到自定义事件

    ```js
    // stream 用到自定义事件: 计算大文件有多少个字符
    const fs = require('fs')
    const readStream = fs.createReadStream('./nodejs/data/file1.txt')
    
    let length = 0
    readStream.on('data', function(chunk) {
      let len = chunk.toString().length
      console.log('len', len)  
      length += len
    })
    readStream.on('end', function() {
      console.log('length', length)
    })
    ```

    ```js
    // stream 用到自定义事件: 计算大文件行数
    const fs = require('fs')
    const readline = require('readline')
    
    let rl = readline.createInterface({
      input: fs.createReadStream('./nodejs/data/file1.txt')
    })
    
    let lineNum = 0
    rl.on('line', function(line) {
      lineNum++
    })
    rl.on('close', function() {
      console.log('lineNum', lineNum)  
    })
    ```

  - 其他场景

    - nodejs中： 处理http请求；多进程通讯

      ![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/inodejs-http%E8%AF%B7%E6%B1%82.png)

      ![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/nodejs%E5%A4%9A%E8%BF%9B%E7%A8%8B%E9%80%9A%E8%AE%AF.png)

    - vue和react组件生命周期触发

    - vue watch

#### 4.7.4 设计原则验证

- 主题和观察者分离，不是知道触发而是被动监听，两者解耦
- 符合开放封闭原则

### 4.8 迭代器模式

#### 4.8.1 介绍

- 顺序访问一个集合

- 使用者无需知道集合的内部结构（封装）

- 示例（jQuery）

  ```js
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div id="div1">
      <a href="#">a1</a>
      <a href="#">a2</a>
      <a href="#">a3</a>
      <a href="#">a4</a>
      <a href="#">a5</a>
    </div>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      var arr = [1, 2, 3]
      var nodeList = document.getElementsByTagName('a')
      var $a = $('a')
      function each(data) {
        var $data = $(data) // 生成迭代器
        $data.each(function(key, val) {
          console.log(key, val)        
        })
      }
      each(arr)
      each(nodeList)
      each($a)
      // 顺序遍历有序集合
      // 使用者不必知道集合的内部结构
      // // 遍历数组
      // arr.forEach(function(item) {
      //   console.log(item)      
      // })
      // // 遍历 nodeList
      // var i, length = nodeList.length
      // for(i=0; i<length; i++) {
      //   console.log(nodeList[i])      
      // }
      // // 遍历 $a
      // $a.each(function(key, elem) {
      //   console.log(key, elem)      
      // })
    </script>
  </body>
  </html>
  ```

#### 4.8.2 UML类图和示例代码

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8F.png)

```js
class Iterator{
  constructor(container) {
    this.list = container.list
    this.index = 0
  }
  next() {
    if(this.hasNext()) {
      return this.list[this.index++]
    }
    return null
  }
  hasNext() {
    if(this.index >= this.list.length) {
      return false
    }
    return true
  }
}

class Container {
  constructor(list) {
    this.list = list
  }
  // 生成遍历器
  getIterator() {
    return new Iterator(this)
  }
}

// 测试
var arr = [1, 2, 3, 4, 5, 6]
let container = new Container(arr)
let iterator = container.getIterator()
while(iterator.hasNext()) {
  console.log(iterator.next())  
}
```

#### 4.8.3 场景

- jQuery each

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>
  <body>
    <div id="div1">
      <a href="#">a1</a>
      <a href="#">a2</a>
      <a href="#">a3</a>
      <a href="#">a4</a>
      <a href="#">a5</a>
    </div>
    <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
    <script>
      var arr = [1, 2, 3]
      var nodeList = document.getElementsByTagName('a')
      var $a = $('a')
      function each(data) {
        var $data = $(data) // 生成迭代器
        $data.each(function(key, val) {
          console.log(key, val)        
        })
      }
      each(arr)
      each(nodeList)
      each($a)
    </script>
  </body>
  </html>
  ```

- ES6 Iterator

  - ES6 Iterator为何存在？

    - ES6语法中，有序集合的数据类型已经有很多
    - Array Map Set String TypedArray arguments NodeList
    - 需要有一个统一的遍历接口来遍历所有数据类型
    - （注意，object不是有序集合，可以用Map代替）

  - ES6 Iterator是什么？

    - 以上数据类型，都有[Symbol.iterator]属性

    - 属性值是函数，执行函数返回一个迭代器

    - 这个迭代器就有next方法可顺序迭代子元素

    - 可运行Array.prototype[Symbol.iterator]来测试

      ```js
      function each(data) {
        // 生成遍历器
        let iterator = data[Symbol.iterator]()
        // console.log(iterator.next()) // 有数据时返回 { value: 1. done: false}
        // console.log(iterator.next())
        // console.log(iterator.next())
        // console.log(iterator.next())
        // console.log(iterator.next()) // 没有数据时返回 { value: undefined. done: true}
        let item = {done:false}
        while(!item.done) {
          item = iterator.next()
          if(!item.done) {
            console.log(item.value)      
          }
        }
      }
      
      // 测试代码
      var arr = [1, 2, 3]
      let nodeList = document.getElementsByTagName('p')
      let m = new Map()
      m.set('a', 100)
      m.set('b', 200)
      each(arr)
      each(nodeList)
      each(m)
      ```

      ```js
      /**
       * Symbol.iterator并不是人人都知道
       * 也不是没个人都需要封装一个 each 方法
       * 因此有了 for...of 语法
       * @param {Iterator} data 可迭代对象
       */
      function each(data) {
        for (let item of data) {
          console.log(item)    
        }
      }
      
      // 测试代码
      var arr = [1, 2, 3]
      let nodeList = document.getElementsByTagName('p')
      let m = new Map()
      m.set('a', 100)
      m.set('b', 200)
      each(arr)
      each(nodeList)
      each(m)
      ```

  - ES6 Iterator 与 Generator

    - Iterator的价值不限于上述几个类型的遍历

    - 还有Generator函数的使用

    - 即只要返回的数据符合Iterator接口的要求

    - 即可使用Iterator语法，这就是迭代器模式

      ```js
      function* helloWorldGenerator() {
        yield 'hello';
        yield 'world';
        yield 'ending';
      }
      
      var hw = helloWorldGenerator();
      hw.next()
      hw.next()
      hw.next()
      hw.next()
      ```

      ```js
      function* foo() {
        yield 1;
        yield 2;
        yield 3;
        yield 4;
        yield 5;
        yield 6;
      }
      
      for (let v of foo()) {
        console.log(v)  
      }
      ```

      

#### 4.8.4 设计原则验证

- 迭代器对象和目标对象分离
- 迭代器将使用者与目标对象隔离开
- 符合开放封闭原则

### 4.9 状态模式

#### 4.9.1 介绍

- 一个对象有状态变化
- 每次状态变化都会触发一个逻辑
- 不能总是用 if..else 来控制
- 示例：
  - 交通信号灯不同颜色的变化

#### 4.9.2 UML类图和示例代码

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E7%8A%B6%E6%80%81%E6%A8%A1%E5%BC%8F.png)

```js
// 状态（红灯、黄灯、绿灯）
class State {
  constructor(color) {
    this.color = color
  }
  handle(context) {
    console.log(`turn to ${this.color} light`)
    context.setState(this)
  }
}

// 主体
class Context {
  constructor() {
    this.state = null
  }
  // 获取状态
  getState() {
    return this.state
  }
  // 设置状态
  setState(state) {
    this.state = state
  }
}

// 测试
let context = new Context()

let green = new State('green')
let yellow = new State('yellow')
let red = new State('red')

// 绿灯亮了
green.handle(context)
console.log(context.getState()) // 打印状态
// 黄灯亮了
yellow.handle(context)
console.log(context.getState()) // 打印状态
// 红灯亮了
red.handle(context)
console.log(context.getState()) // 打印状态
```

#### 4.9.3 场景

- 有限状态机

  - 有限个状态、以及在这些状态之间的变化

  - 如交通信号灯

  - 使用开源lib：javascript-state-machine

  - https://github.com/jakesgordon/javascript-state-machine

    ```js
    import StateMachine from "javascript-state-machine";
    import $ from "jquery";
    // 初始化状态机模型
    let fsm = new StateMachine({
      init: "收藏",
      transitions: [
        {
          name: "doStore",
          from: "收藏",
          to: "取消收藏",
        },
        {
          name: "deleteStore",
          from: "取消收藏",
          to: "收藏",
        },
      ],
      methods: {
        // 监听执行收藏
        onDoStore: function () {
          alert("收藏成功"); // 可以 post 请求
          updateText();
        },
        // 监听取消收藏
        onDeleteStore: function () {
          alert("已经取消收藏"); // 可以 post 请求
          updateText();
        },
      },
    });
    
    let $btn = $("#btn1");
    
    // 按钮点击事件
    $btn.click(function () {
      if (fsm.is("收藏")) {
        fsm.doStore()
      } else {
        fsm.deleteStore()
      }
    });
    
    // 更新按钮的文案
    function updateText() {
      $btn.text(fsm.state);
    }
    // 初始化文案
    updateText();
    
    ```

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>慕课网-双越老师-前端设计模式</title>
    </head>
    <body>
      <p>慕课网-双越老师-前端设计模式</p>
      <p>慕课网-双越老师-前端设计模式</p>
      <p>慕课网-双越老师-前端设计模式</p>
      <hr>
      <button id="btn1"></button>
    </body>
    </html>
    ```

    

- 写一个简单的Promise

  - 回顾Promise的语法

  - 分析：Promise就是一个有限状态机

    - Promise 三种状态：pending  fullfilled  rejected
    - pending -> fullfilled 或者 pending -> rejected
    - 不可逆向变化

  - 写代码

  - 测试

    ```js
    import StateMachine from "javascript-state-machine";
    
    // 状态机模型
    let fsm = new StateMachine({
      init: "pending", // 初始化状态
      transitions: [
        {
          name: "resolve", // 事件名称
          from: "pending",
          to: "fullfilled",
        },
        {
          name: "reject", // 事件名称
          from: "pending",
          to: "rejected",
        },
      ],
      methods: {
        // 监听 resolve
        onResolve: function (state, data) {
          // state - 当前状态机示例；data - fsm.resolve(xxx) 传递的参数
          data.successList.forEach((fn) => fn());
        },
        // 监听 reject
        onReject: function (state, data) {
          // state - 当前状态机示例；data - fsm.reject(xxx) 传递的参数
          data.failList.forEach((fn) => fn());
        },
      },
    });
    
    // 定义 Promise
    class MyPromise {
      constructor(fn) {
        this.successList = [];
        this.failList = [];
        const _this = this;
        fn(
          function () {
            // resolve 函数
            fsm.resolve(_this);
          },
          function () {
            // reject 函数
            fsm.reject(_this);
          }
        );
      }
      then(successFn, failFn) {
        this.successList.push(successFn);
        this.failList.push(failFn);
      }
    }
    
    // 测试代码
    function loadImg(src) {
      const promise = new MyPromise(function (resolve, reject) {
        let img = document.createElement("img");
        img.onload = function () {
          resolve(img);
        };
        img.onerror = function () {
          reject();
        };
        img.src = src;
      });
      return promise;
    }
    
    let src = "https://goss.veer.com/creative/vcg/veer/800water/veer-312472388.jpg";
    let result = loadImg(src);
    
    result.then(
      function () {
        console.log("ok1");
      },
      function () {
        console.log("fail1");
      }
    );
    
    result.then(
      function () {
        console.log("ok2");
      },
      function () {
        console.log("fail2");
      }
    );
    
    ```

#### 4.9.4 设计原则验证

- 将状态对象和主体对象分离，状态的辩护逻辑单独处理
- 符合开放封闭原则

### 4.10 其他设计模式

#### 4.10.1 优先级划分依据

- 不常用
- 对应不到经典的应用场景

#### 4.10.2 讲解方式

- 代码演示，说明该设计模式的目的和用意

#### 4.10.3 有哪些设计模式

- 创造型
  - 原型模式
- 结构型
  - 桥接模式
  - 组合模式
  - 享元模式
- 行为型
  - 策略模式
  - 模板方法模式
  - 职责链模式
  - 命令模式
  - 备忘录模式
  - 中介者模式
  - 访问者模式
  - 解释器模式

### 4.11 原型模式

#### 4.11.1 概念和用意

- clone 自己，生成一个新对象
- java 默认有 clone 接口，不用自己实现

#### 4.11.2 JS 中的应用

- Object.create

  ```js
  // 一个原型对象
  let prototype = {
    getName: function() {
      return this.first + ' ' + this.last
    },
    say: function () {
      alert('hello')
    }
  }
  
  // 基于原型创建 x
  let x = Object.create(prototype)
  x.first = 'A'
  x.last = 'B'
  alert(x.getName())
  x.say()
  
  // 基于原型创建 y
  let y = Object.create(prototype)
  y.first = 'C'
  y.last = 'D'
  alert(y.getName())
  y.say()
  ```

- 对比 JS 中的原型 prototype

  - prototype 可以理解为 ES6 class 的一种底层原理
  - 而 class 是实现面向对象的基础，并不是服务于某个模式
  - 若干年后 ES6 全面普及，大家可能会忽略掉 prototype
  - 但是 Object.create 却会长久存在

### 4.12 桥接模式

#### 4.12.1 概念

- 用于把抽象化与实现化解耦
- 使得二者可以独立变化
- （未找到 JS 中的经典应用）

#### 4.12.2 演示

- 不使用桥接模式

  ![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8F-%E7%94%BB%E5%9B%BE-1.png)

  ```js
  class ColorShape {
    yellowCircle() {
      console.log('yellow circle');
    }
    redCircle() {
      console.log('red circle');
    }
    yellowTriangle() {
      console.log('yellow triangle');
    }
    redTriangle() {
      console.log('red triangle');
    }
  }
  
  // 测试
  let cs = new ColorShape()
  cs.yellowCircle()
  cs.redCircle()
  cs.yellowTriangle()
  cs.redTriangle()
  ```

- 使用桥接模式

  ![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8F-%E7%94%BB%E5%9B%BE-2.png)

  ```js
  class Color {
    constructor(name) {
      this.name = name
    }
  }
  
  class Shape{
    constructor(name, color) {
      this.name = name
      this.color = color
    }
    draw() {
      console.log(`${this.color.name} ${this.name}`); 
    }
  }
  
  // 测试代码
  let red = new Color('red')
  let yellow = new Color('yellow')
  let circle = new Shape('circle', red)
  circle.draw()
  let triangle = new Shape('triangle', yellow)
  triangle.draw()
  ```

#### 4.12.3 设计原则验证

- 抽象和实现分离，解耦
- 符合开放封闭原则

### 4.13 组合模式

#### 4.13.1 概念

- 生成树形结构，表示 “整体-部分” 关系
- 让整体和部分都具有一致的操作方式

#### 4.13.2 演示

- JS 经典应用中，未找到这么复杂的数据类型

- 虚拟 DOM 中的 vnode 是这种形式，但数据类型简单

  ```html
  <div id="div1" class="container">
  	<p>123</p>
  	<p>456</p>
  </div>
  ```

  ```js
  {
    tag: 'div',
    attr: {
      id: 'div1',
      className: 'container'
    },
    children: [
      {
        tag: 'p',
        attr: {},
        children: ['123']
      },
      {
        tag: 'p',
        attr: {},
        children: ['123']
      }
    ]
  }
  ```

- （用 JS 实现一个菜单，不算经典应用，与业务相关）

- 整体和单个节点的操作是一致的

- 整体和单个节点的数据结构也保持一致

#### 4.13.4 设计原则验证

- 将整体和单个节点的操作抽象处理
- 符合开放封闭原则

### 4.14 享元模式

#### 4.14.1 概念

- 共享内存（主要考虑内存，而非效率）
- 相同的数据，共享使用
- （JS 中未找到经典应用场景）

#### 4.14.2 演示

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <!-- 无限下拉列表，将事件代理到高层节点上 -->
  <!-- 如果绑定到 a 标签，对内存开销太大 -->
  <div id="div1">
    <a href="#">a1</a>
    <a href="#">a2</a>
    <a href="#">a3</a>
    <a href="#">a4</a>
    <a href="#">a5</a>
    <!-- 无限下拉列表 -->
  </div>
  <script src="http://lib.sinaapp.com/js/jquery/3.1.0/jquery-3.1.0.js"></script>
  <script>
    var div1 = document.getElementById('div1')
    div1.addEventListener('click', function(e) {
      var target = e.target
      if(target.nodeName === 'A') {
        alert(target.innerHTML)
      }
    })
  </script>
</body>
</html>
```

#### 4.14.3 设计原则验证

- 将相同的部分抽象出来
- 符合开放封闭原则

### 4.15 策略模式

#### 4.15.1 概念

- 不同策略分开处理
- 避免出现大量 if...else 或者 switch...case
- （JS 中未找到经典应用场景）

#### 4.15.2 演示

- 不使用策略模式

```js
class User {
  constructor(type) {
    this.type = type
  }
  buy() {
    if (this.type === 'ordinary') {
      console.log('普通用户购买');
    } else if(this.type === 'member') {
      console.log('会员购买');      
    } else if(this.type === 'vip') {
      console.log('vip 用户购买');      
    }
  }
}

// 测试代码
var u1 = new User('ordinary')
u1.buy()
var u2 = new User('member')
u2.buy()
var u3 = new User('vip')
u3.buy()
```

- 使用策略模式

```js
class OrdinaryUser {
  buy() {
    console.log('普通用户购买');
  }
}

class MemberUser {
  buy() {
    console.log('会员用户购买');
  }
}

class VipUser {
  buy() {
    console.log('vip 用户购买');
  }
}

// 测试代码
var u1 = new OrdinaryUser()
u1.buy()
var u2 = new MemberUser()
u2.buy()
var u3 = new VipUser()
u3.buy()
```



#### 4.15.3 设计原则验证

- 不同策略，分开处理，而不是混合在一期
- 符合开放封闭原则

### 4.16 模板方法模式

```js
class Action {
  handle() {
    handle1();
    handle2();
    handle3();
  }
  handle1() {
    console.log(1);
  }
  handle2() {
    console.log(2);
  }
  handle3() {
    console.log(3);
  }
}

```



### 4.17 职责链模式

#### 4.17.1 概念

- 一步操作可能分为多个职责角色来完成
- 把这些角色分开，完后用一个链串起来
- 将发起者和各个处理者进行隔离

#### 4.17.2 演示

```js
class Action {
  constructor(name) {
    this.name = name
    this.nextAction = null
  }
  setNextAction(action) {
    this.nextAction = action
  }
  handle() {
    console.log(`${this.name} 审批`);
    if (this.nextAction !== null) {
      this.nextAction.handle()
    }
  }
}

// 测试代码
let a1 = new Action('组长')
let a2 = new Action('经理')
let a3 = new Action('总监')
a1.setNextAction(a2)
a2.setNextAction(a3)
a1.handle()
```

#### 4.17.3 JS 中的链式操作

- 职责链模式和业务结合较多，JS 中能联想到链式操作
- jQuery 的链式操作 Promise.then 的链式操作

#### 4.17.4 设计原则验证

- 发起者于各个处理者进行分离
- 符合开放封闭原则

### 4.18 命令模式

#### 4.18.1 概念

- 执行命令时，发布者和执行者分开
- 中间加入命令对象，作为中转站

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E5%91%BD%E4%BB%A4%E6%A8%A1%E5%BC%8F-%E6%A6%82%E5%BF%B5.png)

#### 4.18.2 演示

```js
// 接受者
class Receiver {
  exec() {
    console.log('执行');    
  }
}

// 命令者
class Command {
  constructor(receiver) {
    this.receiver = receiver
  }
  cmd() {
    console.log('执行命令');
    this.receiver.exec()    
  }
}

// 触发者
class Invoker {
  constructor(command) {
    this.command = command
  }
  invoke() {
    console.log('开始');
    this.command.cmd()    
  }
}

// 士兵
let soldier = new Receiver()
// 小号手
let trumpeter = new Command(soldier)
// 将军
let general = new Invoker(trumpeter)
general.invoke()

```

#### 4.18.3 JS 中的应用

- 网页富文本编辑器操作，浏览器封装了一个命令对象
- document.execCommand('bold')
- document.execCommand('undo')

#### 4.18.4 设计原则验证

- 命令对象与执行对象分开，解耦
- 符合开放封闭原则

### 4.19 备忘录模式

#### 4.19.1 概念

- 随时记录一个对象的状态变化
- 随时可以恢复之前的某个状态（如撤销功能）
- 未找到 JS 中的经典应用，除了一些工具（如编辑器）

#### 4.19.2 演示

```js
// 备忘对象
class Memento {
  constructor(content) {
    this.content = content
  }
  getContent() {
    return this.content
  }
}

// 备忘列表
class CareTaker {
  constructor() {
    this.list = []
  }
  add(memento) {
    this.list.push(memento)
  }
  get(index) {
    return this.list[index]
  }
}

// 编辑器
class Editor {
  constructor() {
    this.content = null
  }
  setContent(content) {
    this.content = content
  }
  getContent() {
    return this.content
  }
  saveContentToMemento() {
    return new Memento(this.content)
  }
  getContentFromMemento(memento) {
    this.content = memento.getContent()
  }
}

// 测试代码
let editor = new Editor()
let careTaker = new CareTaker()

editor.setContent('111')
editor.setContent('222')
careTaker.add(editor.saveContentToMemento()) // 当前内容备份
editor.setContent('333')
careTaker.add(editor.saveContentToMemento()) // 当前内容备份
editor.setContent('444')

console.log(editor.getContent());
editor.getContentFromMemento(careTaker.get(1)) // 撤销
console.log(editor.getContent());
editor.getContentFromMemento(careTaker.get(0)) // 撤销
console.log(editor.getContent());
```

#### 4.19.3 设计原则验证

- 状态对象与使用者分开，解耦
- 符合开放封闭原则

### 4.20 中介者模式

#### 4.20.1 概念

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F-%E6%A6%82%E5%BF%B5.png)

#### 4.20.2 演示

```js
class A {
  constructor() {
    this.number = 0;
  }
  setNumber(num, m) {
    this.number = num;
    if (m) {
      m.setB();
    }
  }
}

class B {
  constructor() {
    this.number = 0;
  }
  setNumber(num, m) {
    this.number = num;
    if (m) {
      m.setA();
    }
  }
}
// 中介者
class Mediator {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
  setB() {
    let number = this.a.number;
    this.b.setNumber(number * 100);
  }
  setA() {
    let number = this.b.number;
    this.a.setNumber(number / 100);
  }
}

// 测试
let a = new A();
let b = new B();
let m = new Mediator(a, b);

a.setNumber(100, m);
console.log(a.number, b.number);
b.setNumber(100, m);
console.log(a.number, b.number);

```

#### 4.20.3 设计原则验证

- 将各关联对象通过中介者隔离
- 符合开放封闭原则

### 4.21 访问者模式

- 将数据操作和数据结构进行分离
- 使用场景不多

### 4.22 解释器模式

- 描述语言语法如何定义，如何解释和编译
- 用于专业场景

## 5. 综合案例

### 5.1 用到的设计模式

- 工厂模式
- 单例模式
- 装饰器模式
- 观察者模式
- 状态模式
- 模板方法模式
- 代理模式

### 5.2 UML 类图

![](https://github.com/wanghuan19961020/design-pattern-cart/blob/master/images/%E7%BB%BC%E5%90%88%E6%A1%88%E4%BE%8B-UML%E7%B1%BB%E5%9B%BE.png)



### 5.3 源码地址

https://github.com/wanghuan19961020/design-pattern-cart

### 5.4 总结用到的设计模式

- 工厂模式：$('xxx')，创建商品
- 单例模式：购物车
- 装饰器模式：打点统计
- 观察者模式：网页事件，Promise
- 状态模式：添加到购物车 & 从购物车删除
- 模板方法模式：渲染有统一的方法，内部包含了各模块渲染
- 代理模式：打折商品信息处理
