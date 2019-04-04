# 每日一题[（生活一题）](./life.md)

在这里记录着每天自己遇到的一道印象深刻的前端问题，以及一道生活中随处可见的小问题。

强迫自己形成积累的习惯，鞭挞自己不断前行，共同学习。

### **2019/04/01 - 2019/04/07**

- `__proto__`和 `prototype` 的区别 ？

  <details>
  <summary>点击</summary>

  1. 在 JS 里，万物皆对象。方法（Function）是对象，方法的原型(Function.prototype)是对象。因此，它们都会具有对象共有的特点。即：**对象具有属性`__proto__`，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。**

  2. 方法(Function)方法这个特殊的对象，除了和其他对象一样有上述 proto 属性之外，还有自己特有的属性——原型属性（prototype），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做 constructor，这个属性包含了一个指针，指回原构造函数。

  </details>

* 双精度浮点数是如何保存的 ?

  <details>
      <summary>点击</summary>

  在计算机中，浮点表示法，分为三大部分：

  第一部分用来存储符号位（sign），用来区分正负数，0 表示正数
  第二部分用来存储指数（exponent）
  第三部分用来存储小数（fraction）, 多出的末尾如果是 1 需要进位;

  双精度浮点数一共占据 64 位：

  符号位（sign）占用 1 位
  指数位（exponent）占用 11 位
  小数位（fraction）占用 52 位

  举个例子：0.1 的二进制为

  ```js
  0.00011001100110011001100110011001100110011001100110011001 10011...
  ```

  转化为 2 进制科学计数法

  ```js
  1.1001100110011001100110011001100110011001100110011001 * (2 ^ -4);
  ```

  也就是说 0.1 的：

  - 符号位为：0
  - 小数位为：1001100110011001100110011001100110011001100110011001
  - 指数位为：-4

  指数位为负数的怎么保存？为了减少不必要的麻烦，IEEE 规定了一个偏移量，对于指数部分，每次都加这个偏移量进行保存，这样即使指数是负数，那么加上这个偏移量也变为正数啦。为了使所有的负指数加上这个偏移量都能够变为正数，IEEE 规定 1023 为双精度的偏移量。

  因此指数部分为 -4 + 1023 = 1019， 转化成 11 位二进制为：01111111011

  因此 0.1 在内存中的保存为：

  ```js
    0 01111111011 1001100110011001100110011001100110011001100110011001
  ```

  </details>

- 如何找出字符串中出现最多的字母 （ababccdeajxac）?

  <details>
  <summary>点击</summary>

  最先想到的解法是用 map 纪录每个字符的次数，然后找出最多的即可：

  ```js
  function getMaxNumberOfChar(str) {
    return (str + "").split("").reduce(
      function(pre, cur, index, arr) {
        cur in pre ? pre[cur]++ : (pre[cur] = 1);
        pre[cur] > pre.value && ((pre.char = cur), (pre.value = pre[cur]));
        return pre;
      },
      { value: 0 }
    );
  }
  getMaxNumberOfChar("ababccdeajxac"); // Object {value: 4, a: 4, char: "a", b: 2, c: 3…}
  ```

  此外，可以考虑用正则来辅助处理：

  ```js
  function getMaxNumberOfChar(str) {
    return (str + "")
      .split("")
      .sort()
      .join("")
      .match(/(\w)\1*/g)
      .reduce(
        function(pre, cur) {
          return cur.length > pre.value
            ? { value: cur.length, char: cur[0] }
            : pre;
        },
        { value: 0 }
      );
  }
  getMaxNumberOfChar("ababccdeajxac"); // Object {value: 4, char: "a"}
  ```

  这里拓展一下 reduce 函数的用法

  ```js
  // reduce 函数
  // array.reduce(function(accumulator, currentValue, currentIndex, arr), initialValue)
  // reducer回调函数本身接受几个参数，第一个参数是 accumulator 累加器，第二个是数组中的 item，第三个参数是该项的索引，最后一个参数是原始数组的引用。
  // initialValue 为reduce初始值，否则视数组第一个值为初始值，选填
  const array1 = [1, 2, 3, 4];

  // 1 + 2 + 3 + 4
  console.log(
    array1.reduce((accumulator, currentValue) => {
      console.log(accumulator, currentValue);
      return accumulator + currentValue;
    })
  );
  ```

  </details>

