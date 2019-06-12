# javascript-node

javascript 高级编程笔记

- IEEE 754 标准表示的浮点数误差，例如 0.1 + 0.2 != 0.3 但是为什么 0.1 + 0.25 = 0.35 呢? 怎么解决浮点数计算的精度问题？
- NaN 表示一个本来要返回数值的操作数未返回数值的情况（这样不会抛出错误）， 怎么理解，为什么要不抛出错误呢？ 这样做的目的是什么？ 怎么理解本来要返回数值的操作数未返回数值的情况？
  isNaN 的执行步骤
  1 应用在普通值，尝试将值转换成数值，转换不成数值的返回 true，否则返回 false。 例如 isNaN(true) === false
  2 应用在对象上，先尝试调用对象的 valueOf，是否可以转换成数值。如果不可以，基于这个返回值，调用 toString，再判断该值是否可以转换成数值

* parseFloat 直解析 10 进制，因此没有第二个进制参数。

* 字符串中\x 和\u 的区别？为什么要有\x 和\u
  \x 表示以 16 进制的 nn 表示一个字符（其中 n 为 0-F)，例如 \x41 表示 A
  \u 表示以 16 进制代码 nnnn 表示一个字符（其中 n 为 0-F）,例如 \u03a3 表示希腊字符 Σ

* 如果字符中包含双字节，那么 str.length 返回的值就有可能不正确，为什么？ 怎么会不正确？
* 宿主对象 BOM 和 DOM,不在规范的定义范围之内，所以 BOM 和 DOM 可以不继承 Object
* 非两个数字的时候，+的操作对象，数值或者布尔值的时候，调用他们的 toString()方法取得字符值。其中 null 和 undefinde 会取’null’和’undefinde’
* 对于-操作符来说特殊点：
  1 对于有一个操作数是字符，布尔值，null 或者 undefined, 现在后台调用 Number()函数将其转换成数值，如果转换的值为 NaN，结果就为 NaN
  2 对于对象来说，先调用 valueOf 方法获取该对象表示的数值，如果得到的是 NaN，则结果就为 NaN.如果对象没有 valueOf 则调用对象的 toString 方 法，并将其转换成数字的形式。
* 字符比较的特殊情况， ‘a’ < 3 由于，在字符串和数字进行比较的时候，会将字符串值转换成数字，此时’a’转换的值是 NaN，NaN 与任何值想比较都是 false,所以此时是 false. 并且 ‘a’ >= 3 也为 false
* do while 属于后测试循环语句，里面的代码至少被执行一次。 While 属于前测试循环语句，里面的代码有可能一次也不执行。
* With 的使用场景，是用来简化多次编写一个对象的工作。
  例如
  ```javascript
  var hostName = location.hostname;
  var url = location.href;
  可以改写成;
  with (location) {
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
  }
  ```
* Switch 语句需要注意的点：

1. 和其他语言不同，其他语言的 switch 语句只能传递数值,即 switch（数值）。但是 javascript 中可以传入任何值，字符串，数字，对象都可以
2. 并且 case 中跟的还可以是表达式。例如 case ‘hello’ + ‘world’
3. Swith 值与 case 中的表达式，相比较的时候，用的是===符号
4. 还有就是 switc 替代 for 循环的话，可以这么来写
   ```javascript
   var num = 25;
   switch (true) {
     case num < 0:
       alert('Less than 0.');
       break;
     case num >= 0 && num <= 10:
       alert('Between 0 and 10.');
       break;
     case num > 10 && num <= 20:
       alert('Between 10 and 20.');
       break;
     default:
       alert('More than 20.');
   }
   ```

- 函数的重载：可以为一个函数编写两个定义，只要这两个定义的签名（接受的参数类型和数量）不同即可。
- 函数签名指的是：参数类型，参数个数，参数顺序。
- 为什么说 catch 语句有自己的参数对象？
  当进入 catch 的执行环境的时候，会创建一个新的变量对象，表现在 catch(e)诸如了 e 参数，并且 catch 也不是一个函数。 此时 e 需要挂在到一个
  变量对象上，那么这个变量对象就是新创建的那个。而且 catch 语句是一个独立的作用域，就想函数那样。所以那才有自己的参数对象。
- 为什么使用 Array.apply(null, { length: 3 }) 来创建长度为 15 的数组？
  `Array.apply(null, { length: 3 }) 相当于 Array(undefined, undefined, undefined)`
- Array.prototype.sort 再没有指定回调函数的时候，比较的是字符串的大小。
  ```javascript
  values.sort();
  alert(values); //0,1,10,15,5
  ```
- 链接两个数组并且不用重新返回副本的方法
  原始方法。
  ```javascript
  var a = [];
  var b = [1, 2, 3];
  a = a.concat[b];
  ```
  这样需要创建一个副本，并且需要重新赋值
  简化的方法
  ```javascript
  Array.prototype.push.apply(a, b);
  ```
  这样不需要重新创建副本，并且不需要重新赋值的代码，简洁明了，vue 代码中很常见。
