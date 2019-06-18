# 继承

- 原型链继承  
  缺点：子类继承无法调用父类的构造函数，导致子类继承的父类的属性是不变的

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.sayName = function() {
  console.log('name', this.name);
};

function Teacher(desc) {
  this.desc = desc;
}

Teacher.prototype = new Person('zhou', 18);
Teacher.prototype.constructor = Teacher;
Teacher.prototype.sayDesc = function() {
  console.log('desc', this.desc);
};

var teacher = new Teacher('teacher');
```

- 借用构造函数继承(解决了原型链继承无法调用父类构造函数的缺点)  
  缺点：无法实现函数继承复用
  例如：

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

function Teacher(name, age, desc) {
  Person.call(this, name, age);
  this.desc = desc;
}
```

- 组合继承（解决了借用构造函数继承无法实现函数继承复用的问题）
  缺点：调用了两次父类的构造函数，导致继承时父类中有多余的属性

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.sayName = function() {
  console.log('name', this.name);
};

function Teacher(name, age, desc) {
  Person.call(this, name, age); // 调用一次
  this.desc = desc;
}

Teacher.prototype = new Person('zhou', 18); // 调用第二次
Teacher.prototype.constructor = Teacher;
Teacher.prototype.sayDesc = function() {
  console.log('desc', this.desc);
};
```

- 寄生式继承(不知道有什么用，用来增强对象?)  
  例如:

```javascript
// create其实就相当于Object.create了, es5, es6新增的方法，很多都是因为之前的实践，比如Object.create方法
function create(parent) {
  var f = function() {};
  f.prototype = parent;
  return new f();
}

const obj = {
  name: 'zhou',
  sayName() {
    console.log('name', this.name);
  }
};

function extendObj(obj) {
  var o = create(obj);
  o.age = 18;
  o.sayAge = function() {
    console.log('age', this.age);
  };
}

var obj1 = extendObj(obj); // 这样就可以实现复用obj的内容，从而创建一个新对象

// 其实根据javascript本身的设计来说,复用的应该都只是方法而已，例如Object对象的原型上挂在的也都只是一些方法。所以适用于js
// 的继承方式应该是这样的

var proto = {
  toString() {},
  valueOf() {}
};

function Geno(name) {
  this.name = name;
}

Geno.prototype = proto;
Gen.prototype.constructor = Geno;
```

- 寄生组合式继承(解决组合继承中调用两次超类的构造函数，其实是多实例化了一次父类),其实想想 es6 中 extends 的实现，应该也就是这样的。需要注意的是：类声明没有变量提升。
  例如:

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.desc = 'i am a teacher';
}

Person.prototype = {
  constructor: Person,
  sayName() {
    console.log('name', this.name);
  }
};

function Teacher(name, age) {
  Person.call(this, name, age);
}

Teacher.ptototype = Object.create(Person.prototype);
Teacher.prototype.constructor = Teacher;
```
