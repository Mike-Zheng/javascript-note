# JavaScript Note

> 深度理解javascript運作原理



---

### Table of Contents

| No. | Questions |
|---- | ---------
|1  | [Execution Context 執行環境](#Execution-Context-執行環境) |
|2  | [Lexical Environment 詞法環境](#Lexical-Environment-詞法環境) |



------------------------------


1. ### Execution Context 執行環境
   > 關鍵字: Execution Context 執行環境 執行上下文

   **什麼是Execution Context**

      `Execution Context` 就是執行javascript code時候的環境，用於跟javascript code的運算時求值等，任何javascript code執行的時候都是處於一個 `Execution Context` 之中。

      `Execution Stack` 掌管了 `Execution Context` 創建後的執行順序。當javascript code值型會將創建後的`Execution Stack` 層層疊入，並順著作用域鏈 `scope chain` 訪問變量、如果內部有函數invoke就創建一個新的 `Execution Context` ，把原先的push下去並把控制權交出。

   ![execution_context.png](./images/execution_context.png)

   **Execution Stack**

      * `Execution Stack` 為 `Execution Context` 執行順序的stack，會將建立階段的 `Execution Context` 依順序push & pop ，其順序為FILO(First In, Last Out)。
      * 當引擎執行你的javascript code時，會先建立一個 `Gobal Execution Context` 並且把他push進 `Execution Stack`。
      * 而當執行到function時，會建立 `Functional Execution Context` ，如果有很多function，就會一層疊一層的方式push進去。
      * 每次從 `Execution Stack` 最上方 pop一個 `Execution Context` 並執行稱之為 `call stack` 。

      以下範例解釋 Execution Stack流程
      
      ```javascript
      let a = 'Hello World!';
      function first() {
        console.log('Inside first function');
        second();
        console.log('Again inside first function');
      }
      function second() {
        console.log('Inside second function');
      }
      first();
      console.log('Inside Global Execution Context');
      ```


      ![execution_context_stack.png](./images/execution_context_stack.png)
      > An Execution Context Stack for the above code.


      * 上面JavaScript code在瀏覽器中執行(invoke)時，JavaScript 引擎會先創建一個 `Gobal Execution Context` 並把它push進 `Execution Stack` 中。碰到 first() 執行時，引擎給這個函數創建一個新的 `Execution Context` ，然後把它push進 `Execution Stack` 的頂部。


      * 當 second() 在 first() 函數內部執行時，引擎會給 second 創建 `Execution Context` 並把它push進 `Execution Stack` 頂部，當 second 函數執行完畢，它的 `Execution Context` 就會從 `Execution Stack` 最上方pop，指針會指向它下面的 `Execution Context` ，也就是 first 函數的 `Execution Context`。


      * 當 first 函數執行完畢其 `Execution Context` 也會從最上方pop，指針就指向了 `Gobal Execution Context` 。當所有的代碼執行完畢，引擎會把 `Gobal Execution Context` 也從 `Execution Stack` 中移出。


   **Execution Context的種類**
      
      Execution Context一共有三種:
      1. Gobal Execution Context:
          * 預設或是基本的 `Execution Context`。
          * 在一個程序(program)中只會有一個 `Gobal Execution Context`。
          * 建立 
            * `global object` (在瀏覽器是 `window` )。
            * `this`，並把 `this` 指向 `global object` 。
          
      2. Functional Execution Context:
          * 執行function的時候，會創立一個新的 `Functional Execution Context` ，每一個function執行都會有自己的 `Execution Context` 。
          * 相同的function code在不同的執行階段會建立各自的 `Execution Context` 。
      3. Eval:
          * 在 eval 函數中執行的javascript code也會有自己的 `Execution Context` ，但由於 eval 因為安全因素已經不常使用。

      
   **Execution Context 階段**

      `Execution Context`有兩個階段:
      1. 建立階段 `The Creation Phase`
      2. 執行階段 `The Execution Phase`

      * 建立階段 `The Creation Phase`

        `Execution Context`的創建階段，發生在function invoke時且在執行函數內的code之前，在創建階段js 引擎會做如下操作：

        * 創建 詞法環境( `Lexical Environment` ) 與 變量環境( `Variable Environment` )並  `Hoisting`
           * `Hoisting` : 在建立階段預先將變數分配記憶體空間並預設賦值為 undefined 
        * 建立`this`，建立全域物件 `global object`
          * binding `this` ，而 `global object` 只有在 ` Gobal Execution Context` 的時候才會建立。
        * 建立 `scope chain` 與外部環境 `Outer Environment`
          * 對於 `Gobal Execution Context` 而言，其 `Outer Environment` 為null，對於 `Functional Execution Context` 而言，如果 function b 包在 function a 裡面，那 function b 的外部環境就是 function a
       

      * 執行階段 `The Execution Phase`

        在此階段invoke JavaScript 並pop出 `Execution Stack`。


   **reference**
     * [JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
     * [Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)

     **[Back to Top](#table-of-contents)**



2. ### Lexical Environment 詞法環境

   > 關鍵字: Lexical Environment 詞法環境

   **Lexical Environment**  詞法環境:
      * `Lexical Environment` : 在code中真實存在的位置以及周圍的內容。 在JavaScript中它的位置及順序。
      
      * function內的 `{...}` 即為一個 `scope`，`Lexical Environment`會依照其內外環境創造內容，並存入`Execution Context`之中。

        ``` javascript
        function helloWorld(){
          let text = 'hello world!'
        }
        ```
      * 每當function被呼叫前的創立階段時，都會產生一組新的語彙環境 (`Lexical Environment`)，因此**function的作用域與其相關環境變數在函數創立的時候就已經決定**。
      * 當 `Execution Stack` 執行到此function的 `Execution Context`時，也就是invoke 此function時，會依照**已建立**的`Lexical Environment` 環境執行其內容。


   **Variable Environment** 變量環境:
      * 在 `ES6` 前，宣告變數都是通過var關鍵詞宣告的，在 `ES6` 中則提倡使用 `let` 和 `const` 來聲明變量，為了兼容var的寫法，於是使用 `Variable Environment` 變量環境來存儲var聲明的變量。
      * 為特化的`Lexical Environment`，其內只存變數`var`。

   **Lexical Environment** 的組成
      * `Environment Record` 環境記錄器
        `Environment Record`為存放 變數 與 function宣告 的地方
        `Environment Record` 分為兩種
          * `Declarative environment record` 聲明式環境記錄器存儲變量、函數和參數。
          * `Object environment record` 對象環境記錄器用來定義出現在全局上下文中的變量和函數的關係。

        **note**
        * 在全局環境中，環境記錄器是對象環境記錄器。
        * 在函數環境中，環境記錄器是聲明式環境記錄器。
        * 注意 — 對於函數環境，聲明式環境記錄器還包含了一個傳遞給函數的 arguments 對象（此對象存儲索引和參數的映射）和傳遞給函數的參數的 length。 

   **lexical environment looks like this in pseudocode**

      ``` javascript
      GlobalExectionContext = {
        LexicalEnvironment: {
          EnvironmentRecord: {
            Type: "Object",
            // Identifier bindings go here
          }
          outer: <null>,
          this: <global object>
        }
      }
      ```
      
      ``` javascript
      FunctionExectionContext = {
        LexicalEnvironment: {
          EnvironmentRecord: {
            Type: "Declarative",
            // Identifier bindings go here
          }
          outer: <Global or outer function environment reference>,
          this: <depends on how function is called>
        }
      }
      ```

   * reference
     * [JavaScript: Understanding the Weird Parts](https://www.udemy.com/course/understand-javascript/)
     * [Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)

      **[Back to Top](#table-of-contents)**