- 怎么理解数组的 slice 传入负数： 如果 slice 的参数传入的是一个负数，那么位置可以有数组的长度加上该负数来确定。例如
  ```javascript
  var a = [1, 2, 3, 4];
  var b = a.slice(1, -1);
  b = [2, 3];
  ```
- reduce 和 reduceRight 叫归并方法。怎么理解归并，reduceRight 很少见也很少用。
- 为什么 Date 对象支持的传递方式？  
   没有参数时：不解释，当前时间。  
   只有一个参数的时:  
   传入的是本地表示的时间，本地表示的时间格式可以通过 `var d = new Date() d.toLocaleString()`来获取
  例如 `var d = new Date(‘2019/2/08 14:00’)` 其实会调用 Date.parse，相当于 new Date(Date.parse(‘2019/2/08 14:00’))  
   当有两个以上参数时：  
   `Date ( year, month [ , date [ , hours [ , minutes [ , seconds [ , ms ] ] ] ] ] )`

      	Date的valueOf返回的是Date的毫秒表示，这也就是为什么可以直接比较两个时间的大小
      	`var d = new Date(2017, 2, 8). var d1 = new Date(2018,2,9).  d1 > d // true`

- 对于正则表达式中的静态属性的理解：
  RegExp 上挂载的静态属性有 input,lastMatch,lastParen,leftContent,rightContent。这些属性是根据上一次的正则结果挂载的。不明白为什么要挂在静态属性上???何用

  ```javascript
  var reg = /\s*((\w*)\s*="(\w*)")\s*/;
  const str = 'hello name="zhouxiaojie" world';
  const res = reg.exec(str);
  RegExp.input; // hello name="zhouxiaojie" world 输入的字符串
  RegExp.lastMath; // name="zhouxiaojie"  上次匹配的字符串，注意这里有匹配的空格
  RegExp.lastParen; // zhouxiaojie 最后匹配的分组
  RegExp.leftContent; // hello 匹配模式左边的内容
  RegExp.rightContent; // world 匹配模式右边的内容
  // 另外可以通过RegExp.$1....$9 获取分组内容
  ```

- 这里提到了不支持 unicode, `ES6应该提供了支持`要看一下是怎么支持的？
- 函数 caller 属性值，指向了调用它的函数，利用这个可以做一下报错堆栈
  ```javascript
  function a() {
    console.log('a caller', a.caller);
  }
  function b() {
    a();
  }
  b(); // function b();
  ```
  - 为什么不能为基本类型添加相应的属性值，例如
  ```javascript
  var str = 'hello world';
  str.name = 'zhou'; // 实际上访问 str.name的时候，javascript会创建一个包装对象，但是执行完这一行代码之后随即销毁
  console.log(str.name); // undefined 再次访问str.name的时候，又会新创建一个包装对象，此时的包装对象是没有.name这个属性的
  ```
- toFixed 返回的是数字的字符串表示，之前一直以为返回的是数字,并且它不光是指定后面的小数位，而且还有四舍五入的规则，例如
  ```javascript
  var a = '1.5';
  a.toFixed(0); // 2
  ```
- 为什么要使用 charAt 来获取某个位置的字符？用[index]的方式也可以访问
  在 IE7 中，使用[index]访问，会返回 undefined，ie 有兼容性问题，另外[index]访问超过字符长度的索引，返回的是 undefined. charAt 返回的是空字符串。
- subString, subStr, slice 的区别：  
  其中 subStr 未来将被移除，被 substring 替代。主要看 subString 和 slice 的区别:
  ```javascript
    var str = 'hello world';
    str.slice(3, -1) // lo world
    str.substring(3， -1) // hel subString会把-1转换成0,并且subString(3, -1) 相当于 subString(0, 3)
  ```
- 通过 indexOf 查找字符串中所有符合规则的子字符串
  ```javascript
  var str = 'hello world  i am teacher hello';
  var tem = 'ell';
  var posSet = [];
  var pos = str.indexOf(tem);
  while (pos > -1) {
    posSet.push(pos);
    pos = str.indexOf(tem, pos + 1);
  }
  posSet; // [1, 27]
  ```
- toLocaleLowerCase 和 toLocaleUpperCase 一般不知到程序要运行在那种语言环境的情况下最好是使用这两个方法
- 字符串的 math 本质上和正则表达是的 exec 方法是相同的。
  — 字符串的 search 方法，返回第一个匹配模式的位置，例如
  ```javascript
  var str = 'hello world';
  str.search(/\s/); // 5
  ```
- 对于 str.replace 中特殊字符的理解

