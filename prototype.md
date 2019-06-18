# 面向对象的程序设计

- 工厂模式(设计模式可以借鉴，但是这样创建对象的方式确实没必要了）  
  解决的问题： `使用对象自变量或者使用new Object重复创建对象，代码冗余。使用工厂模式可以简化代码`  
  例如

```javascript
 var o = {
     name: 'zhou',
     age: 17,
     desc: 'i am one person'
     sayName () {
         console.log('name', this.name)
     }
 }
 // 如果在项目中有重复创建这样对象的需求，相同属性，只不过属性值不相同，那么这样的方式必定会有重复的代码。使用工厂模式封装创建对象的逻辑，可以避免这样的问题
 funciton geno (name, age, desc) {
     var o = new Object();
     o.name = name;
     o.age = age;
     o.desc = desc;
     o.sayName = function () {
         console.log('name', this.name)
     }
     return o;
 }
 // 这样可以方便的创建对象
 var o1 = geno('zhou', '18', 'teacher');
 var o1 = geno('wang', '18', 'student');
```

- 构造函数模式(现在比较常见，并且主流的创建对象的方式，推荐)  
  缺点：`sayName`函数在每次实例化对象的时候都会重新创建一次

```javascript
function Teacher(name, desc, age) {
  this.name = name;
  this.desc = desc;
  this.age = age;
  this.sayName = function() {
    console.log('name', this.name);
  };
}

var t = new Teacher('zhou', 'teacher', 20);

// 执行构造函数的过程相当于
var o = {};
Teacher.call(o, name, desc, age);
return o;
```

- 构造函数+原型模式（解决构造函数模式中，方法会在每次实例化重新创建的问题)  
  例如:

```javascript
function Teacher(name, desc, age) {
  this.name = name;
  this.desc = desc;
  this.age = age;
}
Teacher.prototype = {
  constructor: Teacher,
  sayName() {
    console.log('name', this.name);
  }
};
var t = new Teacher('zhou', 'teacher', 18);
var t1 = new Teacher('wang', 'student', 20);
```

- 动态原型模式（解决构造函数+原型模式中设置原型的代码出现在构造函数的外部，破坏了封装性的问题)  
  例如：

```javascript
function Teacher(name, desc, age) {
  this.name = name;
  this.desc = desc;
  this.age = age;
  // 这里判断，是为了不重复执行原型挂载
  if (typeof this.sayName !== 'function') {
    // 这里不可以使用对象自变量的形式书写原型，否则会修改原型引用，导致第一次创建的对象获取的原型不正确
    // 原因 将对象内部的o[[prototype]]赋值成Teacher的原型的执行要在 Teacher.call()之前
    // 过程可能是这样的：
    // var o = new Object();
    // o[[prototype]] = Teacher.prototype
    // Teacher.call(o, name, desc, age)
    // return o;
    Teacher.prototype.sayName = function() {
      console.log('name', this.name);
    };
  }
}

var t = new Teacher('zhou', 'teacher', 18);
var t1 = new Teacher('wang', 'student', 20);
```

- 寄生构造函数模式(实在不知道有什么用, 和工厂模式是一样的代码，只不过在使用的时候，需要加上 new 操作符)  
  例如:

```javascript
funciton geno (name, age, desc) {
     var o = new Object();
     o.name = name;
     o.age = age;
     o.desc = desc;
     o.sayName = function () {
         console.log('name', this.name)
     }
     return o;
 }
 // 这样可以方便的创建对象
 var o1 = geno('zhou', '18', 'teacher');
 var o1 = geno('wang', '18', 'student');
```

- 稳妥构造模式（实际上就是通过闭包创建一个私有属性，只能通过固定的方法获取)  
  例如：

```javascript
function Teacher(name, age, desc) {
  return {
    getName() {
      return name;
    },
    getAge() {
      return age;
    },
    getDesc() {
      return desc;
    }
  };
}
```
