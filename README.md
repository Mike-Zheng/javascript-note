# JavaScript Note 麥可筆記

> 使用 AI 整理並重新理解 javascript 運作原理

[筆記之後改使用 isuue 來記錄](https://github.com/Mike-Zheng/javascript-note/issues)

## 前言

靠 js 吃飯多年，每次開發都會有發現不知或不足的地方，於是開一個筆記紀錄每次的資訊。

---

### Table of Contents

| No. | Content                                                                |
| --- | ---------------------------------------------------------------------- |
| 1   | [Execution Context 執行環境](#1-execution-context-執行環境)            |
| 2   | [Lexical Environment 詞法環境](#2-lexical-environment-詞法環境)        |
| 3   | [Hoisting 提升](#3-hoisting-提升)                                      |
| 4   | [Scope 作用域](#4-scope-作用域)                                        |
| 5   | [Global Scope 全域作用域](#5-global-scope-全域作用域)                  |
| 6   | [Function Scope 函數作用域](#6-function-scope-函數作用域)              |
| 7   | [Block Scope 塊作用域](#7-block-scope-塊作用域)                        |
| 8   | [Variable Declaration 變數宣告](#8-variable-declaration-變數宣告)      |
| 9   | [var, let, const 的區別](#9-var-let-const-的區別)                      |
| 10  | [Temporal Dead Zone 暫時性死區](#10-temporal-dead-zone-暫時性死區)     |
| 11  | [Closure 閉包](#11-closure-閉包)                                       |
| 12  | [Garbage Collection 垃圾回收機制](#12-garbage-collection-垃圾回收機制) |
| 13  | [Memory Management 記憶體管理](#13-memory-management-記憶體管理)       |
| 14  | [Call Stack 呼叫堆疊](#14-call-stack-呼叫堆疊)                         |
| 15  | [Event Loop 事件迴圈](#15-event-loop-事件迴圈)                         |
| 16  | [Callback Queue 回調隊列](#16-callback-queue-回調隊列)                 |
| 17  | [Microtask Queue 微任務隊列](#17-microtask-queue-微任務隊列)           |
| 18  | [Promises Promise 機制](#18-promises-promise-機制)                     |
| 19  | [Async/Await](#19-asyncawait)                                          |
| 20  | [JavaScript 引擎 如 V8 的架構](#20-javascript-引擎-如-v8-的架構)       |

---

### 1. Execution Context 執行環境

#### **什麼是 Execution Context？**

Execution Context（執行環境）是 JavaScript 中程式碼執行的最小單位。每當 JavaScript 引擎執行程式碼時，會創建對應的執行環境，用來管理變數、函數以及作用域。

**功能：**

1. **定義程式碼的執行範圍**：執行環境決定了哪些變數和函數在當前程式碼中是可訪問的。
2. **監控程式執行狀態**：執行環境包含執行程式碼時的相關資訊（例如變數值、函數狀態等）。

---

#### **執行環境的種類**

JavaScript 中的執行環境分為以下三種類型：

1. **全域執行環境 (Global Execution Context)**

   - 預設的執行環境，當 JavaScript 程式啟動時自動創建。
   - 全域變數和函數都屬於此環境。
   - 在瀏覽器中，`this` 指向 `window`；在 Node.js 中，`this` 指向 `global`。

   **範例：**

   ```javascript
   var x = 10;
   console.log(x); // 全域執行環境
   ```

2. **函數執行環境 (Function Execution Context)**

   - 每當一個函數被呼叫時，會創建一個新的函數執行環境。
   - 每個函數執行環境都有自己的作用域，內部的變數無法從外部直接訪問。

   **範例：**

   ```javascript
   function sum(a, b) {
     return a + b; // 函數執行環境
   }
   ```

3. **Eval 執行環境 (Eval Execution Context)**

   - 當執行 `eval()` 時，會創建一個特殊的執行環境。
   - 使用 `eval()` 創建的變數會屬於當前執行環境。

   **範例：**

   ```javascript
   eval("var a = 10; console.log(a);"); // eval 執行環境
   ```

---

#### **執行環境的結構**

執行環境由以下三個主要部分組成：

1. **變數環境 (Variable Environment)**

   - 儲存變數和函數的宣告。
   - 包含：
     - **變數宣告（Variable Declarations）**
     - **函數宣告（Function Declarations）**
     - **函數參數（Function Parameters）**

2. **詞法環境 (Lexical Environment)**

   - 描述當前程式碼的作用域鏈（Scope Chain），用於解析變數和函數。
   - 詞法環境包括：
     - **當前執行代碼塊的變數**
     - **父執行環境的引用（Outer Environment Reference）**

3. **`this` 綁定 (This Binding)**
   - `this` 的值取決於執行環境的上下文：
     - 在全域環境中，`this` 通常指向 `window`（瀏覽器）或 `global`（Node.js）。
     - 在函數執行環境中，`this` 的值根據函數的呼叫方式動態決定。

---

#### **執行環境的運作機制**

執行環境在 JavaScript 程式執行過程中會經歷以下階段：

1. **創建階段 (Creation Phase)**

   - 執行程式碼之前，JavaScript 引擎會創建執行環境。
   - 此階段會完成：
     - **建立詞法環境（Lexical Environment）**
     - **建立變數環境（Variable Environment）**
     - **綁定 `this` 值**

2. **執行階段 (Execution Phase)**
   - JavaScript 執行程式碼，並基於執行環境中的資訊處理變數和函數。

---

#### **執行環境與作用域鏈 (Scope Chain) 的關係**

執行環境中的 **詞法環境 (Lexical Environment)** 決定了作用域鏈。作用域鏈的主要功能是幫助 JavaScript 引擎解析變數。

- **作用域鏈的構成：**

  1. 當前執行環境的變數
  2. 父執行環境的變數
  3. 全域執行環境的變數

- **範例：**

  ```javascript
  function outer() {
    let a = 10;

    function inner() {
      console.log(a); // 從父作用域獲取 a
    }

    inner();
  }

  outer();
  ```

- 當 JavaScript 引擎查找變數時，會沿著作用域鏈進行查找，直到找到對應的變數或報錯為止。

---

#### **執行環境與 Hoisting (提升)**

**Hoisting（提升）** 是 JavaScript 的一個重要特性，指的是變數和函數的宣告會在執行程式碼之前被提升到執行環境的頂部。

- **變數提升：**

  ```javascript
  console.log(a); // undefined
  var a = 10;
  ```

  等價於：

  ```javascript
  var a;
  console.log(a); // undefined
  a = 10;
  ```

- **函數提升：**

  ```javascript
  greet(); // "Hello"
  function greet() {
    console.log("Hello");
  }
  ```

---

#### **Call Stack（呼叫堆疊）與執行環境**

JavaScript 使用 **Call Stack（呼叫堆疊）** 管理執行環境。每個執行環境會被推入堆疊（Push），當執行完成後會從堆疊中彈出（Pop）。

- **範例：**

  ```javascript
  function a() {
    b();
    console.log("In function a");
  }

  function b() {
    console.log("In function b");
  }

  a();
  ```

  **執行過程：**

  1. 全域執行環境被推入堆疊。
  2. 呼叫 `a()` 時，創建 `a` 的執行環境，並推入堆疊。
  3. 呼叫 `b()` 時，創建 `b` 的執行環境，並推入堆疊。
  4. 執行完成後，`b` 的執行環境從堆疊中彈出。
  5. 繼續執行 `a`，最後彈出 `a` 的執行環境。
  6. 最後全域執行環境退出堆疊。

  **堆疊狀態：**

  ```
  1. 全域執行環境
  2. a() 執行環境
  3. b() 執行環境
  ```

---

#### **小結**

1. **Execution Context（執行環境）** 是 JavaScript 執行程式碼的基礎單位。
2. 每個執行環境包含：
   - **變數環境（Variable Environment）**
   - **詞法環境（Lexical Environment）**
   - **`this` 綁定**
3. **全域執行環境** 只有一個，而 **函數執行環境** 可以有多個。
4. **作用域鏈（Scope Chain）** 和 **Hoisting（提升）** 是執行環境的重要概念。
5. JavaScript 使用 **Call Stack（呼叫堆疊）** 管理執行環境。

**[🔝 Back to Top](#table-of-contents)**

### 2. Lexical Environment 詞法環境

#### 什麼是 Lexical Environment？

Lexical Environment（詞法環境）是 JavaScript 中的一種結構，用來管理變數及函式的範圍與作用域。它是 JavaScript 執行程式碼時的核心概念之一，負責追蹤哪些變數、函式在當前的作用域中可以被存取。

#### 組成部分

Lexical Environment 包含兩個主要部分：

1. **Environment Record（環境記錄）**  
   儲存當前作用域中的所有變數和函式的實際記錄。
2. **Outer Lexical Environment Reference（外部詞法環境參考）**  
   指向外層的 Lexical Environment，形成一個作用域鏈（Scope Chain）。

#### 類型

Lexical Environment 可以分為以下幾種類型：

1. **Global Lexical Environment（全域詞法環境）**
   - 預設存在於程式的最外層，包含所有全域變數與全域函式。
   - 沒有外部詞法環境參考，因此其 `Outer Lexical Environment Reference` 為 `null`。
2. **Function Lexical Environment（函式詞法環境）**
   - 每個函式在執行時都會創建自己的詞法環境。
   - 包含函式內部定義的變數及參數。
3. **Block Lexical Environment（區塊詞法環境）**
   - ES6 引入的 `let` 和 `const` 創造了區塊作用域（Block Scope）。
   - 每個區塊（如 `if`、`for` 等）都會創建自己的詞法環境。

#### 運作機制

1. **變數與函式的宣告**  
   當 JavaScript 解譯器執行程式碼時，會根據程式碼結構建立對應的 Lexical Environment，並將變數、函式的宣告加入到 Environment Record 中。
2. **作用域鏈（Scope Chain）**  
   當執行程式碼時，JavaScript 會在當前的 Lexical Environment 中尋找變數或函式。如果找不到，會沿著 `Outer Lexical Environment Reference` 一層層向外尋找，直到找到為止。如果最外層的 Global Lexical Environment 也找不到，則會拋出錯誤。

#### 示例

```javascript
let a = 10; // 全域詞法環境

function foo() {
  let b = 20; // 函式詞法環境
  console.log(a); // 從外部詞法環境取得 'a'

  if (true) {
    let c = 30; // 區塊詞法環境
    console.log(b); // 從函式詞法環境取得 'b'
    console.log(c); // 從區塊詞法環境取得 'c'
  }
}

foo();
```

#### 運行過程

1. **全域層級**
   - 建立 Global Lexical Environment，將 `a` 宣告並儲存到 Environment Record。
2. **函式呼叫時**
   - 呼叫 `foo()` 時，建立 `foo` 的 Function Lexical Environment，將 `b` 宣告並儲存到 Environment Record。
   - `foo` 的外部詞法環境參考指向 Global Lexical Environment。
3. **區塊內部**
   - 進入 `if` 區塊時，建立 Block Lexical Environment，將 `c` 宣告並儲存到 Environment Record。
   - 該區塊的外部詞法環境參考指向 `foo` 的 Function Lexical Environment。

#### 注意事項

1. **變數提升（Hoisting）**
   - 變數宣告（`var`）會被提升到作用域的頂部，但值不會被初始化。
   - 使用 `let` 和 `const` 則不會被提升到環境記錄的頂部，會進入「暫時性死區」（Temporal Dead Zone, TDZ）。
2. **閉包（Closure）**
   - 閉包是一種函式，能夠「記住」外部詞法環境中的變數，即使該外部函式已經執行完畢。

#### 總結

- Lexical Environment 是 JavaScript 中的核心機制，用來處理變數與函式的作用域。
- 它包含 Environment Record 和 Outer Lexical Environment Reference，並透過作用域鏈來尋找變數。
- Lexical Environment 的理解對於掌握 JavaScript 的作用域、變數提升與閉包等概念至關重要。

**[🔝 Back to Top](#table-of-contents)**

### 3. Hoisting 提升

以下是關於 JavaScript 中 Hoisting（提升）概念的整理筆記，專有名詞附帶英文對應，並以繁體中文為主：

---

#### 什麼是 Hoisting（提升）？

Hoisting（提升）是 JavaScript 中的一種行為，指的是變數（Variables）、函式（Functions）或類別（Classes）的宣告（Declaration）會在編譯階段被移動到其作用域（Scope）的頂部，但初始化（Initialization）不會一起提升。

---

#### Hoisting 的範圍

1. **全域作用域（Global Scope）**
2. **函式作用域（Function Scope）**
3. **區塊作用域（Block Scope）** - ES6 之後新增，使用 `let` 和 `const` 宣告。

---

#### 變數（Variables）的提升

1. **使用 `var` 宣告**

   - 變數宣告會被提升，但初始化的部分不會。
   - 在提升後，變數的值為 `undefined`。

   **範例：**

   ```javascript
   console.log(a); // undefined
   var a = 10;
   ```

   **提升後的行為：**

   ```javascript
   var a;
   console.log(a); // undefined
   a = 10;
   ```

2. **使用 `let` 和 `const` 宣告**

   - `let` 和 `const` 的宣告也會被提升，但它們在「暫時性死區」（Temporal Dead Zone, TDZ）中，只有在程式碼執行到宣告的那一行時才可被存取。
   - 嘗試在宣告前存取 `let` 或 `const` 會丟出 `ReferenceError`。

   **範例：**

   ```javascript
   console.log(b); // ReferenceError: Cannot access 'b' before initialization
   let b = 20;

   console.log(c); // ReferenceError: Cannot access 'c' before initialization
   const c = 30;
   ```

---

#### 函式（Functions）的提升

1. **函式宣告（Function Declaration）**

   - 函式宣告會被完全提升，包括函式的名稱和其內部內容。

   **範例：**

   ```javascript
   console.log(add(2, 3)); // 5
   function add(x, y) {
     return x + y;
   }
   ```

   **提升後的行為：**

   ```javascript
   function add(x, y) {
     return x + y;
   }
   console.log(add(2, 3)); // 5
   ```

2. **函式表達式（Function Expression）**

   - 使用 `var` 的函式表達式會遵循變數提升的規則，僅提升變數宣告，但函式本身不會被提升。
   - 使用 `let` 或 `const` 宣告的函式表達式則會進入暫時性死區。

   **範例：**

   ```javascript
   console.log(add); // undefined
   var add = function (x, y) {
     return x + y;
   };
   ```

   **提升後的行為：**

   ```javascript
   var add;
   console.log(add); // undefined
   add = function (x, y) {
     return x + y;
   };
   ```

3. **箭頭函式（Arrow Function）**

   - 箭頭函式是函式表達式的一種，提升行為與 `let` 或 `const` 一致。

   **範例：**

   ```javascript
   console.log(add); // ReferenceError: Cannot access 'add' before initialization
   const add = (x, y) => x + y;
   ```

---

#### 類別（Classes）的提升

1. **類別宣告（Class Declaration）**

   - 類別宣告會被提升，但與 `let` 和 `const` 相同，會進入暫時性死區。
   - 嘗試在宣告前使用類別會丟出 `ReferenceError`。

   **範例：**

   ```javascript
   const obj = new MyClass(); // ReferenceError: Cannot access 'MyClass' before initialization
   class MyClass {
     constructor() {
       this.name = "Hoisting Example";
     }
   }
   ```

2. **類別表達式（Class Expression）**

   - 類似於函式表達式，僅變數宣告會被提升，但類別本身不會被提升。

   **範例：**

   ```javascript
   const obj = new MyClass(); // ReferenceError: Cannot access 'MyClass' before initialization
   const MyClass = class {
     constructor() {
       this.name = "Hoisting Example";
     }
   };
   ```

---

#### 暫時性死區（Temporal Dead Zone, TDZ）

暫時性死區是指從作用域開始到變數被宣告的這段期間，變數無法被存取。這是 `let`、`const` 和類別的特性，用來防止使用未初始化的變數。

**範例：**

```javascript
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 10;
```

---

#### 注意事項

1. **初始化與賦值的區別**
   - 提升只會針對「宣告」部分進行，初始化或賦值的部分不會被提升。
2. **避免使用 `var`**

   - `var` 的提升行為容易導致預期外的錯誤，建議使用 `let` 或 `const`。

3. **讀懂錯誤訊息**
   - 提升相關的錯誤通常是 `ReferenceError` 或 `undefined`，仔細檢查變數的宣告順序。

---

#### 總結

| 宣告方式   | 提升行為                                                            |
| ---------- | ------------------------------------------------------------------- |
| `var`      | 宣告提升，初始化不提升，未初始化時為 `undefined`。                  |
| `let`      | 宣告提升，但進入暫時性死區，未初始化前存取會丟出 `ReferenceError`。 |
| `const`    | 行為與 `let` 相同，但必須在宣告時初始化，否則會丟出 `SyntaxError`。 |
| 函式宣告   | 完全提升，包括函式名稱與內容。                                      |
| 函式表達式 | 僅變數宣告提升，函式本身不會被提升，遵循變數的提升行為。            |
| 箭頭函式   | 行為與函式表達式一致。                                              |
| 類別宣告   | 宣告提升，但進入暫時性死區，未初始化前存取會丟出 `ReferenceError`。 |
| 類別表達式 | 僅變數宣告提升，類別本身不會被提升，遵循變數的提升行為。            |

**[🔝 Back to Top](#table-of-contents)**

### 4. Scope 作用域

在 JavaScript 中，**作用域 (Scope)** 決定了程式碼中的變數、函式以及其他標識符的可訪問性及使用範圍。以下是完整的 JS 作用域筆記整理：

---

#### **作用域的分類**

1. **全域作用域 (Global Scope)**

   - 在程式的頂層宣告的變數屬於全域作用域。它們可以在程式的任何部分被存取。
   - 全域變數會被掛載到全域物件上（例如 `window`、`global` 等）。
   - 範例：

     ```javascript
     var globalVar = "I am global"; // 全域作用域
     console.log(globalVar); // 可在任何地方使用
     ```

2. **區域作用域 (Local Scope)**

   - 在函式內部或區塊內部宣告的變數屬於區域作用域，僅能在該範圍內存取。
   - 範例：

     ```javascript
     function testScope() {
       var localVar = "I am local"; // 區域作用域
       console.log(localVar); // 可在函式內存取
     }
     testScope();
     // console.log(localVar); // 錯誤：無法在函式外部存取
     ```

3. **區塊作用域 (Block Scope)**

   - 使用 `let` 或 `const` 宣告的變數具備區塊作用域，僅在該區塊 `{}` 中有效。
   - 範例：

     ```javascript
     {
       let blockScopedVar = "I am block-scoped";
       console.log(blockScopedVar); // 可在區塊內存取
     }
     // console.log(blockScopedVar); // 錯誤：無法在區塊外部存取
     ```

---

#### **作用域的特性**

1. **Lexical Scope (詞法作用域)**  
   JavaScript 使用詞法作用域，這意味著作用域是由程式碼的結構在撰寫時決定，而不是在程式執行時決定。

   - 範例：

     ```javascript
     function outerFunction() {
       var outerVar = "I am outer";
       function innerFunction() {
         console.log(outerVar); // 可存取外部作用域的變數
       }
       innerFunction();
     }
     outerFunction();
     ```

2. **作用域鏈 (Scope Chain)**

   - 當尋找變數時，JavaScript 會首先在當前作用域中查找，若找不到，則向外層作用域查找，直到全域作用域。
   - 範例：

     ```javascript
     var globalVar = "global";

     function outerFunction() {
       var outerVar = "outer";
       function innerFunction() {
         var innerVar = "inner";
         console.log(globalVar); // 從全域作用域取得
         console.log(outerVar); // 從外層作用域取得
         console.log(innerVar); // 從內層作用域取得
       }
       innerFunction();
     }
     outerFunction();
     ```

---

#### **變數提升 (Hoisting)**

- JavaScript 的 `var` 具有變數提升 (Hoisting) 特性，但提升的變數僅初始化為 `undefined`，而非其賦值內容。
- `let` 和 `const` 也會被提升，但它們會形成暫時性死區 (Temporal Dead Zone)，在宣告前無法存取。
- 範例：

  ```javascript
  console.log(hoistedVar); // undefined
  var hoistedVar = "I am hoisted";

  // console.log(notHoisted); // 錯誤：暫時性死區
  let notHoisted = "I am not hoisted";
  ```

---

#### **函式作用域 (Function Scope)**

- 使用 `var` 宣告的變數具有函式作用域，僅在函式內部有效。
- 範例：

  ```javascript
  function funcScopeExample() {
    var funcScopedVar = "I am function-scoped";
    console.log(funcScopedVar); // 在函式內存取
  }
  funcScopeExample();
  // console.log(funcScopedVar); // 錯誤：無法在函式外部存取
  ```

---

#### **區塊作用域與 `let`、`const`**

1. `let` 和 `const` 創造區塊作用域，並避免 `var` 的作用域污染問題。
2. `const` 用於宣告常數，需在宣告時初始化，且無法重新賦值。
3. 範例：

   ```javascript
   {
     let blockVar = "block scoped";
     const blockConst = "constant scoped";
     console.log(blockVar); // 可存取
     console.log(blockConst); // 可存取
   }
   // console.log(blockVar);  // 錯誤：區塊外部無法存取
   // console.log(blockConst); // 錯誤：區塊外部無法存取
   ```

---

#### **動態作用域與 `this`**

- **`this` 關鍵字**指向執行時的上下文，而非由詞法作用域決定。
- 範例：

  ```javascript
  function showThis() {
    console.log(this); // 根據執行方式決定 `this` 的指向
  }
  showThis(); // 在全域中執行，指向全域物件
  ```

---

#### **閉包 (Closure)**

- 閉包是指函式可以「記住」並存取其定義時所在的作用域，即使函式在其他地方執行。
- 範例：

  ```javascript
  function makeCounter() {
    let count = 0;
    return function () {
      count++;
      return count;
    };
  }
  const counter = makeCounter();
  console.log(counter()); // 1
  console.log(counter()); // 2
  ```

**[🔝 Back to Top](#table-of-contents)**

### 5. Global Scope 全域作用域

在 JavaScript 中，**全域作用域（Global Scope）** 是指在程式的最外層定義的變數或函式，這些變數或函式可以在程式的任何地方被存取。以下是完整的筆記整理：

---

#### 什麼是全域作用域？

- 全域作用域是指變數或函式在程式中最外層定義，這些全域的資源可以在任何地方被存取。
- 任何未在函式內或區塊內（例如 `if` 或 `for`）定義的變數或函式，默認屬於全域作用域。

---

#### 全域變數（Global Variables）

- **定義：** 全域作用域中的變數稱為全域變數。
- **特性：**
  - 可以在任何地方存取。
  - 容易造成命名衝突，因為所有程式碼都能存取或修改它。
- **例子：**

  ```javascript
  var globalVar = "我是全域變數"; // 定義全域變數

  function printGlobalVar() {
    console.log(globalVar); // 存取全域變數
  }

  printGlobalVar(); // 輸出：我是全域變數
  ```

---

#### 全域物件（Global Object）

- **定義：** 全域作用域中的所有全域變數和函式都成為全域物件的屬性。全域物件在瀏覽器中是 `window`，在 Node.js 中是 `global`。
- **常見全域物件：**
  - 在瀏覽器中：`window`。
  - 在 Node.js 中：`global`。
  - 在 ES2020 中：`globalThis`（統一提供跨平台的全域物件）。
- **例子：**

  ```javascript
  var myVar = "Hello World"; // myVar 成為 window 的屬性

  console.log(window.myVar); // 輸出：Hello World
  ```

---

#### 關鍵字 `var`、`let` 和 `const` 的影響

- 使用 `var` 定義的變數，會自動成為全域物件的屬性。
- 使用 `let` 和 `const` 定義的變數，雖然也是全域作用域的一部分，但不會成為全域物件的屬性。
- **例子：**

  ```javascript
  var varVariable = "我是 var 定義的全域變數";
  let letVariable = "我是 let 定義的全域變數";
  const constVariable = "我是 const 定義的全域變數";

  console.log(window.varVariable); // 輸出：我是 var 定義的全域變數
  console.log(window.letVariable); // 輸出：undefined
  console.log(window.constVariable); // 輸出：undefined
  ```

---

#### 全域污染（Global Pollution）

- **定義：** 當程式中有大量的全域變數時，容易出現命名衝突或非預期的行為，這種情況稱為全域污染。
- **避免方法：**
  - 使用區塊作用域（Block Scope）或模組化（Modules）。
  - 使用 `let` 和 `const` 避免不必要的全域變數。
  - 將程式碼封裝在自執行函式（IIFE, Immediately Invoked Function Expression）中。
- **例子：**

  ```javascript
  // 自執行函式避免全域污染
  (function () {
    var localVar = "我是封裝在函式內的變數";
    console.log(localVar); // 輸出：我是封裝在函式內的變數
  })();

  console.log(typeof localVar); // 輸出：undefined，localVar 不存在於全域作用域
  ```

---

#### 全域作用域的最佳實踐

1. **減少使用全域變數：** 將變數封裝在函式或模組內。
2. **使用模組化程式設計：** 透過 ES6 的 `import` 和 `export`，避免直接使用全域作用域。
3. **善用 `let` 和 `const`：** 避免使用 `var`，減少全域物件污染。
4. **命名空間（Namespace）：** 使用物件作為命名空間來封裝程式碼，避免命名衝突。

   ```javascript
   const MyApp = {
     config: {
       apiUrl: "https://api.example.com",
     },
     utils: {
       logMessage: function (message) {
         console.log(message);
       },
     },
   };

   MyApp.utils.logMessage("Hello Namespace!"); // 輸出：Hello Namespace!
   ```

**[🔝 Back to Top](#table-of-contents)**

### 6. Function Scope 函數作用域

在 JavaScript 中，作用域（Scope）指的是變數或函數的可訪問範圍。函數作用域（Function Scope）是 JavaScript 中的一種作用域規則，它規定了變數和函數僅在定義它們的函數內部是可見的。以下是有關 Function Scope 的詳細筆記：

---

#### 什麼是函數作用域？

- 函數作用域（Function Scope）是指**變數僅在其被定義的函數內部有效**，在函數外部無法訪問該變數。
- 使用 `var` 宣告的變數會遵循函數作用域。
- 函數作用域使得函數內部的變數和函數可以被隔離，避免與外部作用域發生衝突。

---

#### 函數作用域的特性

1. **變數的可訪問性**

   - 在函數內部宣告的變數只能在函數內部訪問，外部無法直接訪問。
   - 例如：

     ```javascript
     function myFunction() {
       var localVariable = "我是局部變數"; // localVariable 是局部變數
       console.log(localVariable); // 可在函數內部訪問
     }
     console.log(localVariable); // Error: localVariable is not defined
     ```

2. **作用域鏈（Scope Chain）**

   - 當函數內部需要訪問變數時，JavaScript 會沿著作用域鏈尋找變數。
   - 作用域鏈會從函數內部開始尋找，若找不到，則向外部作用域（例如全域作用域）尋找。

3. **變數遮蔽（Variable Shadowing）**

   - 如果函數內部定義了一個與外部作用域同名的變數，內部變數會遮蔽外部變數。
   - 例如：

     ```javascript
     var name = "外部作用域";

     function myFunction() {
       var name = "內部作用域"; // 內部變數遮蔽外部變數
       console.log(name); // 內部作用域
     }
     myFunction();
     console.log(name); // 外部作用域
     ```

4. **`var` 的函數作用域**

   - 使用 `var` 宣告的變數具有函數作用域。
   - 例如：

     ```javascript
     function testVar() {
       var x = 10;
       if (true) {
         var x = 20; // 重新賦值，作用域仍然是函數作用域
         console.log(x); // 20
       }
       console.log(x); // 20 (同一作用域內被重新賦值)
     }
     testVar();
     ```

---

#### 函數作用域 vs 區塊作用域（Block Scope）

- 使用 `let` 和 `const` 宣告的變數遵循區塊作用域（Block Scope），而 `var` 則遵循函數作用域。
- `let` 和 `const` 的區塊作用域使得它們的作用範圍僅限於 `{}` 大括號內，而 `var` 的作用範圍擴展到整個函數。
- 例如：

  ```javascript
  function testScope() {
    if (true) {
      var varVariable = "我是 var"; // 函數作用域
      let letVariable = "我是 let"; // 區塊作用域
      const constVariable = "我是 const"; // 區塊作用域
    }
    console.log(varVariable); // "我是 var"
    console.log(letVariable); // Error: letVariable is not defined
    console.log(constVariable); // Error: constVariable is not defined
  }
  testScope();
  ```

---

#### 函數作用域的好處

1. **避免命名衝突**
   - 函數作用域將變數封裝在函數內部，減少命名衝突的可能性。
2. **保持程式碼的模組化**
   - 將程式碼邏輯分隔在不同的函數內部，提升可讀性和維護性。
3. **提供封裝性（Encapsulation）**
   - 函數作用域允許特定的邏輯和數據僅在函數內部可見，增強安全性。

---

#### 注意事項

1. **`var` 的提升（Hoisting）**

   - 使用 `var` 宣告的變數會被提升到函數作用域的頂部，但不會初始化。
   - 例如：

     ```javascript
     function hoistingExample() {
       console.log(myVar); // undefined
       var myVar = "我是 var";
       console.log(myVar); // 我是 var
     }
     hoistingExample();
     ```

2. **全域作用域（Global Scope）**
   - 如果在函數外部使用 `var` 宣告變數，該變數屬於全域作用域，可能導致命名衝突。
   - 建議使用 `let` 或 `const` 宣告變數以避免污染全域作用域。

---

#### 總結

- 函數作用域（Function Scope）是 JavaScript 中的核心概念，使用 `var` 宣告變數會遵循函數作用域。
- 函數作用域提供封裝性與模組化，減少命名衝突。
- 雖然函數作用域有其特性，但在現代 JavaScript 中，建議使用 `let` 和 `const`，以避免 `var` 的提升和作用域混淆。

**[🔝 Back to Top](#table-of-contents)**

### 7. Block Scope 塊作用域

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 8. Variable Declaration 變數宣告

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 9. var, let, const 的區別

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 10. Temporal Dead Zone 暫時性死區

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 11. Closure 閉包

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 12. Garbage Collection 垃圾回收機制

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 13. Memory Management 記憶體管理

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 14. Call Stack 呼叫堆疊

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 15. Event Loop 事件迴圈

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 16. Callback Queue 回調隊列

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 17. Microtask Queue 微任務隊列

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 18. Promises Promise 機制

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 19. Async/Await

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**

### 20. JavaScript 引擎 如 V8 的架構

<!-- content -->

**[🔝 Back to Top](#table-of-contents)**