```javascript
var str = '<div id="name" class="wrapper">hello world</div>';
const reg = /(\w*)="(\w*)"/g;
str.replace(
  reg,
  "匹配整个模式的字符串:$& 匹配字符串的左边内容:$' 匹配字符串的右边内容:$` $属性名:$1, 属性值:$2"
);
```

- 对于 str.replace 中第二参数传递函数的理解：  
   第二个参数是函数时，函数被注入的参数：
  1. 模式匹配项，模式匹配项在字符串中位置，原始字符串
  2. 当有分组的时候，参数依此是，匹配的字符串，第一个捕获的分组，第二个捕获的分组.....最后两个参数仍然是模式匹配在字符串中位置，原始字符串
  ```javascript
  var str = '<div id="name" class="wrapper">hello world</div>';
  const reg = /(\w*)="(\w*)"/g;
  str.replace(reg, (match, $1, $2, pos, source) => {
    // match id="name" class="wrapper"
    // $1 id class
    // $2 name wrapper
    // pos 模式匹配的字符在字符串中的位置
    // source 原始字符串
    console.log('match', match);
    console.log('$1', $1);
    console.log('$2', $2);
    console.log('pos', pos);
    console.log('source', source);
    return `属性名:${$1},属性值:${$2}`;
  });
  ```
- split 方法第一个参数不光可以指定字符，还可以指定正则表达式,第二个参数可以用来限定数组的长度。

```javascript
var colors = 'red,blue,green,yellow';
colors.split(',', 2); // ['red', 'blue']
colors.split(/[^,]+/); // ["", ",", ",", ",", ""] 怎么理解前面的""和后面的""
// 假设colors= ',red,blue,green,yellow,'
// colors.split(',') = ["", "red", "blue", "green", "yellow", ""]
```

- localeCompare 方法是比较字母在字母表中的顺序，并且有本地化

```javascript
'yellow'.localeCompare('black'); // -1
'balck'.localeCompare('yellow'); // 1
'yellow'.localeCompare('yellow'); // 0
```

- String.fromCharCode 接受一个或者多个字符编码，把他们转换成字符串

```javascript
String.fromCharCode(104, 101); // he
```

- 对于 encodeURI 和 encodeURIComponent 方法的理解：  
  ecodeURI 和 encodeURIComponent 可以对 Uri 进行编码，以便发送给浏览器，有效的 Uri 不能包含某些特殊字符，比如空格。而这两个方法是用来给 Uri 进行编码，他们使用特殊的 utf-8 来替换无效的
  字符，从而让浏览器能够接受和理解。
  其中 ecodeURI 主要是用于整个 url, ecodeURIcomponent 主要用于部分 url。ecodeURI 不会对属于 Url 特殊字符进行编码，如：/ + # & ?等，而 ecodeURIcomponent 会对所有的特殊字符进行编码例如

  ```javascript
  var uri = 'http://www.wrox.com/illegal value.htm#start';
  encodeURI(uri); //"http://www.wrox.com/illegal%20value.htm#start" 只对空格做了编码
  encodeURIComponent(uri); //"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start" 对所有的特殊字符进行了编码
  ```

  对应相应的解码 `decodeURI` `decode- URIComponent`
  其中`escape`和`unescape`已经被废弃

- 在 eval 中创建的变量不会提升。
- 在 ECMAScript 中，没有指出如何访问 Global 对象，但是浏览器都是把它作为 window 一部分来实现的。所以浏览器下的 Global 是 window 对象的一个子集。因为 window 不仅包含规范内定义的内容，来 还包含了宿主浏览器的一些内容。
- 舍入方法。  
  `Math.floor` 向下取证 `Math.ceil` 向上取整 `Math.round`标准的四舍五入
- 对于 Math.random 的理解:  
  Math.random 产生的一个`大于等于0` `小于1` 的数，所以要想产生一个闭范围的值。例如
  ```javascript
  function getRandomRange(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min; // 注意这里max - min + 1 开范围的不加1即可
  }
  ```
- 在使用Object.defineProperty定义属性描述的时候，configurable, configurable, wirteable默认为false
- 在没有Object.defineProperty 可以设置setter getter方法之前，浏览器使用了两个非标准方法__defineGetter__和__defineSetter__这也就是为什么在chrome终端打印对象的时候，会   有这两个方法。使用旧访问器代码如下：
  ``` javascript
  var o = {
      name: 'hello'
  }
  o.__defineGetter__('hello', () => {
      return this.name + 'getter'
  })
  o.__defineSetter__('hello', (value) => {
      this.name = value;
  })
  ```

- `Object.getOwnPropertyDescriptor()`只能获取实例属性的描述
- `Object.getOwnPropertyNames` 获取所有的属性名称，包括不可枚举的
- instanceOf操作符的理解，只要是实例的原型链上包含f.prototype,那么 instance instanceOf f就会返回true.