# JavaScript Style

> eslint-config-ecomfe

## Overview

- [**JavaScript Style**](#javascript-style)
  - [Overview](#overview)
  - [可能的错误或逻辑错误](#可能的错误或逻辑错误)
  - [最佳实践](#最佳实践)
  - [严格模式](#严格模式)
  - [变量](#变量)
  - [Node.js 和 CommonJS](#nodejs-和-commonjs)
  - [代码风格](#代码风格)
  - [ECMAScript 6](#ecmascript-6)
  - [Vue](#vue)

## Rules

### 可能的错误或逻辑错误

- **禁止方向错误的 for 循环**

  eslint: [for-direction](http://eslint.cn/docs/rules/for-direction)

  ```js
  // ✓ ok
  for (var i = 0; i < 10; i++) { ... }
  
  // ✗ avoid
  for (var i = 0; i < 10; i--) { ... }
  
  for (var i = 10; i >= 0; i++) { ... }
  ```

- **getter 必须有返回值，并且禁止返回空**

    eslint: [getter-return](http://eslint.cn/docs/rules/getter-return)

    ```js
    // ✓ ok
    p = {
        get name(){
            return "nicholas";
        }
    };
    
    Object.defineProperty(p, "age", {
        get: function (){
            return 18;
        }
    });
    
    class P{
        get name(){
            return "nicholas";
        }
    }
    
    // ✗ avoid
    p = {
        get name(){
            // no returns.
        }
    };
    
    Object.defineProperty(p, "age", {
        get: function (){
            // no returns.
        }
    });
    
    class P {
        get name(){
            // no returns.
        }
    }
    ```

- **允许在循环里使用await**

    eslint: [no-await-in-loop](http://eslint.cn/docs/rules/no-await-in-loop)

    > reason: 要求太严格了，有时需要在循环中写 await

    ```js
    // ✓ ok
    async function foo(things) {
      const results = [];
      for (const thing of things) {
        results.push(await bar(thing));
      }
      return baz(results);
    }
    ```

- **禁止与负零进行比较**

    eslint: [no-compare-neg-zero](http://eslint.cn/docs/rules/no-compare-neg-zero)

    ```js
    // ✓ ok
    if (x === 0) { ... }
    
    if (Object.is(x, -0)) { ... }
    
    // ✗ avoid
    if (x === -0) { ... }
    ```

- **禁止条件语句中出现赋值语句**

    eslint: [no-cond-assign](http://eslint.cn/docs/rules/no-cond-assign)

    ```js
    // ✓ ok
    if (x === 0) { ... }
    
    // ✗ avoid
    if (x = 0) { ... }
    
    while (x = 0) { ... }
    ```

- **允许使用 console**

    eslint: [no-console](http://eslint.cn/docs/rules/no-console)

    > reason: 脚手架保障build过程中去除 console

    ```js
    // ✓ ok
    console.log("Log a debug level message.");
    ```

-   **禁止将常量作为分支条件判断中的测试表达式，但允许作为循环条件判断中的测试表达式**

    eslint: [no-constant-condition](http://eslint.cn/docs/rules/no-constant-condition)

    ```js
    // ✓ ok
    if (x === 0) { ... }
    
    while (true) { ... }
    
    // ✗ avoid
    if (false) { ... }
    ```

- **允许在正则表达式中使用控制字符**

  > 在 ASCII 中，0-31 范围内的控制字符是特殊的、不可见的字符。
  >
  > ！控制字符并不是 Ctrl 键的ASCII 表示，咱们 eslintrc.js 里的相应注释应该是理解错误。

  eslint: [no-control-regex](http://eslint.cn/docs/rules/no-control-regex)

  > reason: 几乎不会遇到这种场景。

  ```js
  // ✓ ok
  var pattern1 = /\x1f/;
  var pattern2 = new RegExp("\x1f");
  ```

- **允许使用 debugger**

  eslint: [no-debugger](http://eslint.cn/docs/rules/no-debugger)

  > reason: 脚手架保障build过程中去除。

  ```js
  // ✓ ok
  function isTruthy(x) {
      debugger;
      return Boolean(x);
  }
  ```

- **禁止在 `function` 定义中出现重复的参数**

  eslint: [no-dupe-args](http://eslint.cn/docs/rules/no-dupe-args)

  ```js
  // ✓ ok
  function foo(a, b, c) {
      console.log(a, b, c);
  }
  
  var bar = function (a, b, c) {
      console.log(a, b, c);
  };
  
  // ✗ avoid
  function foo(a, b, a) {
      console.log("value of the second a:", a);
  }
  
  var bar = function (a, b, a) {
      console.log("value of the second a:", a);
  };
  ```

- **禁止在对象字面量中出现重复的键名**

  [no-dupe-keys](http://eslint.cn/docs/rules/no-dupe-keys)

  ```js
  // ✓ ok
  var foo = {
      bar: "baz",
      quxx: "qux"
  };
  
  // ✗ avoid
  var foo = {
      bar: "baz",
      bar: "qux"
  };
  
  var foo = {
      "bar": "baz",
      bar: "qux"
  };
  ```
  
- **禁止在 switch 语句中出现重复测试表达式的 case**

  eslint: [`no-duplicate-case`](http://eslint.cn/docs/rules/no-duplicate-case)

  ```js
  switch (id) {
      case 1:
      // ...
      case 1:   // ✗ avoid
  }
  ```
  
- **禁止出现空代码块，允许 catch 为空代码块**

  eslint: [no-empty](http://eslint.cn/docs/rules/no-empty)

  ```js
  // ✓ ok
  if (foo) {
  	...
  }
  
  while (foo) {
  	...
  }
    
  // ✗ avoid
  if (foo) {  }
  
  while (foo) {  }
  
  switch(foo) {  }
  ```

- **禁止在正则表达式中使用空的字符集 []**

  eslint: [no-empty-character-class](http://eslint.cn/docs/rules/no-empty-character-class)

  ```js
  // ✓ ok
  /^abc[a-z]/.test("abcdefg"); // true
  "abcdefg".match(/^abc[a-z]/); // ["abcd"]
    
  // ✗ avoid
  /^abc[]/.test("abcdefg"); // false
  "abcdefg".match(/^abc[]/); // null
  ```

- **禁止对 `catch` 子句的参数重新赋值**

  eslint: [ no-ex-assign](http://eslint.cn/docs/rules/no-ex-assign)

  ```js
  // ✓ ok
  try {
      // ...
  } catch (e) {
      const newVal = 'new value';
  }
  
  // ✗ avoid
  try {
      // ...
  } catch (e) {
      e = 'new value';
  }
  ```

- **禁止不必要的布尔转换**

  eslint: [ no-extra-boolean-cast](http://eslint.cn/docs/rules/no-extra-boolean-cast)

  ```js
  // ✓ ok
  const result = true
  if (result) { ... }
  
  // ✗ avoid
  const result = true
  if (!!result) { ... }
  ```

- **禁止将一个函数声明重新赋值**

  eslint: [no-func-assign](http://eslint.cn/docs/rules/no-func-assign)

  ```js
  // ✓ ok 
  var foo = function () {}
  foo = bar;
  
  function foo(foo) {
      foo = bar;
  }
  
  function foo() {
      var foo = bar;
  }
  
  // ✗ avoid
  function foo() {}
  foo = bar;
  
  function foo() {
      foo = bar;
  }
  ```

- **禁止在嵌套的块中出现var变量声明或函数声明**

  eslint: [no-inner-declarations](http://eslint.cn/docs/rules/no-inner-declarations)

  ```js
  // ✓ ok 
  function doSomething() { }
  
  if (foo) {
      let bar1;
    	const hello = 'world';
  }
  
  // ✗ avoid
  if (test) {
      function doSomethingElse () { }
  }
  
  while (test) {
      var bar2;
  }
  ```

- **禁止在 RegExp 构造函数中出现非法的正则表达式**

  eslint: [no-invalid-regexp](http://eslint.cn/docs/rules/no-invalid-regexp)

  ```js
  RegExp('[a-z]');    // ✓ ok
  RegExp('[a-z');     // ✗ avoid
  ```

- **禁止使用特殊空白符（比如全角空格），除非是出现在字符串、正则表达式或模版字符串中**

  eslint: [`no-irregular-whitespace`](http://eslint.cn/docs/rules/no-irregular-whitespace)

  ```js
  str = ' <NBSP>thing';							// ✓ ok
  function myFunc() /*<NBSP>*/{}   	// ✗ avoid
  ```

- **禁止将 Math, JSON 或 Reflect 直接作为函数调用**

  eslint: [ no-obj-calls](http://eslint.cn/docs/rules/no-obj-calls)

  ```js
  // ✓ ok
  function area(r) {
      return Math.PI * r * r;
  }
  var object = JSON.parse("{}");
  var value = Reflect.get({ x: 1, y: 2 }, "x");
  
  // ✗ avoid
  var math = Math();
  var json = JSON();
  var reflect = Reflect();
  ```

- **禁止直接使用 hasOwnProperty, isPrototypeOf 或 propertyIsEnumerable**

  eslint: [no-prototype-builtins](http://eslint.cn/docs/rules/no-prototype-builtins)

  > 该规则原意是禁止**在直接对象实例上**调用 `Object.prototype` 的一些方法。咱们eslintrc.js文件里的配置及相关注释应该是理解错误，应修改对应配置项。
  >
  
  ```js
    // ✓ ok
    var hasBarProperty = Object.prototype.hasOwnProperty.call(foo, "bar");
    
    var isPrototypeOfBar = Object.prototype.isPrototypeOf.call(foo, bar);
    
    var barIsEnumerable = {}.propertyIsEnumerable.call(foo, "bar");
    
    // ✗ avoid
    var hasBarProperty = foo.hasOwnProperty("bar");
    
    var isPrototypeOfBar = foo.isPrototypeOf(bar);
    
    var barIsEnumerable = foo.propertyIsEnumerable("bar");
  ```
  
- **禁止正则表达式字面量中出现多个空格**

  eslint: [no-regex-spaces](http://eslint.cn/docs/rules/no-regex-spaces)

  ```js
  // ✓ ok
  var re = /foo {3}bar/;
  var re = new RegExp("foo {3}bar");
  
  // ✗ avoid
  var re = /foo   bar/;
  var re = new RegExp("foo   bar");
  ```

- **禁止在数组中出现连续的逗号**

  eslint: [ no-sparse-arrays](http://eslint.cn/docs/rules/no-sparse-arrays)

  ```js
  // ✓ ok
  var items = [];
  var items = new Array(23);
  var colors = [ "red", "blue", ];
  
  // ✗ avoid
  var items = [,];
  var colors = [ "red",, "blue" ];
  ```

- **禁止在普通字符串中出现模版字符串里的变量形式**

  eslint: [no-template-curly-in-string](http://eslint.cn/docs/rules/no-template-curly-in-string)

  ```js
  // ✓ ok
  `Hello ${name}!`;
  `Time: ${12 * 60 * 60 * 1000}`;
  
  templateFunction`Hello ${name}`;
  
  // ✗ avoid
  "Hello ${name}!";
  'Hello ${name}!';
  "Time: ${12 * 60 * 60 * 1000}";
  ```

- **禁止在 return, throw, break 或 continue 之后还有代码**

  eslint: [no-unreachable](http://eslint.cn/docs/rules/no-unreachable)

  ```js
  // ✗ avoid
  function foo() {
      return true;
      console.log("done");
  }
  
  function bar() {
      throw new Error("Oops!");
      console.log("done");
  }
  
  while(value) {
      break;
      console.log("done");
  }
  
  throw new Error("Oops!");
  console.log("done");
  
  function baz() {
      if (Math.random() < 0.5) {
          return;
      } else {
          throw new Error();
      }
      console.log("done");
  }
  
  for (;;) {}
  console.log("done");
  ```

- **禁止在 finally 中出现 return, throw, break 或 continue**

  eslint: [ no-unsafe-finally](http://eslint.cn/docs/rules/no-unsafe-finally)

  > finally 中的语句会在 try 之前执行。

  ```js
  // ✓ ok
  let foo = function() {
      try {
          return 1;
      } catch(err) {
          return 2;
      } finally {
          console.log("hola!");
      }
  };
  
  // ✗ avoid
  let foo = function() {
      try {
          return 1;
      } catch(err) {
          return 2;
      } finally {
          return 3;
      }
  };
  ```

- **禁止在 in 或 instanceof 操作符的左侧变量前使用感叹号**

  eslint: [ no-unsafe-negation](http://eslint.cn/docs/rules/no-unsafe-negation)

  ```js
  if (!key in obj) { ... } // ✗ avoid
  ```

- **必须**使用 isNaN(foo) 而不是 foo === NaN。

    eslint: [ use-isnan](http://eslint.cn/docs/rules/use-isnan)

    ```js
    if (isNaN(foo)) { ... }		// ✓ ok
    if (foo === NaN) { ... }	// ✗ avoid
    ```

- **强制 typeof 表达式比较的对象必须是 "undefined", "object", "boolean", "number", "string", "function", "symbol", 或 "bigint"**

    eslint: [ valid-typeof](http://eslint.cn/docs/rules/valid-typeof)

    ```js
    // ✓ ok
    typeof foo === "string"
    typeof bar == "undefined"
    typeof foo === baz
    typeof bar === typeof qux
    
    // ✗ avoid
    typeof foo === "strnig"
    typeof foo == "undefimed"
    typeof bar != "nunber"
    typeof bar !== "fucntion"
    ```

### 最佳实践

- **setter 必须有对应的 getter，getter 可以没有对应的 setter**

    eslint: [ accessor-pairs](http://eslint.cn/docs/rules/accessor-pairs)

    ```js
    // ✓ ok
    var o = {
        set a(value) {
            this.val = value;
        },
        get a() {
            return this.val;
        }
    };
    
    // ✗ avoid
    var o = {
        set a(value) {
            this.val = value;
        }
    };
    ```

- **数组的方法除了 forEach 之外，回调函数必须有返回值**

    eslint: [ array-callback-return](http://eslint.cn/docs/rules/array-callback-return)

    ```js
    // ✓ ok
    var bar = foo.map(node => node.getAttribute("id"));
    
    var foo = Array.from(nodes, function(node) {
        if (node.tagName === "DIV") {
            return true;
        }
        return false;
    });
    
    // ✗ avoid
    var indexMap = myArray.reduce(function(memo, item, index) {
        memo[item] = index;
    }, {});
    
    var foo = Array.from(nodes, function(node) {
        if (node.tagName === "DIV") {
            return true;
        }
    });
    ```

-  **var 定义的变量，允许在块外使用**

    eslint: [ block-scoped-var](http://eslint.cn/docs/rules/block-scoped-var)

    > reason: 已经禁止使用 var 了。

- **在类的非静态方法中，可以不存在对 this 的引用**

    eslint: [class-methods-use-this](http://eslint.cn/docs/rules/class-methods-use-this)

    ```js
    // ✓ ok
    class A {
        foo() {
            console.log("Hello World");
        }
    }
    ```

- **禁止函数的圈复杂度超过20**

    > https://en.wikipedia.org/wiki/Cyclomatic_complexity
    >
    > 圈复杂度数量上表现为源代码的**独立执行路径的条数**。
    >
    > ps: 在程序流程图上更容易看出。

    eslint: [complexity](http://eslint.cn/docs/rules/complexity)

- **允许函数在不同分支返回不同类型的值**

  > reason: 缺少 TypeScript 的支持，类型判断是不准确的

  eslint: [ consistent-return](http://eslint.cn/docs/rules/consistent-return)

  ```js
  // ✓ ok
  function doSomething(condition) {
      if (condition) {
          return true;
      } else {
          return 666;
      }
  }
  ```

- **强制所有控制语句使用一致的括号风格, if、else、for、while、do**

  eslint: [ curly](http://eslint.cn/docs/rules/curly)

  ```js
  // ✓ ok
  if (foo) {
      foo++;
  }
  
  // ✗ avoid
  if (foo) foo++;
  
  if (foo) {
      baz();
  } else qux();
  ```

- **switch 语句必须有 default**

  eslint: [default-case](http://eslint.cn/docs/rules/default-case)

  ```js
  // ✓ ok
  switch (a) {
      case 1:
          /* code */
          break;
  
      default:
          /* code */
          break;
  }
  
  // ✗ avoid
  switch (a) {
      case 1:
          /* code */
          break;
  }
  ```

- **点跟着属性换行**

  eslint: [dot-location](http://eslint.cn/docs/rules/dot-location)

  ```js
  // ✓ ok
  var foo = object
  .property;
  var bar = object.property;
  
  // ✗ avoid
  var foo = object.
  property;
  ```

- **必须使用 `===` 或 `!==`，禁止使用 `==` 或 `!=`**

  eslint: [eqeqeq](http://eslint.cn/docs/rules/eqeqeq)

  ```js
  // ✓ ok
  a === b
  a !== b
  
  // ✗ avoid
  a == b
  a != b
  ```

- **for in 内部可以没有 hasOwnProperty**

  eslint: [guard-for-in](http://eslint.cn/docs/rules/guard-for-in)

  ```js
  // ✓ ok
  for (key in foo) {
      doSomething(key);
  }
  ```

- **禁止使用 caller 或 callee**

  > 它们是已废弃的语法

  eslint: [no-caller](http://eslint.cn/docs/rules/no-caller)

  ```js
  // ✓ ok
  function foo(n) {
      if (n <= 0) {
          return;
      }
  
      foo(n - 1);
  }
  
  [1,2,3,4,5].map(function factorial(n) {
      return !(n > 1) ? 1 : factorial(n - 1) * n;
  });
  
  // ✗ avoid
  function foo(n) {
    	...
  
      arguments.callee(n - 1);
  }
  
  [1,2,3,4,5].map(function(n) {
      return !(n > 1) ? 1 : arguments.callee(n - 1) * n;
  });
  ```

- **switch 的 case 内有变量定义的时候，必须使用大括号将 case 内变成一个代码块**

  eslint: [no-case-declarations](http://eslint.cn/docs/rules/no-case-declarations)

  ```js
  // ✓ ok
  switch (foo) {
      case 1: {
          let x = 1;
          break;
      }
      case 2: {
          const y = 2;
          break;
      }
      case 3: {
          function f() {}
          break;
      }
      case 4:
          var z = 4;
          break;
      default: {
          class C {}
      }
  }
  
  // ✗ avoid
  switch (foo) {
      case 1:
          let x = 1;
          break;
      case 2:
          const y = 2;
          break;
      case 3:
          function f() {}
          break;
      default:
          class C {}
  }
  ```

- **禁止在正则表达式中出现形似除法操作符的开头，如 `let a = /=foo/`**

  eslint: [no-div-regex](http://eslint.cn/docs/rules/no-div-regex)

  ```js
  // ✓ ok
  function bar() { return /[=]foo/; }
  
  // ✗ avoid
  function bar() { return /=foo/; }
  ```

- **允许 if 语句中 return 语句之后有 else 块**

  eslint: [ no-else-return](http://eslint.cn/docs/rules/no-else-return)

  ```js
  // ✓ ok
  function foo() {
      if (x) {
          return y;
      } else {
          return z;
      }
  }
  ```

- **允许有空函数**

  eslint: [no-empty-function](http://eslint.cn/docs/rules/no-empty-function)

  > reason: 有时需要将一个空函数设置为某个项的默认值。

  ```js
  // ✓ ok
  function func(callback = function () {}) {
    ...
  }
  ```

  

- **禁止使用空解构模式** 

  eslint: [no-empty-pattern](http://eslint.cn/docs/rules/no-empty-pattern)

  ```js
  // ✓ ok
  var {a = {}} = foo;
  var {a = []} = foo;
  function foo({a = {}}) {}
  function foo({a = []}) {}
  
  // ✗ avoid
  var {} = foo;
  var [] = foo;
  var {a: {}} = foo;
  var {a: []} = foo;
  function foo({}) {}
  function foo([]) {}
  function foo({a: {}}) {}
  function foo({a: []}) {}
  ```

- **禁止使用 foo == null，必须使用 foo === null**

  eslint: [ no-eq-null](http://eslint.cn/docs/rules/no-eq-null)

  ```js
  // ✓ ok
  if (foo === null) { ... }
  
  while (qux !== null) { ... }
  
  // ✗ avoid
  if (foo == null) { ... }
  
  while (qux != null) { ... }
  ```

- **禁止使用 eval**

  eslint: [no-eval](http://eslint.cn/docs/rules/no-eval)

  ```js
  // ✗ avoid
  var foo = eval;
  foo("var a = 0");
  ```

- **禁止扩展原生类型**

  eslint: [no-extend-native](http://eslint.cn/docs/rules/no-extend-native)

  ```js
  // ✗ avoid
  Object.prototype.a = "a";
  Object.defineProperty(Array.prototype, "times", { value: 999 });
  ```

- **禁止不必要的 `.bind()` 调用**

  eslint: [no-extra-bind](http://eslint.cn/docs/rules/no-extra-bind)

  ```js
  // ✓ ok
  var x = function () {
      this.foo();
  }.bind(bar);
  
  var x = function (a) {
      return a + 1;
  }.bind(foo, bar);
  
  // ✗ avoid
  var x = function () {
      foo();
  }.bind(bar);
  
  var x = (() => {
      foo();
  }).bind(bar);
  
  var x = (() => {
      this.foo();
  }).bind(bar);
  
  var x = function () {
      (function () {
        this.foo();
      }());
  }.bind(bar);
  
  var x = function () {
      function foo() {
        this.bar();
      }
  }.bind(baz);
  ```

- **允许不必要的标签**

  eslint: [no-extra-label](http://eslint.cn/docs/rules/no-extra-label)

  ```js
  // ✓ ok
  A: while (a) {
      break A;
  }
  
  B: for (let i = 0; i < 10; ++i) {
      break B;
  }
  
  C: switch (a) {
      case 0:
          break C;
  }
  ```

- **禁止 `case` 语句落空**

  > 有意为之的落空需注释

  eslint: [ no-fallthrough](http://eslint.cn/docs/rules/no-fallthrough)

  ```js
  // ✓ ok
  switch(foo) {
      case 1:
          doSomething();
          break;
  
      case 2:
          doSomething();
  }
  
  switch(foo) {
      case 1:
          doSomething();
          // 落空
  
      case 2:
          doSomething();
  }
  
  // ✗ avoid
  switch(foo) {
      case 1:
          doSomething();
  
      case 2:
          doSomething();
  }
  ```

- **禁止数字字面量中使用前导和末尾小数点**

  eslint: [no-floating-decimal](http://eslint.cn/docs/rules/no-floating-decimal)

  ```js
  // ✓ ok
  var num = 0.5;
  var num = 2.0;
  var num = -0.7;
  
  // ✗ avoid
  var num = .5;
  var num = 2.;
  var num = -.7;
  ```

- **禁止对原生对象或只读的全局对象进行赋值**

  eslint: [ no-global-assign](http://eslint.cn/docs/rules/no-global-assign)

  ```js
  // ✓ ok
  a = 1
  var b = 1
  b = 2
  
  // ✗ avoid
  Object = null
  undefined = 1
  ```

- **允许使用短符号进行类型转换**

  eslint: [ no-implicit-coercion](http://eslint.cn/docs/rules/no-implicit-coercion)

  ```js
  // ✓ ok
  var b = !!foo;
  var b = ~foo.indexOf(".");
  var n = +foo;
  var n = 1 * foo;
  var s = "" + foo;
  foo += ``;
  ```

- **允许在全局范围内使用变量声明和 `function` 声明**

  eslint: [ no-implicit-globals](http://eslint.cn/docs/rules/no-implicit-globals)

  ```js
  // ✓ ok
  var foo = 1;
  
  function bar() {}
  ```

- **禁止使用类似 `eval()` 的方法**

  eslint: [ no-implied-eval](http://eslint.cn/docs/rules/no-implied-eval)

  ```js
  // ✓ ok
  setTimeout(function() {
      alert("Hi!");
  }, 100);
  
  setInterval(function() {
      alert("Hi!");
  }, 100);
  
  // ✗ avoid
  setTimeout("alert('Hi!');", 100);
  
  setInterval("alert('Hi!');", 100);
  
  execScript("alert('Hi!')");
  
  window.setTimeout("count = 5", 10);
  
  window.setInterval("foo = bar", 10);
  ```

- **禁止 `this` 关键字出现在类和类对象之外**

  eslint: [no-invalid-this](http://eslint.cn/docs/rules/no-invalid-this)

  ```js
  // ✓ ok
  function Foo() {
      // OK, this is in a legacy style constructor.
      this.a = 0;
      baz(() => this);
  }
  
  // ✗ avoid
  this.a = 0;
  baz(() => this);
  ```

- **禁用 `__iterator__` 属性**

  eslint: [ no-iterator](http://eslint.cn/docs/rules/no-iterator)

  > `__iterator__` 是一个已废弃的属性，使用 `[Symbol.iterator]` 替代它

  ```js
  // ✓ ok
  var __iterator__ = foo;
  
  // ✗ avoid
  Foo.prototype.__iterator__ = function() {
      return new FooIterator(this);
  };
  
  foo.__iterator__ = function () {};
  
  foo["__iterator__"] = function () {};
  
  ```

- **禁用标签语句**

  eslint: [no-labels](http://eslint.cn/docs/rules/no-labels)

  ```js
  // ✓ ok
  var f = {
      label: "foo"
  };
  
  // ✗ avoid
  label:
      while(true) {
          ...
      }
  
  label:
      while(true) {
          break label;
      }
  ```

- **禁用不必要的嵌套块**

  eslint: [ no-lone-blocks](http://eslint.cn/docs/rules/no-lone-blocks)

  ```js
  // ✓ ok
  while (foo) {
      bar();
  }
  
  // ✗ avoid
  while (foo) {
      bar();
      {
          baz();
      }
  }
  ```

- **允许在在循环内的函数内部出现外部定义的变量**

  eslint: [ no-loop-func](http://eslint.cn/docs/rules/no-loop-func)

  ```js
  // ✓ ok
  for (var i=10; i; i--) {
      (function() { return i; })();
  }
  ```

- **允许使用 magic numbers**

  > [魔术数字](https://zh.wikipedia.org/zh-cn/魔術數字)是在代码中多次出现的没有明确含义的数字。（最好用命名常量取代它）

  eslint: [no-magic-numbers](http://eslint.cn/docs/rules/no-magic-numbers)

  ```js
  // ✓ ok
  var dutyFreePrice = 100,
      finalPrice = dutyFreePrice + (dutyFreePrice * 0.25);
  ```

- **禁止使用多个空格**

  eslint: [no-multi-spaces](http://eslint.cn/docs/rules/no-multi-spaces)

  ```js
  // ✓ ok
  var a = 1;
  
  if(foo === "bar") {}
  
  a << b
  
  var arr = [1, 2];
  
  a ? b: c
  
  // ✗ avoid
  var a =  1;
  
  if(foo   === "bar") {}
  
  a <<  b
  
  var arr = [1,  2];
  
  a ?  b: c
  ```

- **禁止使用多行字符串**

  eslint: [ no-multi-str](http://eslint.cn/docs/rules/no-multi-str)

  ```js
  // ✓ ok
  var x = "Line 1\n" +
          "Line 2";
  
  // ✗ avoid
  var x = "Line 1 \
           Line 2";
  ```

- **禁止直接 new 一个类而不赋值**

  eslint: [ no-new](http://eslint.cn/docs/rules/no-new)

  ```js
  // ✓ ok
  var thing = new Thing();
  
  Thing();
  
  // ✗ avoid
  new Thing();
  ```

- **禁止使用 new Function**

  eslint: [no-new-func](http://eslint.cn/docs/rules/no-new-func)

  ```js
  // ✓ ok
  var x = function (a, b) {
      return a + b;
  };
  
  // ✗ avoid
  var x = new Function("a", "b", "return a + b");
  var x = Function("a", "b", "return a + b");
  ```

- **禁止使用 new 来生成 String, Number 或 Boolean**

  eslint: [ no-new-wrappers](http://eslint.cn/docs/rules/no-new-wrappers)

  ```js
  // ✓ ok
  var text = String(someValue);
  var num = Number(someValue);
  
  var object = new MyString();
  
  // ✗ avoid
  var stringObject = new String("Hello world");
  var numberObject = new Number(33);
  var booleanObject = new Boolean(false);
  ```

- **禁用八进制字面量**

  eslint: [ no-octal](http://eslint.cn/docs/rules/no-octal)

  ```js
  // ✓ ok
  var num  = "071";
  
  // ✗ avoid
  var num = 071;
  var result = 5 + 07;
  ```

- **禁止在字符串中使用八进制转义序列**

  eslint: [ no-octal-escape](http://eslint.cn/docs/rules/no-octal-escape)

  ```js
  // ✓ ok
  var foo = "Copyright \u00A9";   // unicode
  
  var foo = "Copyright \xA9";     // 十六进制
  
  // ✗ avoid
  var foo = "Copyright \251";
  ```

- **禁止对 `function` 的参数进行重新赋值**

  eslint: [no-param-reassign](http://eslint.cn/docs/rules/no-param-reassign)

  ```js
  // ✓ ok
  function foo(bar) {
      var baz = bar;
  }
  
  // ✗ avoid
  function foo(bar) {
      bar = 13;
  }
  ```

- **禁用 `__proto__` 属性**

  eslint: [no-proto](http://eslint.cn/docs/rules/no-proto)

  ```js
  // ✓ ok
  var a = Object.getPrototypeOf(obj);
  
  Object.setPrototypeOf(obj, b);
  
  var c = { __proto__: a };
  
  // ✗ avoid
  var a = obj.__proto__;
  
  var a = obj["__proto__"];
  
  obj.__proto__ = b;
  
  obj["__proto__"] = b;
  ```

- **禁止多次声明同一变量**

  eslint: [no-redeclare](http://eslint.cn/docs/rules/no-redeclare)

  ```js
  // ✓ ok
  var a = 3;
  a = 10;
  
  // ✗ avoid
  var a = 3;
  var a = 10;
  ```

- **允许使用对象的某些属性**

  eslint: [no-restricted-properties](http://eslint.cn/docs/rules/no-restricted-properties)

  ```js
  // ✓ ok
  foo.__defineGetter__(bar, baz);
  ```

- **禁止在 `return` 语句中使用赋值语句**

  eslint: [no-return-assign](http://eslint.cn/docs/rules/no-return-assign)

  ```js
  // ✓ ok
  function doSomething() {
      return foo === bar + 2;
  }
  
  // ✗ avoid
  function doSomething() {
      return foo = bar + 2;
  }
  
  function doSomething() {
      return foo += 2;
  }
  
  function doSomething() {
      return (foo = bar + 2);
  }
  ```

- **允许出现 location.href = "javascript:void(0)"**

  eslint: [no-script-url](http://eslint.cn/docs/rules/no-script-url)

  > reason: 有些场景下还是需要用到这个

  ```js
  // ✓ ok
  location.href = "javascript:void(0)";
  ```

- **禁止将自己赋值给自己**

  eslint: [ no-self-assign](http://eslint.cn/docs/rules/no-self-assign)

  ```js
  // ✓ ok
  foo = bar;
  [a, b] = [b, a];
  
  // ✗ avoid
  foo = foo;
  
  [a, b] = [a, b];
  
  [a, ...b] = [x, ...b];
  
  ({a, b} = {a, x});
  ```

- **禁止将自己与自己比较**

  eslint: [ no-self-compare](http://eslint.cn/docs/rules/no-self-compare)

  ```js
  // ✗ avoid
  var x = 10;
  if (x === x) {
      x = 20;
  }
  ```

- **禁止使用逗号操作符**

  eslint: [no-sequences](http://eslint.cn/docs/rules/no-sequences)

  > 此规则禁止逗号操作符的使用，以下情况除外：
  >
  > - 在初始化或者更新部分 `for` 语句时。
  > - 如果表达式序列被明确包裹在括号中。

  ```js
  // ✓ ok
  foo = (doSomething(), val);
  
  for (i = 0, j = 10; i < j; i++, j--);
  
  // ✗ avoid
  foo = doSomething(), val;
  
  for (; doSomething(), !!test; );
  ```

- **禁止抛出异常字面量**

  eslint: [no-throw-literal](http://eslint.cn/docs/rules/no-throw-literal)

  ```js
  // ✓ ok
  throw new Error("error");
  
  var e = new Error("error");
  throw e;
  
  try {
      throw new Error("error");
  } catch (e) {
      throw e;
  }
  
  // ✗ avoid
  throw "error";
  
  throw 0;
  
  throw undefined;
  
  throw null;
  ```

- **禁用一成不变的循环条件**

  eslint: [ no-unmodified-loop-condition](http://eslint.cn/docs/rules/no-unmodified-loop-condition)

  ```js
  // ✓ ok
  while (node) {
      doSomething(node);
      node = node.parent;
  }
  
  for (var j = 0; j < items.length; ++j) {
      doSomething(items[j]);
  }
  
  // ✗ avoid
  while (node) {
      doSomething(node);
  }
  node = other;
  
  for (var j = 0; j < items.length; ++i) {
      doSomething(items[j]);
  }
  
  while (node !== root) {
      doSomething(node);
  }
  ```

- **禁止出现未使用过的表达式**

  eslint: [no-unused-expressions](http://eslint.cn/docs/rules/no-unused-expressions)

  ```js
  // ✓ ok
  f()
  
  a = 0
  
  // ✗ avoid
  n + 1
  ```

- **禁止出现没用到的 label （已经禁止使用 label 了）**

  eslint: [no-unused-labels](http://eslint.cn/docs/rules/no-unused-labels) 

- **禁止不必要的 `.call()` 和 `.apply()`**

  eslint: [ no-useless-call](http://eslint.cn/docs/rules/no-useless-call)

  ```js
  // ✓ ok
  foo.call(obj, 1, 2, 3);
  foo.apply(obj, [1, 2, 3]);
  obj.foo.call(null, 1, 2, 3);
  obj.foo.apply(null, [1, 2, 3]);
  
  // ✗ avoid
  foo.call(undefined, 1, 2, 3);
  foo.apply(undefined, [1, 2, 3]);
  foo.call(null, 1, 2, 3);
  foo.apply(null, [1, 2, 3]);
  obj.foo.call(obj, 1, 2, 3);
  obj.foo.apply(obj, [1, 2, 3]);
  ```

- **禁止不必要的 `catch` 子句**

  eslint: [ no-useless-catch](http://eslint.cn/docs/rules/no-useless-catch)

  ```js
  // ✓ ok
  try {
    doSomethingThatMightThrow();
  } catch (e) {
    handleError(e);
  }
  
  try {
    doSomethingThatMightThrow();
  } catch (e) {
    doSomethingBeforeRethrow();
    throw e;
  }
  
  // ✗ avoid
  try {
    doSomethingThatMightThrow();
  } catch (e) {
    throw e;
  }
  ```

- **禁止不必要的字符串字面量或模板字面量的连接**

  eslint: [ no-useless-concat](http://eslint.cn/docs/rules/no-useless-concat)

  ```js
  // ✓ ok
  var c = a + b;
  var c = '1' + a;
  var a = 1 + '1';
  var c = 1 - 2;
  var c = "foo" +
      "bar";
  
  // ✗ avoid
  var a = `some` + `string`;
  var a = '1' + '0';
  var a = '1' + `0`;
  var a = `1` + '0';
  var a = `1` + `0`;
  ```

- **禁用不必要的转义字符**

  eslint: [no-useless-escape](http://eslint.cn/docs/rules/no-useless-escape)

  ```js
  // ✓ ok
  "\"";
  '\'';
  "\x12";
  "\u00a9";
  "\371";
  "xs\u2111";
  `\``;
  `\${${foo}}`;
  `$\{${foo}}`;
  /\\/g;
  /\t/g;
  /\w\$\*\^\./;
  
  // ✗ avoid
  "\'";
  '\"';
  "\#";
  "\e";
  `\"`;
  `\"${foo}\"`;
  `\#{foo}`;
  /\!/;
  /\@/;
  ```

- **禁止多余的 return 语句**

  eslint: [no-useless-return](http://eslint.cn/docs/rules/no-useless-return)

  ```js
  // ✓ ok
  function foo() { return 5; }
  
  // ✗ avoid
  function foo() { return; }
  ```

- **禁用 `void` 操作符**

  eslint: [no-void](http://eslint.cn/docs/rules/no-void)

  ```js
  // ✗ avoid
  void foo
  
  var foo = void bar();
  ```

- **允许在注释中使用特定的警告术语TODO和FIXME**

  eslint: [ no-warning-comments](http://eslint.cn/docs/rules/no-warning-comments)

  ```js
  function callback(err, results) {
    if (err) {
      console.error(err);
      return;
    }
    // TODO														// ✓ ok
  }
  ```

- **禁用 `with` 语句**

  eslint: [no-with](http://eslint.cn/docs/rules/no-with)

  ```js
  // ✓ ok
  const r = ({x, y}) => Math.sqrt(x * x + y * y);
  
  // ✗ avoid
  with (point) {
      r = Math.sqrt(x * x + y * y); // is r a member of point?
  }
  ```

- **Promise 的 reject 中必须传入 Error 对象，而不是字面量**

  eslint: [prefer-promise-reject-errors](http://eslint.cn/docs/rules/prefer-promise-reject-errors)

  ```js
  // ✓ ok
  Promise.reject(new Error("something bad happened"));
  
  new Promise(function(resolve, reject) {
    reject(new Error("something bad happened"));
  });
  
  // ✗ avoid
  Promise.reject("something bad happened");
  
  new Promise(function(resolve, reject) {
    reject("something bad happened");
  });
  ```

- **强制在 `parseInt()` 使用基数参数**

  eslint: [ radix](http://eslint.cn/docs/rules/radix)

  ```js
  // ✓ ok
  var num = parseInt("071", 10);
  
  var num = parseInt("071", 8);
  
  // ✗ avoid
  var num = parseInt("071");
  
  var num = parseInt(someValue);
  
  var num = parseInt("071", "abc");
  
  var num = parseInt();
  ```

- **禁止使用不带 `await` 表达式的 async 函数**

  eslint: [ require-await](http://eslint.cn/docs/rules/require-await)

  ```js
  // ✓ ok
  async function foo() {
      await doSomething();
  }
  
  bar(async () => {
      await doSomething();
  });
  
  // ✗ avoid
  async function foo() {
      doSomething();
  }
  
  bar(async () => {
      doSomething();
  });
  ```

- **允许所有的 `var` 声明不在它们所在的作用域顶部**

  eslint: [ vars-on-top](http://eslint.cn/docs/rules/vars-on-top)

  ```js
  // ✓ ok
  function doSomething() {
      for (var i=0; i<10; i++) {}
  }
  
  function doSomething() {
      var first;
      if (true) {
          first = true;
      }
      var second;
  }
  ```

- **必须使用 `if (foo === 5)` 而不是 `if (5 === foo)`**

  eslint: [yoda](http://eslint.cn/docs/rules/yoda)

  ```js
  // ✓ ok
  if (5 & value) { ... }
  
  if (value === "red") { ... }
  
  // ✗ avoid
  if (true == flag) { ... }
  
  if (5 > count) { ... }
  ```

### 严格模式

- **禁止使用 "strict"，除了全局统一**

  eslint: [strict](http://eslint.cn/docs/rules/strict)

  ```js
  // ✓ ok
  "use strict";
  
  function foo() {
  }
  
  // ✗ avoid
  function foo() {
      "use strict";
  }
  ```

### 变量

- **允许变量在定义的时候可以不赋值**

  eslint: [init-declarations](http://eslint.cn/docs/rules/init-declarations)

  ```js
  // ✓ ok
  var bar;
  ```

- **允许 catch 子句的参数与外层作用域中的变量同名**

  eslint: [no-catch-shadow](http://eslint.cn/docs/rules/no-catch-shadow)

  > 已弃用，被no-shadow替代

  ```js
  // ✓ ok
  var err = "x";
  
  try {
      throw "problem";
  } catch (err) {
  
  }
  ```

- **禁止对一个变量使用 delete**

  eslint: [no-delete-var](http://eslint.cn/docs/rules/no-delete-var)

  ```js
  // ✗ avoid
  var x;
  delete x;
  ```

- **禁止 label 名称与已定义的变量重复    （已经禁止使用 label 了）**

  eslint: [no-label-var](http://eslint.cn/docs/rules/no-label-var)

- **允许使用全局变量**  

  eslint: [ no-restricted-globals](http://eslint.cn/docs/rules/no-restricted-globals)

  ```js
  // ✓ ok
  var event = 1;		// 声明一个自定义全局变量
  ```

- **允许变量名与上层作用域内的已定义的变量重复**

  eslint: [ no-shadow](http://eslint.cn/docs/rules/no-shadow)

  > reason：很多时候函数的形参和传参是同名的

  ```js
  // ✓ ok
  var a = 3;
  function b() {
      var a = 10;
  }
  
  if (true) {
      let a = 5;
  }
  ```

- **禁止使用关键字作为变量名**

  eslint: [no-shadow-restricted-names](http://eslint.cn/docs/rules/no-shadow-restricted-names)

  ```js
  // ✓ ok
  function f(a, b){}
  
  // ✗ avoid
  function NaN(){}
  
  !function(Infinity){};
  
  var undefined = 5;
  
  try {} catch(eval){}
  ```

- **禁止使用未定义的变量**

  eslint: [ no-undef](http://eslint.cn/docs/rules/no-undef)

  ```js
  // ✓ ok
  var b = 666;
  b = 10;
  
  // ✗ avoid
  var a = someFunction();
  b = 10;
  ```

- **禁止将变量初始化为 `undefined`**

  eslint: [ no-undef-init](http://eslint.cn/docs/rules/no-undef-init)

  ```js
  // ✓ ok
  var foo = 100;
  let bar = 200;
  
  // ✗ avoid
  var foo = undefined;
  let bar = undefined;
  ```
  
- **允许使用 undefined**

    eslint: [ no-undefined](http://eslint.cn/docs/rules/no-undefined)

    ```js
    // ✓ ok
    if (foo === undefined) {
        ...
    }
    ```

- **禁止出现未使用过的变量**

    eslint: [no-unused-vars](http://eslint.cn/docs/rules/no-unused-vars)

    ```js
    // ✓ ok
    var x = 10;
    alert(x);
    
    // ✗ avoid
    some_unused_var = 42;
    ```

- **变量必须先定义后使用**

    eslint: [no-use-before-define](http://eslint.cn/docs/rules/no-use-before-define)

    ```js
    // ✓ ok
    var a = 10;
    alert(a);
    
    // ✗ avoid
    alert(a);
    var a = 10;
    ```

### Node.js 和 CommonJS

这些规则是关于Node.js 或 在浏览器中使用CommonJS 的：

- **不强制 callback 之后必须立即 return**

    eslint: [ callback-return](http://eslint.cn/docs/rules/callback-return)

    ```js
    // ✓ ok
    function foo(err, callback) {
        if (err) {
            callback(err);
        }
        callback();
    }
    
    function foo(err, callback) {
        if (err) {
            return callback(err);
        }
        callback();
    }
    ```

- **不强制 require 必须在全局作用域下**

    eslint: [global-require](http://eslint.cn/docs/rules/global-require)

    ```js
    // ✓ ok
    function readFile(filename, callback) {
        var fs = require('fs');			// ✓ ok
        fs.readFile(filename, callback)
    }
    ```

- **不强制 callback 中的 err 必须被处理**

    eslint: [ handle-callback-err](http://eslint.cn/docs/rules/handle-callback-err)

    > reason: 它是通过字符串匹配来判断函数参数 err 的，不准确

    ```js
    // ✓ ok
    function loadData (err, data) {
        doSomething();
    }
    
    function loadData (err, data) {
        if (err) {
            console.log(err.stack);
        }
        doSomething();
    }
    ```

- **禁止直接使用 Buffer 构造函数**

    eslint: [ no-buffer-constructor](http://eslint.cn/docs/rules/no-buffer-constructor)

    > reason: Buffer 构造函数是已废弃的语法

    ```js
    // ✓ ok
    Buffer.alloc(5);
    Buffer.allocUnsafe(5);
    Buffer.from([1, 2, 3]);
    
    // ✗ avoid
    new Buffer(5);
    new Buffer([1, 2, 3]);
    
    Buffer(5);
    Buffer([1, 2, 3]);
    ```

- **不强制 require 必须放在一起**

    eslint: [ no-mixed-requires](http://eslint.cn/docs/rules/no-mixed-requires)

    ```js
    // ✓ ok
    var fs = require('fs'),
        i = 0;
    
    var async = require('async'),
        debug = require('diagnostics').someFunction('my-module'),
        eslint = require('eslint');
    ```

    

- **禁止直接 new require("foo")**

  eslint: [ no-new-require](http://eslint.cn/docs/rules/no-new-require)

  ```js
  // ✓ ok
  var AppHeader = require('app-header');
  var appHeader = new AppHeader();
  
  // ✗ avoid
  var appHeader = new require('app-header');
  ```

- **禁止对 `__dirname` 或 `__filename` 使用字符串连接**

  eslint: [ no-path-concat](http://eslint.cn/docs/rules/no-path-concat)

  > reason: 不同平台下的路径符号不一致，建议使用 path 处理平台差异性

  ```js
  // ✓ ok
  const path = require('path');
  var fullPath = path.join(__dirname, "foo.js");
  var fullPath = path.resolve(__dirname, "foo.js");
  
  // ✗ avoid
  var fullPath = __dirname + "/foo.js";
  
  var fullPath = __filename + "/foo.js";
  ```

- **允许使用 process.env**

  eslint: [no-process-env](http://eslint.cn/docs/rules/no-process-env)

  ```js
  // ✓ ok
  if(process.env.NODE_ENV === "development") {
      //...
  }
  
  var config = require("./config");
  
  if(config.env === "development") {
      //...
  }
  ```

- **允许使用 process.exit(0)**

  eslint: [ no-process-exit](http://eslint.cn/docs/rules/no-process-exit)

  ```js
  // ✓ ok
  process.exit(1);
  process.exit(0);
  
  Process.exit();
  var exit = process.exit;
  ```

- **不限制使用Node.js的模块**

  eslint: [no-restricted-modules](http://eslint.cn/docs/rules/no-restricted-modules)

  ```js
  // ✓ ok
  var fs = require('fs');
  var cluster = require('cluster');
  ```

- **允许**使用 node 中的**同步的方法**，比如 `fs.readFileSync`

  eslint: [ no-sync](http://eslint.cn/docs/rules/no-sync)

  ```js
  // ✓ ok
  fs.existsSync(somePath);
  ```

### 代码风格

- **如果数组元素内或元素间有换行，则要求换行**

  eslint: [array-element-newline](http://eslint.cn/docs/rules/array-element-newline)

  ```js
  // ✓ ok
  var a = [];
  var b = [1];
  var c = [1, 2];
  var d = [1, 2, 3];
  var e = [
      function foo() {
          dosomething();
      },
      function bar() {
          dosomething();
      }
  ];
  
  // ✗ avoid
  var d = [1,
      2, 3];
  var e = [
      function foo() {
          dosomething();
      }, function bar() {
          dosomething();
      }
  ];
  ```

- **禁止在数组括号内出现空格**

  eslint: [array-bracket-spacing](http://eslint.cn/docs/rules/array-bracket-spacing)

  ```js
  // ✓ ok
  var arr = [];
  var arr = ['foo', 'bar', 'baz'];
  var arr = [['foo'], 'bar', 'baz'];
  var arr = [
    'foo',
    'bar',
    'baz'
  ];
  
  var [x, y] = z;
  var [x,y] = z;
  
  // ✗ avoid
  var arr = [ 'foo', 'bar' ];
  var arr = ['foo', 'bar' ];
  var arr = [ ['foo'], 'bar'];
  var arr = [[ 'foo' ], 'bar'];
  var arr = [ 'foo',
    'bar'
  ];
  var [ x, y ] = z;
  var [ x,y ] = z;
  ```

- **元素视长度自由控制换行，不强制**

  eslint: [array-element-newline](http://eslint.cn/docs/rules/array-element-newline)

  ```js
  // ✓ ok
  var c = [1, 2];
  var d = [1, 2, 3];
  
  var arr = [
      1,
      2,
      3
  ];
  ```

- **大括号后要求使用一个或多个空格**

  eslint: [block-spacing](http://eslint.cn/docs/rules/block-spacing)

  ```js
  // ✓ ok
  function foo() { return true; }
  if (foo) { bar = 0; }
  
  // ✗ avoid
  function foo() {return true;}
  if (foo) { bar = 0;}
  function baz() {let i = 0;
      return i;
  }
  ```

- **在function，if，try等方法后使用统一的大括号风格**

  eslint: [brace-style](http://eslint.cn/docs/rules/brace-style)

  ```js
  // ✓ ok
  function foo () {
    return true;
  }
  
  if (foo) {
    bar();
  }
  
  if (foo) {
    bar();
  } else {
    baz();
  }
  
  try {
    somethingRisky();
  } catch(e) {
    handleError();
  }
  
  // ✗ avoid
  function foo()
  {
    return true;
  }
  
  if (foo)
  {
    bar();
  }
  
  try
  {
    somethingRisky();
  } catch(e)
  {
    handleError();
  }
  
  if (foo) {
    bar();
  }
  else {
    baz();
  }
  ```

- **变量名使用驼峰命名**

  eslint: [camelcase](http://eslint.cn/docs/rules/camelcase)

  ```js
  // ✓ ok
  var myFavoriteColor   = "#112C85";
  var _myFavoriteColor  = "#112C85";
  var myFavoriteColor_  = "#112C85";
  const MY_FAVORITE_COLOR = "#112C85";
  
  // ✗ avoid
  var my_favorite_color = "#112C85";
  ```

- **不要求注释必须大写开头**

  eslint: [capitalized-comments](http://eslint.cn/docs/rules/capitalized-comments)

  > 咱们通常是中文，所以不用了

- **禁用拖尾逗号**

  eslint: [comma-dangle](http://eslint.cn/docs/rules/comma-dangle)

  ```js
  // ✓ ok
  var foo = {
      bar: "baz",
      qux: "quux"
  };
  
  var arr = [1, 2];
  
  // ✗ avoid
  var foo = {
      bar: "baz",
      qux: "quux",
  };
  
  var arr = [1, 2, ];
  ```

- **逗号前不空格，后空格**

  eslint: [comma-spacing](http://eslint.cn/docs/rules/comma-spacing)

  ```js
  // ✓ ok
  var foo = 1, bar = 2;
  var arr = [1, 2];
  
  // ✗ avoid
  var foo = 1 ,bar = 2;
  var arr = [1 , 2];
  foo(a ,b);
  ```

- **要求逗号放在数组元素、对象属性或变量声明之后，且在同一行**

  eslint: [ comma-style](http://eslint.cn/docs/rules/comma-style)

  ```js
  // ✓ ok
  var foo = 1, bar = 2;
  
  var foo = 1,
      bar = 2;
  
  function bar() {
      return {
          "a": 1,
          "b:": 2
      };
  }
  
  // ✗ avoid
  var foo = 1
  ,
  bar = 2;
  
  var foo = 1
    , bar = 2;
  
  
  var foo = ["apples"
             , "oranges"];
  
  function bar() {
      return {
          "a": 1
          ,"b:": 2
      };
  }
  ```

- **禁止在计算属性内使用空格**

  eslint: [computed-property-spacing](http://eslint.cn/docs/rules/computed-property-spacing)

  ```js
  // ✓ ok
  obj[foo]
  obj['foo']
  var x = {[b]: a}
  obj[foo[bar]]
  
  // ✗ avoid
  obj[foo ]
  obj[ 'foo']
  var x = {[ b ]: a}
  ```

- **this 的别名可以自定**

  eslint: [consistent-this](http://eslint.cn/docs/rules/consistent-this)

  ```js
  // ✓ ok
  var self = this;
  var that = this;
  var _this = this;
  ```

- **强制非空文件末尾至少存在一行空行 (LF)**

  eslint: [eol-last](http://eslint.cn/docs/rules/eol-last)

  ```js
  // ✓ ok
  function doSmth() {
    var foo = 2;
  }
  			// 文件末尾空行
  ```

  ```js
  // ✗ avoid
  function doSmth() {
    var foo = 2;
  }
  ```

- **禁止在函数调用时函数名和开括号之间有空格**

  eslint: [func-call-spacing](http://eslint.cn/docs/rules/func-call-spacing)

  ```js
  // ✓ ok
  fn();
  
  // ✗ avoid
  fn ();
  
  fn
  ();
  ```

- **函数名与赋值给它们的变量名或属性名相匹配**

  eslint: [ func-name-matching](http://eslint.cn/docs/rules/func-name-matching)

  ```js
  // ✓ ok
  var foo = function foo() {};
  var foo = function() {};
  var foo = () => {};
  foo = function foo() {};
  
  obj.foo = function foo() {};
  obj['foo'] = function foo() {};
  obj['foo//bar'] = function foo() {};
  obj[foo] = function bar() {};
  
  var obj = {foo: function foo() {}};
  var obj = {[foo]: function bar() {}};
  var obj = {'foo//bar': function foo() {}};
  var obj = {foo: function() {}};
  
  obj['x' + 2] = function bar(){};
  var [ bar ] = [ function bar(){} ];
  ({[foo]: function bar() {}})
  
  module.exports = function foo(name) {};
  module['exports'] = function foo(name) {};
  
  // ✗ avoid
  var foo = function bar() {};
  foo = function bar() {};
  obj.foo = function bar() {};
  obj['foo'] = function bar() {};
  var obj = {foo: function bar() {}};
  ({['foo']: function bar() {}});
  ```

- **函数表达式可以没有名字**

  eslint: [func--names](http://eslint.cn/docs/rules/func-names)

  ```js
  // ✓ ok
  (function() {
      // ...
  }())
  ```

- **允许使用 function 声明或函数表达式**

  eslint: [func-style](http://eslint.cn/docs/rules/func-style)

  ```js
  // ✓ ok
  function foo() {
      // ...
  }
  
  // ✓ ok
  var foo = function() {
      // ...
  };
  ```

- **如果函数的任一参数有换行，则要求在函数括号内换行。否则禁止换行**

  eslint: [ function-paren-newline](http://eslint.cn/docs/rules/function-paren-newline)

  ```js
  // ✓ ok
  function foo(bar, baz) {}
  
  var foo = function(
    bar,
    baz
  ) {};
  
  foo(
    bar,
    baz,
    qux
  );
  
  // ✗ avoid
  function foo(bar,
    baz
  ) {}
  
  var foo = function(
    bar, baz
  ) {};
  
  foo(bar,
    baz);
  ```

- **除关键字和保留字外，不限制标识符的使用。**

  eslint: [id-blacklist](http://eslint.cn/docs/rules/id-blacklist)

  ```js
  // ✓ ok
  var data = {...};
  
  function callback() {
      // ...
  }
  
  var aBaBaB = 999;
  
  // ✗ avoid
  const class = 100;
  const if = 200;
  ```

- **不限制标识符的最小和最大长度**

  eslint: [id-length](http://eslint.cn/docs/rules/id-length)

  ```js
  // ✓ ok
  const a = 100;
  const aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = 100
  ```

  

- **不限制标识符的使用**

  eslint: [id-match](http://eslint.cn/docs/rules/id-match)

- **禁止在箭头函数体之前出现换行**

  eslint: [implicit-arrow-linebreak](http://eslint.cn/docs/rules/implicit-arrow-linebreak)

  ```js
  // ✓ ok
  (foo) => bar;
  
  (foo) => (bar);
  
  (foo) => bar => baz;
  
  (foo) => (
    bar()
  );
  
  // ✗ avoid
  (foo) =>
    bar;
  
  (foo) =>
    (bar);
  
  (foo) =>
    bar =>
      baz;
  
  (foo) =>
  (
    bar()
  );
  ```

- **四个空格缩进**

  eslint: [indent](http://eslint.cn/docs/rules/indent)

  ```js
  // ✓ ok
  if (a) {
      b=c;
      function foo(d) {
          e=f;
      }
  }
  
  // ✗ avoid
  if (a) {
    b=c;
    function foo(d) {
      e=f;
    }
  }
  ```

- **强制所有不包含单引号的 JSX 属性值使用单引号**

  eslint: [jsx-quotes](http://eslint.cn/docs/rules/jsx-quotes)

  ```js
  // ✓ ok
  <a b='c' />
  <a b="'" />
  
  // ✗ avoid
  <a b="c" />
  ```

- **强制在冒号后只有一个空格**

  eslint: [key-spacing](http://eslint.cn/docs/rules/key-spacing)

  ```js
  var obj = { "foo": 42 };		// ✓ ok
  
  var obj = { "foo" : 42 };		// ✗ avoid
  ```

- **关键字和类似关键字的符号前后均有空格**

  eslint: [keyword-spacing](http://eslint.cn/docs/rules/keyword-spacing)

  ```js
  // ✓ ok
  if (foo) {
      //...
  } else if (bar) {
      //...
  } else {
      //...
  }
  
  // ✗ avoid
  if (foo) {
      //...
  }else if (bar) {
      //...
  }else {
      //...
  }
  ```

- **不强制注释之前或之后是否应该有空行**

  eslint: [lines-around-comment](http://eslint.cn/docs/rules/lines-around-comment)

  ```js
  // ✓ ok
  var night = "long";
  /* what a great and wonderful day */
  var day = "great"
  ```

  ```js
  // ✓ ok
  var night = "long";
  
  /* what a great and wonderful day */
  var day = "great"
  ```

  ```js
  // ✓ ok
  var night = "long";
  
  /* what a great and wonderful day */
  
  var day = "great"
  ```

  

- **要求在类成员之后有一行空行**

  eslint: [lines-between-class-members](http://eslint.cn/docs/rules/lines-between-class-members)

  ```js
  // ✓ ok
  class MyClass {
    foo() {
      //...
    }
  
    bar() {
      //...
    }
  }
  
  // ✗ avoid
  class MyClass {
    foo() {
      //...
    }
    bar() {
      //...
    }
  }
  ```

- **不强制块语句的最大可嵌套深度**

  eslint: [ max-depth](http://eslint.cn/docs/rules/max-depth)

  ```js
  // ✓ ok
  function foo() {
      for (;;) { // Nested 1 deep
          let val = () => (param) => { // Nested 2 deep
             if (true) { // Nested 3 deep
                  if (true) { // Nested 4 deep
                    ...
                  }
              }
          };
      }
  }
  ```

- **不强制一行的最大长度**

  eslint: [max-len](http://eslint.cn/docs/rules/max-len)

- **不强制一个文件的最大行数**

  eslint: [max-lines](http://eslint.cn/docs/rules/max-lines)

- **不强制回调函数最大嵌套深度**

  eslint: [max-nested-callbacks](http://eslint.cn/docs/rules/max-nested-callbacks)

  ```js
  // ✓ ok
  foo(function () {
      bar(function () {
          baz(function() {
              qux(function () {
  							...
              });
          });
      });
  });
  ```

- **不强制函数定义中最大参数个数**

  eslint: [max-params](http://eslint.cn/docs/rules/max-params)

  ```js
  // ✓ ok
  function f(a, b, c, d, e, f, g, h, i, j, k, l, m, n, o) {
    ...
  }
  ```

- **不强制一个函数中所允许允许的最大语句数量**

  eslint: [max-statements](http://eslint.cn/docs/rules/max-statements)

- **强制每一行中所允许的最大语句数量为 1**

  eslint: [max-statements-per-line](http://eslint.cn/docs/rules/max-statements-per-line)

  ```js
  // ✓ ok
  var bar, baz;
  if (condition) bar = 1;
  
  // ✗ avoid
  var bar; var baz;
  if (condition) { bar = 1; }
  ```

- **不要求多行注释使用唯一的风格**

  eslint: [multiline-comment-style](http://eslint.cn/docs/rules/multiline-comment-style)

  ```js
  // ✓ ok
  /*
   * 多行注释
   * 多行注释
   */
  
  /* 多行注释
   * 多行注释
   */
  
  // 多行注释
  // 多行注释
  ```

  

- **不强制要求在三元操作数中间换行，看表达式长度自行决定**

  eslint: [ multiline-ternary](http://eslint.cn/docs/rules/multiline-ternary)

  ```js
  // ✓ ok
  foo > bar ?
      value1 :
      value2;
  
  foo > bar ? value1 : value2;
  ```

- **构造函数名首字母大写**

  eslint: [new-cap](http://eslint.cn/docs/rules/new-cap)

  ```js
  var friend = new Person();	// ✓ ok
  
  var friend = new person();	// ✗ avoid
  ```

- **要求调用无参构造函数时有圆括号**

  eslint: [new-parens](http://eslint.cn/docs/rules/new-parens)

  ```js
  // ✓ ok
  var person = new Person();
  var person = new (Person)();
  
  // ✗ avoid
  var person = new Person;
  var person = new (Person);
  ```

- **方法链中的每个调用之后或或深度成员访问之后有一个换行符**

  eslint: [newline-per-chained-call](http://eslint.cn/docs/rules/newline-per-chained-call)

  ```js
  // ✓ ok
  obj
    .prop.method()
    .method2()
    .method3().prop;
  
  // ✗ avoid
  obj.method().method2().method3();
  
  obj
    .chain({}).map(foo)
    .filter(bar);
  ```

- **通常不允许使用 Array的构造函数来创建数组。唯一的例外是通过给构造函数传入指定的一个数值来创建稀疏数组**

  eslint: [no-array-constructor](http://eslint.cn/docs/rules/no-array-constructor)

  ```js
  // ✓ ok
  new Array(someOtherArray.length)
  Array(500)
  
  // ✗ avoid
  new Array(0, 1, 2)
  Array(0, 1, 2)
  ```

- **不禁止位运算**

  eslint: [ no-bitwise](http://eslint.cn/docs/rules/no-bitwise)

  > 通常是不用，没必要禁止

  ```js
  // ✓ ok
  var x = y | z;
  
  var x = y & z;
  
  var x = y ^ z;
  
  var x = ~ z;
  
  var x = y << z;
  
  var x = y >> z;
  
  var x = y >>> z;
  
  x |= y;
  
  x &= y;
  
  x ^= y;
  
  x <<= y;
  
  x >>= y;
  
  x >>>= y;
  ```

- **禁止使用 continue 语句**

  eslint: [no-continue](http://eslint.cn/docs/rules/no-continue)

  ```js
  // ✓ ok
  var sum = 0,
      i;
  
  for(i = 0; i < 10; i++) {
      if(i < 5) {
         sum += i;
      }
  }
  
  // ✗ avoid
  var sum = 0,
      i;
  
  for(i = 0; i < 10; i++) {
      if(i >= 5) {
          continue;
      }
  
      sum += i;
  }
  ```

- **允许注释和代码出现在同一行**

  eslint: [no-inline-comments](http://eslint.cn/docs/rules/no-inline-comments)

  ```js
  // ✓ ok
  var a = 1; // declaring a to 1
  ```

- **禁止 if 语句作为唯一语句出现在 else 语句块中**

  eslint: [no-lonely-if](http://eslint.cn/docs/rules/no-lonely-if)

  ```js
  // ✓ ok
  if (condition) {
      // ...
  } else if (anotherCondition) {
      // ...
  }
  
  // ✗ avoid
  if (condition) {
      // ...
  } else {
      if (anotherCondition) {
          // ...
      }
  }
  ```

- **禁止混合使用不同的操作符**

  eslint: [no-mixed-operators](http://eslint.cn/docs/rules/no-mixed-operators)

  ```js
  // ✓ ok
  var foo = a || b || c;
  var foo = a && b && c;
  var foo = (a && b < 0) || c > 0 || d + 1 === 0;
  var foo = a && (b < 0 || c > 0 || d + 1 === 0);
  
  var foo = a + b - c;
  
  // ✗ avoid
  var foo = a && b < 0 || c > 0 || d + 1 === 0;
  var foo = a & b | c;
  ```

- **禁止空格和 tab 的混合缩进**

  eslint: [no-mixed-spaces-and-tabs](http://eslint.cn/docs/rules/no-mixed-spaces-and-tabs)

  ```js
  // ✓ ok
  function add(x, y) {
  // --->return x + y;
      return x + y;
  }
  
  // ✗ avoid
  function add(x, y) {
  // --->..return x + y;
  
        return x + y;
  }
  
  function main() {
  // --->var x = 5,
  // --->....y = 7;
  
      var x = 5,
          y = 7;
  }
  ```

- **禁止在单行语句中使用多个赋值**

  eslint: [no-multi-assign](http://eslint.cn/docs/rules/no-multi-assign)

  ```js
  // ✓ ok
  var a = 5;
  var b = a;
  
  // ✗ avoid
  var a = b = c = 5;
  ```

- **允许否定表达式**

  eslint:[no-negated-condition](http://eslint.cn/docs/rules/no-negated-condition)

  ```js
  // ✓ ok
  if (!a) {
      doSomething();
  }
  ```

- **禁止使用嵌套的三元表达式**

  eslint: [no-nested-ternary](http://eslint.cn/docs/rules/no-nested-ternary)

  ```js
  // ✓ ok
  var thing = foo ? bar : foobar;
  
  // ✗ avoid
  var thing = foo ? bar : baz === qux ? quxx : foobar;
  
  foo ? baz === qux ? quxx() : foobar() : bar();
  ```

- **禁用 `Object` 的构造函数**

  eslint: [no-new-object](http://eslint.cn/docs/rules/no-new-object)

  ```js
  // ✓ ok
  var myObject = new CustomObject();
  
  var myObject = {};
  
  // ✗ avoid
  var myObject = new Object();
  
  var myObject = new Object;
  ```

- **允许一元操作符 `++` 和 `--`**

  eslint: [ no-plusplus](http://eslint.cn/docs/rules/no-plusplus)

  ```js
  // ✓ ok
  var foo = 0;
  foo++;
  
  var bar = 42;
  bar--;
  
  for (i = 0; i < l; i++) {
      return;
  }
  ```

- **不限定不能使用的语法**

  eslint: [no-restricted-syntax](http://eslint.cn/docs/rules/no-restricted-syntax)

- **允许使用 tab （制表符）**

  eslint: [no-tabs](http://eslint.cn/docs/rules/no-tabs)

- **允许使用三元操作符**

  eslint: [no-ternary](http://eslint.cn/docs/rules/no-ternary)

  ```js
  // ✓ ok
  var foo = isBar ? baz : qux;
  ```

- **禁止使用行尾空白（空格、tab 和其它 Unicode 空白字符）**

  eslint: [no-trailing-spaces](http://eslint.cn/docs/rules/no-trailing-spaces)

  ```js
  // ✓ ok
  var foo = 0;
  var baz = 5;
  
  // ✗ avoid
  var foo = 0;//•••••
  var baz = 5;//••
  //•••••
  ```

- **允许标识符中有悬空下划线**

  eslint: [no-underscore-dangle](http://eslint.cn/docs/rules/no-underscore-dangle)

  ```js
  // ✓ ok
  var foo_;
  var __proto__ = {};
  foo._bar();
  ```

- **当有更简单的结构可以代替三元操作符时，禁止使用三元操作符**

  eslint: [no-unneeded-ternary](http://eslint.cn/docs/rules/no-unneeded-ternary)

  ```js
  // ✓ ok
  var a = x === 2 ? "Yes" : "No";
  
  var a = x !== false;
  
  var a = x ? "Yes" : "No";
  
  var a = x ? y : x;
  
  var a = x ? x : 1;
  
  // ✗ avoid
  var a = x === 2 ? true : false;
  
  var a = x ? true : false;
  
  var a = f(x ? x : 1);
  ```

- **禁止属性前有空白**

  eslint: [no-whitespace-before-property](http://eslint.cn/docs/rules/no-whitespace-before-property)

  ```js
  // ✓ ok
  foo.bar
  
  foo[bar]
  
  foo
    .bar()
    .baz()
  
  // ✗ avoid
  foo [bar]
  
  foo. bar
  
  foo .bar
  
  foo. bar. baz
  
  foo. bar()
    .baz()
  
  foo
    .bar(). baz()
  ```

- **不限制单行语句的位置**

  eslint: [nonblock-statement-body-position](http://eslint.cn/docs/rules/nonblock-statement-body-position)

  ```js
  // ✓ ok
  if (foo) {
    bar();
  } else {
    baz();
  }
  
  if (foo) bar();
  else baz();
  
  while (foo) bar();
  
  for (let i = 1; i < foo; i++) bar();
  
  do bar(); while (foo)
  
  // ✓ ok
  if (foo)
    bar();
  else
    baz();
  
  while (foo)
    bar();
  
  for (let i = 1; i < foo; i++)
    bar();
  
  do
    bar();
  while (foo)
  ```

- **如果在属性内部或属性之间有换行符，就要求有换行符**

  eslint: [object-curly-newline](http://eslint.cn/docs/rules/object-curly-newline)

  ```js
  // ✓ ok
  let a = {};
  let b = {foo: 1};
  let c = {foo: 1, bar: 2};
  let d = {
      foo: 1,
      bar: 2
  };
  let e = {
      foo: function() {
          dosomething();
      }
  };
  
  let {} = obj;
  let {f} = obj;
  let {g, h} = obj;
  let {
      i,
      j
  } = obj;
  let {
      k = function() {
          dosomething();
      }
  } = obj;
  
  // ✗ avoid
  let a = {
  };
  let b = {
      foo: 1
  };
  let c = {
      foo: 1, bar: 2
  };
  let d = {foo: 1,
      bar: 2};
  let e = {foo: function() {
      dosomething();
  }};
  
  let {
  } = obj;
  let {
      f
  } = obj;
  let {
      g, h
  } = obj;
  let {i,
      j} = obj;
  let {k = function() {
      dosomething();
  }} = obj;
  ```

- **要求每个作用域有多个变量声明**

  eslint: [one-var](http://eslint.cn/docs/rules/one-var)

  ```js
  // ✓ ok
  function foo() {
      var bar;
      var baz;
  }
  
  // ✗ avoid
  function foo(){
      let bar = true,
          baz = false;
  }
  ```

- **不强制每个变量初始化语句换行**

  eslint: [one-var-declaration-per-line](http://eslint.cn/docs/rules/one-var-declaration-per-line)

  ```js
  // ✓ ok
  var a, b;
  
  let a,
      b;
  
  let a,
      b = 0;
  
  // ✓ ok
  var a, b, c = 0;
  
  let a,
      b = 0, c;
  ```

  

- **要求尽可能地简化赋值操作**

  eslint: [ operator-assignment](http://eslint.cn/docs/rules/operator-assignment)

  ```js
  // ✓ ok
  x += y;
  x = y * z;
  x = (x * y) * z;
  x[0] /= y;
  x[foo()] = x[foo()] % 2;
  x = y + x;
  
  // ✗ avoid
  x = x + y;
  x = y * x;
  x[0] = x[0] / y;
  x.y = x.y << z;
  ```

- **要求把换行符放在操作符前面**

  eslint: [operator-linebreak](http://eslint.cn/docs/rules/operator-linebreak)

  ```js
  // ✓ ok
  foo = 1 + 2;
  
  foo = 1
      + 2;
  
  foo
      = 5;
  
  // ✗ avoid
  foo = 1 +
        2;
  
  foo =
      5;
  ```

- **不强制块语句和类的开始或末尾有空行**

  eslint: [padded-blocks](http://eslint.cn/docs/rules/padded-blocks)

  ```js
  // ✓ ok
  if (a) {
      b();
  }
  
  if (a) {
  
      b();
  
  }
  
  if (a) {
      b();
  
  }
  ```

- **不强制在语句间填充空行**

  eslint: [padding-line-between-statements](http://eslint.cn/docs/rules/padding-line-between-statements)

  ```js
  // ✓ ok
  function foo() {
      bar();
  
      return;
  }
  
  // ✓ ok
  function foo() {
      bar();
      return;
  }
  ```

- **按具体需要决定是否对对象字面量属性名称使用引号**

  eslint: [quote-props](http://eslint.cn/docs/rules/quote-props)

  ```js
  // ✓ ok
  var object1 = {
      "a-b": 0,
      "0x0": 0,
      "1e2": 0
  };
  
  var object3 = {
      foo() {
          return;
      }
  };
  
  // ✗ avoid
  var object = {
      "a": 0,
      "b": 0,
  };
  ```

- **要求尽可能地使用单引号**

  eslint: [quotes](http://eslint.cn/docs/rules/quotes)

  ```js
  // ✓ ok
  var single = 'single';
  var backtick = `back${x}tick`;
  
  // ✗ avoid
  var double = "double";
  var unescaped = "a string containing 'single' quotes";
  ```

- **不强制使用 JSDoc 注释**

  eslint: [require-jsdoc](http://eslint.cn/docs/rules/require-jsdoc)

  ```js
  // ✓ ok
  function foo() {
      return 10;
  }
  
  /**
   * It returns 10
   */
  function foo() {
      return 10;
  }
  ```

- **要求在语句末尾使用分号**

  eslint: [semi](http://eslint.cn/docs/rules/semi)

  ```js
  // ✓ ok
  var name = "ESLint";
  
  object.method = function() {
      // ...
  };
  
  // ✗ avoid
  var name = "ESLint"
  
  object.method = function() {
      // ...
  }
  ```

- **强制分号之前无空格，分号之后有空格**

  eslint: [semi-spacing](http://eslint.cn/docs/rules/semi-spacing)

  ```js
  // ✓ ok
  var foo;
  var foo; var bar;
  throw new Error("error");
  for (i = 0; i < 10; i++) {}
  
  // ✗ avoid
  var foo ;
  var foo;var bar;
  throw new Error("error") ;
  for (i = 0 ; i < 10 ; i++) {}
  for (i = 0;i < 10;i++) {}
  ```

- **强制分号出现在句子末尾**

  eslint: [semi-style](http://eslint.cn/docs/rules/semi-style)

  ```js
  // ✓ ok
  var a = 1;
  var b = 3;
  
  // ✗ avoid
  var a = 1
  ;var b = 2
  ;var c = 3
  ;
  ```

- **不要求对象属性按序排列**

  eslint: [sort-keys](http://eslint.cn/docs/rules/sort-keys)

  ```js
  // ✓ ok
  let obj = {a: 1, c: 3, b: 2};
  let obj = {a: 1, b: 2, c: 3};
  ```

- **要求同一个声明块中的变量按顺序排列，忽略大小写**

  eslint: [sort-vars](http://eslint.cn/docs/rules/sort-vars)

  ```js
  // ✓ ok
  var a, b;
  
  var a, A;
  
  var a, B, c;
  
  // ✗ avoid
  var b, a;
  
  var c, a, B;
  ```

- **块语句必须总是至少有一个前置空格**

  eslint: [space-before-blocks](http://eslint.cn/docs/rules/space-before-blocks)

  ```js
  // ✓ ok
  if (a) {
      b();
  }
  
  // ✗ avoid
  if (a){
      b();
  }
  ```

- **强制在 function的左括号之前使用空格**

  eslint: [ space-before-function-paren](http://eslint.cn/docs/rules/space-before-function-paren)\

  ```js
  // ✓ ok
  function foo() {
      // ...
  }
  
  // ✗ avoid
  function foo () {
      // ...
  }
  ```

- **强制在圆括号内使用一致的空格**

  eslint: [ space-in-parens](http://eslint.cn/docs/rules/space-in-parens)

  ```js
  // ✓ ok
  foo();
  
  foo('bar');
  
  var foo = (1 + 2) * 3;
  (function () { return 'bar'; }());
  
  // ✗ avoid
  foo( 'bar');
  foo('bar' );
  foo( 'bar' );
  
  var foo = ( 1 + 2 ) * 3;
  ( function () { return 'bar'; }() );
  ```

- **中缀运算符周围有空格**

  eslint: [space-infix-ops](http://eslint.cn/docs/rules/space-infix-ops)

  ```js
  // ✓ ok
  a + b
  
  // ✗ avoid
  a+b
  
  a+ b
  
  a +b
  ```

- **注释块 // 或 /* 必须跟随至少一个空白**

  eslint: [spaced-comment](http://eslint.cn/docs/rules/spaced-comment)

  ```js
  // ✓ ok
  /* 注释 */
  
  // 注释
  
  /* 注释 */
  
  /*
   * 注释
   */
  
  /*
  注释
  */
  
  // ✗ avoid
  /*注释*/
  
  //注释
  
  ```

- **要求switch case冒号之后有一个空格**。

  eslint: [switch-colon-spacing](http://eslint.cn/docs/rules/switch-colon-spacing)

  ```js
  // ✓ ok
  switch (a) {
      case 0: foo(); break;
      case 1:
          bar();
          break;
      default:
          baz();
          break;
  }
  
  // ✗ avoid
  switch (a) {
      case 0 :break;
      default :foo();
  }
  ```

- **禁止在模板标记和它们的字面量之间有空格**

  eslint: [template-tag-spacing](http://eslint.cn/docs/rules/template-tag-spacing)

  ```js
  // ✓ ok
  func`Hello world`;
  
  // ✗ avoid
  func `Hello world`;
  ```

- **不要求按 Unicode 字节顺序标记 (BOM)**

  eslint: [unicode-bom](http://eslint.cn/docs/rules/unicode-bom)

  ```js
  // ✓ ok
  U+FEFF
  var abc;
  
  // ✓ ok
  var abc;
  ```

- **不要求正则表达式被括号括起来**

  eslint: [wrap-regex](http://eslint.cn/docs/rules/wrap-regex)

  ```js
  // ✓ ok
  function a() {
      return (/foo/).test("bar");
  }
  
  // ✓ ok
  function a() {
      return /foo/.test("bar");
  }
  ```

### ECMAScript 6

- **当箭头函数大括号是可以省略的，强制不使用它们** 

  eslint: [arrow-body-style](http://eslint.cn/docs/rules/arrow-body-style)

  ```js
  // ✓ ok
  let foo = () => 0;
  let foo = (retv, name) => {
      retv[name] = true;
      return retv;
  };
  
  // ✗ avoid
  let foo = () => {
      return 0;
  };
  ```

- **要求箭头函数在所有情况下使用圆括号将参数括起来**

  eslint: [arrow-parens](http://eslint.cn/docs/rules/arrow-parens)

  ```js
  // ✓ ok
  () => {};
  (a) => {};
  (a) => a;
  (a) => {'\n'}
  
  // ✗ avoid
  a => {};
  a => a;
  a => {'\n'}
  ```

- **规范化箭头函数的箭头(=>)之前或之后的空格**

  eslint: [arrow-spacing](http://eslint.cn/docs/rules/arrow-spacing)

  ```js
  // ✓ ok
  () => {};
  (a) => {};
  () => {'\n'};
  
  // ✗ avoid
  () =>{};
  (a) => {};
  ()=> {'\n'};
  ```

- **派生类中的构造函数必须调用 super()。非派生类的构造函数不能调用 super()**

  eslint: [constructor-super](http://eslint.cn/docs/rules/constructor-super)

  ```js
  // ✓ ok
  class A extends B {
      constructor() {
          super();
      }
  }
  
  // ✗ avoid
  class A {
      constructor() {
          super();
      }
  }
  
  class A extends B {
      constructor() { }
  }
  ```

- **强制在 * 和 function 关键字之间有空格，* 之后无空格**

  eslint:  [generator-star-spacing](http://eslint.cn/docs/rules/generator-star-spacing)

  ```js
  // ✓ ok
  function *generator() {
      yield "44";
      yield "55";
  }
  
  // ✗ avoid
  function * generator() {
      yield "44";
      yield "55";
  }
  ```

- **禁止修改class 声明的变量**

  eslint: [no-class-assign](http://eslint.cn/docs/rules/no-class-assign)

  ```js
  // ✓ ok
  let A = class A { }
  A = 0; // A is a variable.
  
  // ✗ avoid
  A = 0;
  class A { }
  ```

- **禁止在可能与比较操作符相混淆的地方使用箭头函数**

  eslint: [no-confusing-arrow](http://eslint.cn/docs/rules/no-confusing-arrow)

  ```js
  // ✓ ok
  var x = (a) => (1 ? 2 : 3);
  var x = (a) => { return 1 ? 2 : 3; };
  
  // ✗ avoid
  var x = (a) => 1 ? 2 : 3;
  var x = (a) => (1 ? 2 : 3);
  ```

- **禁止修改 const 声明的变量**

  eslint: [no-const-assign](http://eslint.cn/docs/rules/no-const-assign)

  ```js
  // ✓ ok
  const a = 0;
  console.log(a);
  
  // ✗ avoid
  const a = 0;
  ++a;
  ```

- **禁止类成员中出现重复的名称**

  eslint: [no-dupe-class-members](http://eslint.cn/docs/rules/no-dupe-class-members)

  ```js
  // ✓ ok
  class Foo {
    bar() { }
    qux() { }
  }
  
  class Foo {
    get bar() { }
    set bar(value) { }
  }
  
  class Foo {
    static bar() { }
    bar() { }
  }
  
  // ✗ avoid
  class Foo {
    bar() { }
    bar() { }
  }
  
  class Foo {
    bar() { }
    get bar() { }
  }
  
  class Foo {
    static bar() { }
    static bar() { }
  }
  ```

- **禁止重复模块导入**

  eslint: [no-duplicate-imports](http://eslint.cn/docs/rules/no-duplicate-imports)

  ```js
  // ✓ ok
  import { merge, find } from 'module';
  import something from 'another-module';
  
  // ✗ avoid
  import { merge } from 'module';
  import something from 'another-module';
  import { find } from 'module';
  ```

- **禁止在构造函数中，在调用 super() 之前使用 this 或 super**

  eslint: [no-this-before-super](http://eslint.cn/docs/rules/no-this-before-super)

  ```js
  // ✓ ok
  class A extends B {
      constructor() {
          super();
          this.a = 0;
      }
  }
  
  // ✗ avoid
  class A extends B {
      constructor() {
          this.foo();
          super();
      }
  }
  ```

- **禁止使用不必要的计算属性**

  eslint: [no-useless-computed-key](http://eslint.cn/docs/rules/no-useless-computed-key)

  ```js
  // ✓ ok
  var c = { 'a': 0 };
  var c = { 0: 0 };
  var a = { x() {} };
  var c = { a: 0 };
  var c = { '0+1,234': 0 };
  
  // ✗ avoid
  var a = { ['0']: 0 };
  var a = { ['0+1,234']: 0 };
  var a = { [0]: 0 };
  var a = { ['x']: 0 };
  var a = { ['x']() {} };
  ```

- **禁止在 import 和 export 和解构赋值时将引用重命名为相同的名字**

  eslint: [no-useless-rename](http://eslint.cn/docs/rules/no-useless-rename)

  ```js
  // ✓ ok
  import * as foo from "foo";
  import { foo } from "bar";
  import { foo as bar } from "baz";
  
  export { foo };
  export { foo as bar };
  export { foo as bar } from "foo";
  
  let { foo } = bar;
  let { foo: bar } = baz;
  let { [foo]: foo } = bar;
  
  // ✗ avoid
  import { foo as foo } from "bar";
  export { foo as foo };
  export { foo as foo } from "bar";
  let { foo: foo } = bar;
  let { 'foo': foo } = bar;
  ```

- **阻止 var 的使用，推荐使用 const 或 let**

  eslint: [no-var](http://eslint.cn/docs/rules/no-var)

  ```js
  // ✓ ok
  let x = "y";
  const CONFIG = {};
  
  // ✗ avoid
  var x = "y";
  var CONFIG = {};
  ```

- **不要求对象字面量中方法和属性使用简写语法**

  eslint: [object-shorthand](http://eslint.cn/docs/rules/object-shorthand)

  ```js
  // ✓ ok
  var foo = {
      x: x,
      y: y,
      z: z,
  };
  
  var foo = {
      a: function() {},
      b: function() {}
  };
  
  // ✓ ok
  var foo = {x, y, z};
  
  var foo = {
      a() {},
      b() {}
  };
  ```

- **要求回调函数使用箭头函数**

  eslint: [prefer-arrow-callback](http://eslint.cn/docs/rules/prefer-arrow-callback)

  ```js
  // ✓ ok
  foo((a) => a);
  
  // ✗ avoid
  foo(function(a) { return a; });
  ```

- **要求使用 const 声明那些声明后不再被修改的变量**

  eslint: [ prefer-const](http://eslint.cn/docs/rules/prefer-const)

  ```js
  // ✓ ok
  const a = 0;		// 不变的量
  console.log(a);
  
  // ✗ avoid
  let a;			
  a = 0;		// 不再改变
  ```

- **不要求优先使用数组解构和对象解构**

  eslint: [prefer-destructuring](http://eslint.cn/docs/rules/prefer-destructuring)

  ```js
  // ✓ ok
  var [ foo ] = array;
  var { foo } = object;
  
  // ✓ ok
  var foo = array[someIndex];
  var foo = object.foo;
  ```

- **不禁用 `parseInt()` 和 `Number.parseInt()`**

  eslint: [prefer-numeric-literals](http://eslint.cn/docs/rules/prefer-numeric-literals)

  ```js
  // ✓ ok
  parseInt("111110111", 2) === 503;
  parseInt("767", 8) === 503;
  parseInt("1F7", 16) === 503;
  Number.parseInt("111110111", 2) === 503;
  Number.parseInt("767", 8) === 503;
  Number.parseInt("1F7", 16) === 503;
  
  // ✓ ok
  0b111110111 === 503;
  0o767 === 503;
  0x1F7 === 503;
  ```

- **要求使用剩余参数而不是 arguments**

  eslint: [prefer-rest-params](http://eslint.cn/docs/rules/prefer-rest-params)

  ```js
  // ✓ ok
  function foo(...args) {
      console.log(args);
  }
  
  // ✗ avoid
  function foo() {
      console.log(arguments);
  }
  ```

- **要求使用扩展运算符而非 .apply()**

  eslint: [ prefer-spread](http://eslint.cn/docs/rules/prefer-spread)

  ```js
  // ✓ ok
  foo(...args);
  
  // ✗ avoid
  foo.apply(null, args);
  ```

- **要求使用模板字面量而非字符串连接**

  eslint: [prefer-template](http://eslint.cn/docs/rules/prefer-template)

  ```js
  // ✓ ok
  var str = `Hello, ${name}!`;
  var str = `Time: ${12 * 60 * 60 * 1000}`;
  
  // ✗ avoid
  var str = "Hello, " + name + "!";
  var str = "Time: " + (12 * 60 * 60 * 1000);
  ```

- **要求 generator 函数内有 yield**

  eslint: [require-yield](http://eslint.cn/docs/rules/require-yield)

  ```js
  // ✓ ok
  function* foo() {
    yield 5;
    return 10;
  }
  
  // ✗ avoid
  function* foo() {
    return 10;
  }
  ```

- **强制剩余和扩展运算符及其表达式之间无空格**

  eslint: [rest-spread-spacing](http://eslint.cn/docs/rules/rest-spread-spacing)

  ```js
  // ✓ ok
  let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
  
  // ✗ avoid
  fn(... args)
  [... arr, 4, 5, 6]
  ```

- **不强制模块内的 import 排序**

  eslint: [ sort-imports](http://eslint.cn/docs/rules/sort-imports)

  ```js
  // ✓ ok
  import b from 'foo.js';
  import a from 'bar.js';
  
  // ✓ ok
  import a from 'baz.js';
  import b from 'qux.js';
  ```

- **不要求 symbol 描述**

  eslint: [symbol-description](http://eslint.cn/docs/rules/symbol-description)

  ```js
  // ✓ ok
  var foo = Symbol("some description");
  
  // ✓ ok
  var foo = Symbol();
  ```

- **禁止模板字符串中的嵌入表达式周围空格的使用**

  eslint: [template-curly-spacing](http://eslint.cn/docs/rules/template-curly-spacing)

  ```js
  // ✓ ok
  `hello, ${people.name}!`;
  
  `hello, ${
      people.name
  }!`;
  
  // ✗ avoid
  `hello, ${ people.name}!`;
  `hello, ${people.name }!`;
  
  `hello, ${ people.name }!`;
  ```

- **强制在 yield* 表达式中 * 之前使用空格**

  eslint: [yield-star-spacing](http://eslint.cn/docs/rules/yield-star-spacing)

  ```js
  // ✓ ok
  function *generator() {
    yield *other();
  }
  
  // ✗ avoid
  function *generator() {
    yield* other();
  }
  ```

### Vue

- **强制 .vue 文件里的template内缩进为4个空格**

  eslint-plugin-vue: [vue/html-indent](https://eslint.vuejs.org/rules/html-indent.html)

  ```vue
  <!-- ✓ ok -->
  <template>
      <div class="foo">
        	Hello.
      </div>
  </template>
  
  <!-- ✗ avoid -->
  <template>
  	<div class="foo">
    	Hello.
    </div>
  </template>
  ```

- **强制 .vue 文件里的template内每行最多一个属性**

  eslint-plugin-vue: [vue/max-attributes-per-line](https://eslint.vuejs.org/rules/max-attributes-per-line.html)

  ```vue
  <template>
    <!-- ✓ ok -->
    <MyComponent lorem="1"/>
    <MyComponent
      lorem="1"
      ipsum="2"
    />
    <MyComponent
      lorem="1"
      ipsum="2"
      dolor="3"
    />
  
    <!-- ✗ avoid -->
    <MyComponent lorem="1" ipsum="2"/>
    <MyComponent
      lorem="1" ipsum="2"
    />
    <MyComponent
      lorem="1" ipsum="2"
      dolor="3"
    />
  </template>
  ```

- **不要求.vue文件内元素的内容前后断行**

  eslint-plugin-vue: [vue/singleline-html-element-content-newline](https://eslint.vuejs.org/rules/singleline-html-element-content-newline.html)

  ```vue
  <template>
    <!-- ✓ ok -->
    <div attr>
      content
    </div>
    
    <tr attr>
      <td>
        {{ data1 }}
      </td>
      <td>
        {{ data2 }}
      </td>
    </tr>
    
    <div attr>
      <!-- comment -->
    </div>
    
    <!-- ✗ ok -->
    <div attr>content</div>
    
    <tr attr><td>{{ data1 }}</td><td>{{ data2 }}</td></tr>
    
    <div attr><!-- comment --></div>
  </template>
  ```

- **要求.vue文件内开始标签和结束标签的结尾符号前没有空格**

  eslint-plugin-vue: [vue/html-closing-bracket-spacing](https://eslint.vuejs.org/rules/html-closing-bracket-spacing.html)

  ```vue
  <template>
    <!-- ✓ ok -->
    <div>
    <div foo>
    <div foo="bar">
    </div>
    <div />
    <div foo />
    <div foo="bar" />
  
    <!-- ✗ avoid -->
    <div >
    <div foo >
    <div foo="bar" >
    </div >
    <div/>
    <div foo/>
    <div foo="bar"/>
  </template>
  ```

- **强制组件标签、void标签（img等）、svg标签、math标签自我闭合**

  eslint-plugin-vue: [vue/html-self-closing](https://eslint.vuejs.org/rules/html-self-closing.html)

  ```vue
  <template>
    <!-- ✓ ok -->
    <div></div>
    <img/>
    <MyComponent/>
    <svg><path d=""/></svg>
  
    <!-- ✗ avoid -->
    <div/>
    <img>
    <MyComponent></MyComponent>
    <svg><path d=""></path></svg>
  </template>
  ```

  