### **2019/03/27 - 2019/03/31**

- 请问这句语句 `var args=[].slice.call(arguments,1)` 是什么意思？

  <details>
  <summary>点击</summary>

  先看原函数：

  ```js
  function a() {
    var args = [].slice.call(arguments, 1);
    console.log(args);
  }

  a("haha", 1, 2, 3, 4, 5); // log出[1, 2, 3, 4, 5]
  a("run", "-g", "-b"); // log出['-g', '-b']
  ```

  1. 首先，函数 call() 方法，第一个参数改变函数的 this 指向，后面剩余参数传入原函数 slice 中
  2. arguments 是什么？
     > arguments 是函数中的一个类数组的参数集合对象
     > 如： {'0': 'haha', '1': 1, '2': 2}
  3. slice 为数组可从已有的数组中返回选定的元素。
     > 此题为从 index = 1 往后
  4. 综上，这句语句的作用是——**将函数中的实参值转化成数组**
     </details>

- 连等赋值问题

  ```js
  var a = { n: 1 };
  var b = a;
  a.x = a = { n: 2 };
  // a ? b ? a.x ? 结果是什么？
  ```

    <details>
      <summary>点击</summary>

      我们可以先尝试交换下连等赋值顺序（a = a.x = {n: 2};），可以发现输出不变，即顺序不影响结果。

      那么现在来解释对象连等赋值的问题：按照 es5 规范，题中连等赋值等价于
      a.x = (a = { n: 2 });，按优先获取左引用（lref），然后获取右引用（rref）的顺序，a.x 和 a 中的 a 都指向了{ n: 1 }。至此，至关重要或者说最迷惑的一步明确。(a = {n: 2})执行完成后，变量 a 指向{n: 2}，**并返回{n: 2}**;接着执行 a.x = { n: 2 }，这里的 a 就是 b（指向{ n: 1 }），所以 b.x 就指向了{ n: 2 }。

      赋值时有返回该值， 如 `a = 4 // return 4` , 赋值变量 `let n = 5 //return undefinded`

      ```js
      a = { n: 2 };
      b = { n: 1, x: { n: 2 } };
      a.x = undefinded;
      ```

    </details>

- 如何手动实现一个 Promise ?

    <details>
    <summary>点击</summary>
    promise 的三种状态 pending, resolve, reject

  ```js
  function MyPromise(callback) {
    let that = this;
    //定义初始状态
    //Promise状态
    that.status = "pending";
    //value
    that.value = "undefined";
    //reason 是一个用于描述Promise被拒绝原因的值。
    that.reason = "undefined";
    //用来解决异步问题的数组
    that.onFullfilledArray = [];
    that.onRejectedArray = [];

    //定义resolve
    function resolve(value) {
      //当status为pending时，定义Javascript值，定义其状态为fulfilled
      if (that.status === "pending") {
        that.value = value;
        that.status = "resolved";
        that.onFullfilledArray.forEach(func => {
          func(that.value);
        });
      }
    }

    //定义reject
    function reject(reason) {
      //当status为pending时，定义reason值，定义其状态为rejected
      if (that.status === "pending") {
        that.reason = reason;
        that.status = "rejected";
        that.onRejectedArray.forEach(func => {
          func(that.reason);
        });
      }
    }

    //捕获callback是否报错
    try {
      callback(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }

  MyPromise.prototype.then = function(onFulfilled, onRejected) {
    let that = this;
    //需要修改下，解决异步问题，即当Promise调用resolve之后再调用then执行onFulfilled(that.value)。
    //用两个数组保存下onFulfilledArray
    if (that.status === "pending") {
      that.onFullfilledArray.push(value => {
        onFulfilled(value);
      });

      that.onRejectedArray.push(reason => {
        onRejected(reason);
      });
    }

    if (that.status === "resolved") {
      onFulfilled(that.value);
    }

    if (that.status === "rejected") {
      onRejected(that.reason);
    }
  };
  ```

  </details>

