# JavaScript Note 麥可筆記

> 整理並重新理解 javascript 運作原理


[筆記之後改使用isuue來記錄](https://github.com/Mike-Zheng/javascript-note/issues)

## 前言

靠 js 吃飯多年，每次開發都會有發現不知或不足的地方，於是開一個筆記紀錄每次的資訊。

---

### Table of Contents

| No. | Content                                                       |
| --- | ------------------------------------------------------------- |
| 1   | [Execution Context 執行環境](#execution-context-執行環境)     |
| 2   | [Lexical Environment 詞法環境](#lexical-environment-詞法環境) |
| 3   | [Hoisting 提升](#hoisting-提升)                               |

---

1. ### Execution Context 執行環境

   > 關鍵字: Execution Context 執行環境 執行上下文

   **什麼是 Execution Context**

   `Execution Context` 就是執行 javascript code 時候的環境，用於跟 javascript code 的運算時求值等，任何 javascript code 執行的時候都是處於一個 `Execution Context` 之中。

   `Execution Stack` 負責 `Execution Context` 創建後的執行順序。當 javascript code 運行會將創建後的`Execution Stack` 層層疊入，並順著作用域鏈 `scope chain` 訪問變量、如果內部有 invoke 其他函數就創建一個新的 `Execution Context` ，push 在原先的之上並把控制權交出。

   ![execution_context.png](./images/execution_context.png)

   **Execution Stack**

   - `Execution Stack` 為 `Execution Context` 執行順序的 stack，會將建立階段的 `Execution Context` 依順序 push & pop ，其順序為 FILO(First In, Last Out)。
   - 當引擎執行你的 javascript code 時，會先建立一個 `Gobal Execution Context` 並且把他 push 進 `Execution Stack`。
   - 而當執行到 function 時，會建立 `Functional Execution Context` ，如果有很多 function，就會一層疊一層的方式 push 進去。
   - call stack: 每次從 `Execution Stack` 最上方 pop 一個 `Execution Context` 並執行稱之為 `call stack` 。

   以下範例解釋 Execution Stack 流程

   ```javascript
   let a = "Hello World!";
   function first() {
     console.log("Inside first function");
     second();
     console.log("Again inside first function");
   }
   function second() {
     console.log("Inside second function");
   }
   first();
   console.log("Inside Global Execution Context");
   ```

   ![execution_context_stack.png](./images/execution_context_stack.png)

   > An Execution Context Stack for the above code.

   ```bash
   Inside first function
   Inside second function
   Again inside first function
   Inside Global Execution Context
   ```

   - 上面 JavaScript code 在瀏覽器中執行(invoke)時，JavaScript 引擎會先創建一個 `Gobal Execution Context` 並把它 push 進 `Execution Stack` 中。碰到 first() 執行時，引擎給這個函數創建一個新的 `Execution Context` ，然後把它 push 進 `Execution Stack` 的頂部。

   - 當 second() 在 first() 函數內部執行時，引擎會給 second 創建 `Execution Context` 並把它 push 進 `Execution Stack` 頂部，當 second 函數執行完畢，它的 `Execution Context` 就會從 `Execution Stack` 最上方 pop，指針會指向它下面的 `Execution Context` ，也就是 first 函數的 `Execution Context`。

   - 當 first 函數執行完畢其 `Execution Context` 也會從最上方 pop，指針就指向了 `Gobal Execution Context` 。當所有的代碼執行完畢，引擎會把 `Gobal Execution Context` 也從 `Execution Stack` 中移出。

   **Execution Context 的種類**

   Execution Context 一共有三種:

   1. Gobal Execution Context:

   - 預設或是基本的 `Execution Context`。
   - 在一個程序(program)中只會有一個 `Gobal Execution Context`。
   - 建立
     - `global object` (在瀏覽器是 `window` )。
     - `this`，並把 `this` 指向 `global object` 。

   2. Functional Execution Context:

   - 執行 function 的時候，會創立一個新的 `Functional Execution Context` ，每一個 function 執行都會有自己的 `Execution Context` 。
   - 相同的 function code 在不同的執行階段會建立各自的 `Execution Context`。

   3. Eval:

   - 在 eval 函數中執行的 javascript code 也會有自己的 `Execution Context` ，但由於 eval 因為安全因素已經不常使用。

   **Execution Context 階段**

   `Execution Context`有兩個階段:

   1. 建立階段 `The Creation Phase`
   2. 執行階段 `The Execution Phase`

   - 建立階段 `The Creation Phase`

     `Execution Context`的創建階段，發生在 function invoke 時且在執行函數內的 code 之前，在創建階段 js 引擎會做如下操作:

     - 創建 詞法環境( `Lexical Environment` ) 與 變量環境( `Variable Environment` )並 `Hoisting`
       - `Hoisting` : 在建立階段預先將變數分配記憶體空間並預設賦值為 undefined
     - 建立`this`，建立全域物件 `global object`
       - binding `this` ，而 `global object` 只有在 ` Gobal Execution Context` 的時候才會建立。
     - 建立 `scope chain` 與外部環境 `Outer Environment`
       - 對於 `Gobal Execution Context` 而言，其 `Outer Environment` 為 null，對於 `Functional Execution Context` 而言，如果 function b 包在 function a 裡面，那 function b 的外部環境就是 function a

   - 執行階段 `The Execution Phase`

     在此階段 invoke JavaScript 並 pop 出 `Execution Stack`。

   **reference**

   - [JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
   - [Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)

   **[🔝 Back to Top](#table-of-contents)**

2. ### Lexical Environment 詞法環境

   > 關鍵字: Lexical Environment 詞法環境

   **Lexical Environment** 詞法環境:

   - `Lexical Environment` : 在 code 中真實存在的位置以及周圍的內容。 在 JavaScript 中它的位置及順序。

   - 基本上來說，function 內的 `{...}` 即為一個 `scope`，`Lexical Environment`會依照其內外環境創造內容，並存入`Execution Context`之中。

     ```javascript
     function helloWorld() {
       let text = "hello world!";
     }
     ```

   - 每當 function 被呼叫前的創立階段時，都會產生一組新的語彙環境 (`Lexical Environment`)，因此 function 的作用域與其相關環境變數在 **`Execution Context` 創立的時候** 就已經決定。
   - 當 `Execution Stack` 執行到此 function 的 `Execution Context`時，也就是 invoke 此 function 時，會依照**已建立**的`Lexical Environment` 環境執行其內容。

   **Variable Environment** 變量環境:

   - 在 `ES6` 前，宣告變數都是通過 var 關鍵詞宣告的，在 `ES6` 中則提倡使用 `let` 和 `const` 來聲明變量，為了兼容 var 的寫法，於是使用 `Variable Environment` 變量環境來存儲 var 聲明的變量。
   - 為特化的`Lexical Environment`，其內只存變數`var`。

   **Lexical Environment** 的組成

   - `Environment Record` 環境記錄器:
     - `Environment Record`為存放 變數 與 function 宣告 的地方
     - `Environment Record` 分為兩種:
       - `Declarative environment record` **聲明式環境記錄器** 存儲變數、函數和參數。
       - `Object environment record` **對象環境記錄器** 用來定義出現在`Execution Context`中的變量和函數的關係。
   - `this`
   - `scope chain` (outer)

   **note**

   - 在全局環境`Golbal Execution Context`中，環境記錄器 `Environment Record` 是 **對象環境記錄器 `Object environment record`**。
   - 在函數環境`Functional Execution Context`中，環境記錄器 `Environment Record` 是 **聲明式環境記錄器 `Declarative environment record`**。
   - 對於函數環境 `Functional Execution Context`， 聲明式環境記錄器還包含了一個傳遞給函數的 arguments 對象和傳遞給函數的參數的 length。

   **`Lexical Environment` in `Execution Context`**

   ```javascript
   GlobalExecutionContext = {
     LexicalEnvironment: {
       EnvironmentRecord: {
         Type: "Object",
         // Identifier bindings go here
       }
       outer: <null>,
       this: <global object>
     },
      VariableEnvironment: {
       EnvironmentRecord: {
         Type: "Object",
         var: undefined,
       }
       outer: <null>
     }
   }
   ```

   ```javascript
   FunctionExecutionContext = {
     LexicalEnvironment: {
       EnvironmentRecord: {
         Type: "Declarative",
         // Identifier bindings go here
       }
       outer: <Global or outer function environment reference>,
       this: <depends on how function is called>
     },
     VariableEnvironment: {
       EnvironmentRecord: {
         Type: "Declarative",
         var: undefined
       },
       outer: <GlobalLexicalEnvironment>
     }
   }
   ```

   **reference**

   - [JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
   - [Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)
   - [重学 js —— Lexical Environments（词法环境）和 Environment Records（环境记录）](https://github.com/lizhongzhen11/lizz-blog/issues/49)

   **[🔝 Back to Top](#table-of-contents)**

3. ### Hoisting 提升

   > 關鍵字: Hoisting 提升

   **Hoisting** 提升

   基本概念:

   - 在`Execution Context` 建立階段預先將其內部`variables` 與 `functions`分配記憶體空間。
   - `variables`預設賦值為 undefined 。

   ```javascript
   console.log(a); //undefined
   var a = "Hello World!";
   ```

   以上程式碼 a 會被提升 `Hoisting`
   因此會執行的結果會同

   ```javascript
   var a; //undefined
   console.log(a);
   a = "Hello World!";
   ```

   **TDZ** 暫時死區 (Temporal Dead Zone)
   概念:

   - ES6 之後加入的 `let`/ `const`，避免宣告前使用該變數，`hoisting` 之後暫時存放的位置，以利後續的警告。
   - 如果在宣告`let`/ `const`之前使用變數，存在「暫時死區」無法存取，使用它就會報錯 `ReferenceError`。

   **Hoisting in closure**

   - Case: 利用一個 for 迴圈每隔一秒印出一個數字:

     ```javascript
     for (var i = 0; i < 3; i++) {
       setTimeout(() => {
         console.log(i);
       }, 1000);
     }
     ```

     這是錯誤寫法，因為根據 hoisting 的原則，i 會是一個 global 變數。

     其變數 i 為同一個 environment record 的紀錄，因此 for 會更改其值，上面的程式碼等同於:

     ```javascript
     var i;
     for (i = 0; i < 3; i++) {
       setTimeout(() => {
         console.log(i);
       }, 1000);
     }
     ```

     - 這個問題可以這樣做

       **方法 1** 使用 `IIFE`

       捕捉環境的變數建立新的`Functional Execution Context`

       ```javascript
       for (var i = 0; i < 3; i++) {
         (function (j) {
           setTimeout(() => {
             console.log(j);
           }, j * 1000);
         })(i);
       }
       ```

       使用一個 `IIFE` ，它接受一個變數 j 當作參數，緊接著我們立刻將 i 傳進去當作參數呼叫它。每一次 `IIFE` 都產生了一個區域變數 j，值分別是 0, 1, 2。

       **方法 2** 使用 ES6 `let`/ `const`

       ```javascript
       for (let i = 0; i < 3; i++) {
         setTimeout(() => {
           console.log(i);
         }, i * 1000);
       }
       ```

   **reference**

   - [JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
   - [[教學] JavaScript 中的 Hoisting 是什麼意思？let const var 的差異居然是這個？](https://shubo.io/javascript-hoisting/)

   **[🔝 Back to Top](#table-of-contents)**

4. ### this

   > 關鍵字: this

   **this**

   基本概念:

   - this 就是一個指針，指向我們 invoke function 的對象。
   - this 的指向的是目前呼叫 function 或方法的擁有者(owner)物件，也就是說它與 function 如何被呼叫或調用有關，雖然是同一 function 的呼叫，因為不同的物件呼叫，也有可能是不同的 this 值。
   - this 的值跟`scope`作用域跟程式碼的位置在哪裡完全無關，只跟**你如何呼叫**有關

   ```javascript
   var obj = {
     func1: function () {
       console.log(this);
     },
   };

   function func2() {
     console.log(this);
   }

   obj.func1();
   func2();
   ```

   在 JavaScript 中我們都是從左讀到右，解釋一下範例中的 this

   - 執行 obj.func1()時的 this 指向 obj

   - 執行 func2()時的 this 指向 window

   **arrow function** 箭頭函式

   ```javascript
   var obj2 = {
     func: () => {
       console.log(this);
     },
   };
   obj2.func(); // window
   // 非嚴格模式下
   ```

   箭頭函數按`Lexical Environment`詞法環境，來綁定它的環境，所以 this 實際上會引用到原來的環境。

   Note: Lexical Environment 詞法環境:在你在寫 code 的地方將變量決定變數的值

   也就是說，當我們 invoke 此時 obj2.func()時，該箭頭函數其實在定義好 obj2 對象時就確定了它的 Lexical Environment 詞法環境。

   - call(呼叫): 以個別提供的 this 值與傳入參數值來呼叫 function。
   - bind(綁定): 建立一個新的 function，這個新 function 在呼叫時，會以提供的 this 值與一連串的傳入參數值來進行呼叫。
   - apply(應用): 與 call 方法功能一樣，只是除了 this 值傳入外，另一個傳入參數值使用陣列。

   https://github.com/xitu/gold-miner/blob/master/TODO1/mastering-javascript-this-keyword-detailed-guide.md

   **reference**

   - [JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
   - [[教學] JavaScript 中的 Hoisting 是什麼意思？let const var 的差異居然是這個？](https://shubo.io/javascript-hoisting/)

   **[🔝 Back to Top](#table-of-contents)**

5. ### closure

   基本概念:

   - 外層函數執行完消逝，但內部函數卻依然保留了已消逝的外部環境變量，使得在執行內部函數時，完整記錄了他的值

   詞法作用域(js)

   ```js
   function start() {
   alert(args); // 真正執行的作用域
   }
   function server() {
   var args = “parameter here.”;
   start(); // 這裡只是調用點
   }
   server(); // ReferenceError: args is not defined

   ```

   動態作用域(其他語言)

   ```js
   function start() {
   alert(args);
   }
   function server() {
   var args = “parameter here.”;
   start(); // 在呼叫函數時決定可訪問的變數
   }
   server(); // parameter here.

   ```

   **reference**

   - [無法理解的-js-閉包原理](https://angela52799.medium.com/%E7%84%A1%E6%B3%95%E7%90%86%E8%A7%A3%E7%9A%84-js-%E9%96%89%E5%8C%85%E5%8E%9F%E7%90%86-645fde1076fc)
