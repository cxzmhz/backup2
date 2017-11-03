# Es6

### 1. let和const命令
1. let 命令
  - 用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。
  - 适用于for循环中要取到每一次循环的计数器i
  - 不存在变量提升
    * 它所声明的变量一定要在声明后使用，否则报错。
  - 暂时性死区:只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
  ```js
  var tmp = 123;
  if (true) {
  	tmp = 'abc'; // ReferenceError 报错
  	let tmp;
  }
  ```
  - 在相同的作用域内不允许重复声明
  - 块级作用域
2. const命令
  - const声明一个只读的常量。一旦声明，常量的值就不能改变。
  - 其他的与let一样

### 2. ES6 声明变量的六种方法
	- var ,function ,let ,const ,import , class

### 3. 字符串的扩展
1. includes(), startsWith(), endsWith()
  - includes():返回布尔值，表示是否找到了参数字符串。
  - startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
  - endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
2. 模板字符串
  - 模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
  ```js
  // 普通字符串
  `In JavaScript '\n' is a line-feed.`

  // 多行字符串
  `In JavaScript this is
   not legal.`

  console.log(`string text line 1
  string text line 2`);

  // 字符串中嵌入变量
  let name = "Bob", time = "today";
  `Hello ${name}, how are you ${time}?`
  ```

### 4. 数值的扩展
- ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。
- 这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。
```js
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```

### 5. 函数的扩展
1. ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。
```js
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```
2. 箭头函数
- ES6 允许使用“箭头”（=>）定义函数
```js
var f = v => v;

//上面的箭头函数等同于：

var f = function(v) {
  return v;
};
```
- 箭头函数的几个注意点：
  * 函数体内的this对象，就是定义时所在的对象（即定义时所处的上下文环境），而不是使用时所在的对象。this对象的指向是可变的，但是在箭头函数中，它是固定的。普通函数中的this，谁调用这个函数，this就指向谁
  ```js
  document.querySelector('button').addEventListener('click',function(){
    console.log(this);	//输出<button>clickMe</button>
  });

  document.querySelector('button').addEventListener('click',()=>{
    console.log(this);	//输出 window对象
  })
  ```
  * 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
  * 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

### 6. 对象的扩展
+ Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
```js
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

### 7. class的基本语法
+ 在构造函数上，ES6 提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。
```js
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);

//上面的代码用 ES6 的class改写，就是下面这样

//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
var b=new Point(1,2);
```
+ 可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。
+ 类的数据类型就是函数，类本身就指向构造函数。
+ 构造函数的prototype属性，在ES6的“类”上面继续存在。事实上，类的所有方法都定义在类的prototype属性上面,包括constructor方法
+ 类必须使用new调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用new也可以执行。
+ 类不存在变量提升（hoist），这一点与 ES5 完全不同。
+ Class的静态方法：static
	- 如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。
	```js
	class Foo {
	  static classMethod() {
	    return 'hello';
	  }
	}

	Foo.classMethod() // 'hello'

	var foo = new Foo();
	foo.classMethod()
	// TypeError: foo.classMethod is not a function
	```

### 7.1 Class的继承
+ Class 可以通过extends关键字实现继承
```js
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```
+ 上面代码中，constructor方法和toString方法之中，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。
+ super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B，因此super()在这里相当于A.prototype.constructor.call(this)。

### 8. Module的语法
+ 模块功能主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
```js
export var firstName = 'Michael';
//在另一个模块中用import
import fName from './fistName.js'
```

+ 一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用export关键字输出该变量。下面是一个JS文件，里面使用export命令输出变量。
```js
// profile.js
//另一种导出的写法 export {};
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```

+ 需要特别注意的是，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。
```js
// 报错
var m = 1;
export m;
// 正确写法一
export var m = 1;

// 正确写法二
var m = 1;
export {m};
```

+ export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
```js
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
//上面代码输出变量foo，值为bar，500毫秒之后变成baz。
```

+ export命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错

+ import 命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。如果想为输入的变量重新取一个名字，import命令要使用as关键字，将输入的变量重命名。
```js
import { lastName as surname } from './profile';
```

+ import命令具有提升效果，会提升到整个模块的头部，首先执行。因为import命令是编译阶段执行的，在代码运行之前。

+ 如果多次重复执行同一句import语句，那么只会执行一次，而不会执行多次。
```js
import { foo } from 'my_module';
import { bar } from 'my_module';
// 等同于
import { foo, bar } from 'my_module';
```

+ export default 命令
  - export default命令，为模块指定默认输出。一个模块只能有一个默认输出，因此export default命令只能使用一次。
  ```js
  // export-default.js
  export default {
  	a:1,
  	b:2
  }
  //上面代码是一个模块文件export-default.js，它的默认输出是一个对象。

  //其他模块加载该模块时，import命令可以为该对象指定任意名字。

  // import-default.js
  import customName from './export-default';
  console.log(customName.a); // 'foo'
  ```

+ 因为export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。
```js
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;

// 错误
export default var a = 1;
```

+ ES6 模块与 CommonJS 模块的差异
  - CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
    * 即：ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。
  - CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
    * 因为 CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

+ Es6的模块自动使用严格模式，严格模式有一下限制：
  - 变量必须声明后再使用
  - 函数的参数不能有同名属性，否则报错
  - 不能使用with语句
  - 不能对只读属性赋值，否则报错
  - 不能使用前缀0表示八进制数，否则报错
  - 不能删除不可删除的属性，否则报错
  - 不能删除变量delete prop，会报错，只能删除属性delete global[prop]
  - eval不会在它的外层作用域引入变量
  - eval和arguments不能被重新赋值
  - arguments不会自动反映函数参数的变化
  - 不能使用arguments.callee
  - 不能使用arguments.caller
  - 禁止this指向全局对象
  - 不能使用fn.caller和fn.arguments获取函数调用的堆栈
  - 增加了保留字（比如protected、static和interface）

### 9. Promise对象
+ Promise对象，可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。
+ 基本用法
	- Promise对象是一个构造函数，用来生成Promise实例。下面代码创造了一个Promis实例。
	```js
	var promise = new Promise(function(resolve, reject) {
      // ... some code

      if (/* 异步操作成功 */){
        resolve(value);
      } else {
        reject(error);
      }
    });
	```
	- Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
	- resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
	- reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
	- Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
	```js
	promise.then(function(value) {
      // success
    }, function(error) {
      // failure
    });
    //then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为	resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。
	```
+ Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
```js
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```
+ 上面代码中，getJSON是对 XMLHttpRequest 对象的封装，用于发出一个针对 JSON 数据的 HTTP 请求，并且返回一个Promise对象。需要注意的是，在getJSON内部，resolve函数和reject函数调用时，都带有参数。如果调用resolve函数和reject函数时带有参数，那么它们的参数会被传递给回调函数。reject函数的参数通常是Error对象的实例，表示抛出的错误；

### 9. async 函数
+ async函数返回一个 Promise 对象。
+ async函数内部return语句返回的值，会成为then方法回调函数的参数。
```js
async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"
```
+ async函数内部抛出错误，会导致返回的Promis对象变为reject状态。抛出的错误对象会被catch方法回调函数接收到。
```js
async function f() {
  throw new Error('出错了');
}

f().then(
  v => console.log(v),
  e => console.log(e)
)
// Error: 出错了
```
+ async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。