- AST（抽象语法树）？

  <details>
    <summary>点击</summary>

  **什么是 AST（抽象语法树）？**

  > It is a hierarchical program representation that presents source code structure according to the grammar of a programming language, each AST node corresponds to an item of a source code.
  >
  > 它是一种分层程序表示，它根据编程语言的语法呈现源代码结构，每个 AST 节点对应一个源代码项。

  Babel,Webpack，vue-cli 和 esLint 等很多的工具和库的核心都是通过 Abstract Syntax Tree (抽象语法树)这个概念来实现对代码的检查、分析等操作的。

  解析（parsing），转译（transforming），生成（generation）。

  将源码解析成 AST 抽象语法树，再对此语法树进行相应的转译，最后生成我们所需要的代码。

  第三方的生成 AST 库有很多，这里推荐几个——esprima, babylon(babel 使用)

  其转化的内容大致是这样的：

  ```json
  {
    "type": "Program",
    "start": 0,
    "end": 16,
    "body": [
      {
        "type": "FunctionDeclaration",
        "start": 0,
        "end": 16,
        "id": {
          "type": "Identifier",
          "start": 9,
          "end": 12,
          "name": "ast"
        },
        "expression": false,
        "generator": false,
        "params": [],
        "body": {
          "type": "BlockStatement",
          "start": 14,
          "end": 16,
          "body": []
        }
      }
    ],
    "sourceType": "module"
  }
  ```

  **AST 的使用场景**

  - 代码语法的检查、代码风格的检查、代码的格式化、代码的高亮、代码错误提示、代码自动补全等等.
  - 代码混淆压缩
  - 优化变更代码，改变代码结构使达到想要的结构
    </details>

* 为什么 `0.1 + 0.2 !== 0.3` ?

    <details>
      <summary>点击</summary>

  ### IEEE-754 精度问题

  所有使用 IEEE-754 数字实现的编程语言都有这个问题。

  0.1 和 0.2 的二进制浮点数表示并不是精确的，所以相加后不等于 0.3。这个相加的结果接近 0.30000000000000004。

  首先将 0.1 转化为 2 进制

  ```js
  // 0.1  十进制 -> 二进制
  0.1 * 2 = 0.2  取0
  0.2 * 2 = 0.4  取0
  0.4 * 2 = 0.8  取0
  0.8 * 2 = 1.6  取1
  0.6 * 2 = 1.2  取1
  0.2 * 2 = 0.4  取0
  0.4 * 2 = 0.8  取0
  0.8 * 2 = 1.6  取1
  0.6 * 2 = 1.2  取1
  //0.000110011(0011)`   0.1二进制(0011)里面的数字表示循环

  ```

  你会发现 0.1 转二级制会一直无线循环下去，根本算不出一个正确的二进制数。

  所以我们得出 0.1 = 0.000110011(0011)，那么 0.2 的演算也基本如上所示，所以得出 0.2 = 0.00110011(0011)

  六十四位中符号位占一位，整数位占十一位，其余五十二位都为小数位。因为 0.1 和 0.2 都是无限循环的二进制了，所以在小数位末尾处需要判断是否进位（就和十进制的四舍五入一样）

  那么把这两个二进制加起来会得出 0.010011....0100 , 这个值算成十进制就是 0.30000000000000004

    </details>

![](https://raw.githubusercontent.com/zxpsuper/daily-question/master/image/fork_and_star.jpg)
