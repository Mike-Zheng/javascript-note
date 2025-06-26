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
| 9   | [Temporal Dead Zone 暫時性死區](#9-temporal-dead-zone-暫時性死區)      |
| 10  | [Closure 閉包](#10-closure-閉包)                                       |
| 11  | [Garbage Collection 垃圾回收機制](#11-garbage-collection-垃圾回收機制) |
| 12  | [Memory Management 記憶體管理](#12-memory-management-記憶體管理)       |
| 13  | [Call Stack 呼叫堆疊](#13-call-stack-呼叫堆疊)                         |
| 14  | [Event Loop 事件迴圈](#14-event-loop-事件迴圈)                         |
| 15  | [Callback Queue 回調隊列](#15-callback-queue-回調隊列)                 |
| 16  | [Microtask Queue 微任務隊列](#16-microtask-queue-微任務隊列)           |
| 17  | [Promises Promise 機制](#17-promises-promise-機制)                     |
| 18  | [Async/Await](#18-asyncawait)                                          |
| 19  | [JavaScript 引擎 如 V8 的架構](#19-javascript-引擎-如-v8-的架構)       |

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

在 JavaScript 中，塊作用域（Block Scope）是由一對大括號 `{}` 定義的範圍，最常見於控制流程語句如 `if`、`for`、`while`，以及在 ES6 中的 `let` 和 `const` 關鍵字。塊作用域的出現使得變量的可見範圍更加清晰，並避免了全域變量污染問題。

#### 7.1 變量宣告

- **let**

  - 使用 `let` 宣告的變量具有塊作用域。
  - 範例：

    ```javascript
    if (true) {
      let x = 10;
      console.log(x); // 10
    }
    console.log(x); // ReferenceError: x is not defined
    ```

- **const**

  - `const` 宣告的變量同樣具有塊作用域，並且必須被初始化，值不可重新指派。
  - 範例：

    ```javascript
    if (true) {
      const y = 20;
      console.log(y); // 20
    }
    console.log(y); // ReferenceError: y is not defined
    ```

#### 7.2 暫時性死區 (Temporal Dead Zone)

- 在塊作用域內，從代碼塊開始到變量用 `let` 或 `const` 宣告之前的區域稱為暫時性死區。
- 該區域內引用這些變量會導致 ReferenceError。
- 範例：
  ```javascript
  {
    console.log(z); // ReferenceError: Cannot access 'z' before initialization
    let z = 30;
  }
  ```

#### 7.3 比較 var、let 與 const

- **var**

  - `var` 宣告的變量是函數作用域（Function Scope）或全域作用域（Global Scope），不支援塊作用域。
  - 範例：

    ```javascript
    if (true) {
      var a = 5;
    }
    console.log(a); // 5
    ```

- **let 與 const**
  - 這兩者支援塊作用域，能夠避免變量提升（Hoisting）帶來的問題。
  - 它們提升了代碼可讀性和維護性。

#### 7.4 使用建議

- 優先使用 `let` 或 `const` 來宣告變量，尤其是在需要塊作用域的情況下。
- 使用 `const` 宣告不會被重新賦值的變量，以增加代碼的穩定性。
- 僅在需要重新賦值的情況下使用 `let`。

#### 7.5 常見問題

- **作用域污染**
  - 使用 `var` 可能導致作用域污染，應儘量避免。
- **意外重複宣告**
  - 使用 `let` 和 `const` 可以防止同一作用域內重複宣告變量。

**[🔝 Back to Top](#table-of-contents)**

### 8. Variable Declaration 變數宣告

在 JavaScript 中，變數宣告 (Variable Declaration) 是建立和儲存資料的基本方式。以下是 JavaScript 中的三種主要變數宣告方式：`var`、`let` 和 `const`，以及它們的特性與使用範例。

---

#### 8.1 `var` 宣告

`var` 是 JavaScript 中最早出現的變數宣告方式，但因為作用域 (Scope) 的行為較不直覺，現代開發中較少使用。

- **特性**：

  1. **函式作用域** (Function Scope)：`var` 宣告的變數只能在函式內有效。
  2. **允許重複宣告**：同一作用域內可以多次使用 `var` 宣告相同名稱的變數。
  3. **提升 (Hoisting)**：`var` 宣告的變數會在作用域的最上方被初始化為 `undefined`。

- **範例**：

  ```javascript
  console.log(a); // undefined (變數提升)
  var a = 10;
  console.log(a); // 10

  if (true) {
    var b = 20; // `b` 在整個函式作用域內有效
  }
  console.log(b); // 20
  ```

---

#### 8.2 `let` 宣告

`let` 是 ES6 (ECMAScript 2015) 中引入的變數宣告方式，解決了 `var` 的一些問題。

- **特性**：

  1. **區塊作用域** (Block Scope)：`let` 宣告的變數僅在 `{}` 區塊內有效。
  2. **不允許重複宣告**：同一作用域內不能使用相同名稱的變數重複宣告。
  3. **提升但不可使用**：`let` 變數會提升，但在初始化前無法使用 (稱為暫時性死區，Temporal Dead Zone, TDZ)。

- **範例**：

  ```javascript
  if (true) {
    let x = 10;
    console.log(x); // 10
  }
  // console.log(x); // ReferenceError: x is not defined

  let y = 5;
  // let y = 10; // SyntaxError: Identifier 'y' has already been declared
  ```

---

#### 8.3 `const` 宣告

`const` 是 ES6 中引入的，用於宣告常數 (Constant)。一旦宣告後，變數的值不可改變。

- **特性**：

  1. **區塊作用域** (Block Scope)：與 `let` 相同，僅在 `{}` 區塊內有效。
  2. **必須立即初始化**：宣告時必須指定初始值。
  3. **不可重新賦值**：值一旦設定後，不能再更改。
  4. **對於物件和陣列 (Object and Array)**：`const` 宣告的物件或陣列本身不可重新指派 (reassignment)，但其內容可以修改。

- **範例**：

  ```javascript
  const z = 50;
  // z = 60; // TypeError: Assignment to constant variable.

  const arr = [1, 2, 3];
  arr.push(4); // 可以修改陣列內容
  console.log(arr); // [1, 2, 3, 4]

  const obj = { name: "John" };
  obj.age = 30; // 可以修改物件屬性
  console.log(obj); // { name: "John", age: 30 }
  ```

---

#### 8.4 三種宣告方式比較

| 特性                              | `var`                       | `let`                    | `const`                  |
| --------------------------------- | --------------------------- | ------------------------ | ------------------------ |
| **作用域**                        | 函式作用域 (Function Scope) | 區塊作用域 (Block Scope) | 區塊作用域 (Block Scope) |
| **提升 (Hoisting)**               | 是 (初始化為 `undefined`)   | 是 (但不可使用，TDZ)     | 是 (但不可使用，TDZ)     |
| **允許重複宣告**                  | 是                          | 否                       | 否                       |
| **是否必須初始化**                | 否                          | 否                       | 是                       |
| **是否可重新賦值 (Reassignment)** | 是                          | 是                       | 否                       |

---

#### 8.5 使用建議

1. **優先使用 `const`**：能使用常數的情況下，儘量使用 `const`，以避免意外改動變數值。
2. **需要改變值時使用 `let`**：如果變數的值需要更新，則使用 `let`。
3. **避免使用 `var`**：`var` 的作用域行為易導致錯誤，除非需支援舊版瀏覽器，否則不建議使用。

---

**[🔝 Back to Top](#table-of-contents)**

### 9. Temporal Dead Zone 暫時性死區

在 JavaScript 中，`let` 和 `const` 宣告的變數有一個特殊的行為，稱為 **Temporal Dead Zone (TDZ, 暫時性死區)**。這是一個變數在宣告之前無法被存取的範圍。

---

#### Temporal Dead Zone (TDZ) 是什麼？

當變數使用 `let` 或 `const` 宣告時，該變數在其宣告語句之前的程式碼區間內處於 **暫時性死區 (Temporal Dead Zone)**。在這段區域內，變數被視為已經存在於作用域中，但尚未完成初始化，因此任何對該變數的存取都會拋出錯誤。

---

#### 特性與行為

1. **變數的生命周期**：

   - 在執行 `let` 或 `const` 宣告之前，變數已進入作用域 (Scope)。
   - 在宣告之前，變數處於暫時性死區 (TDZ)，無法被存取。
   - 在宣告之後，變數才可被正常使用。

2. **錯誤的類型**：

   - 如果在 TDZ 中存取變數，會拋出 `ReferenceError`。

3. **作用於 `let` 和 `const`**：

   - 只有 `let` 和 `const` 宣告會有 TDZ 的行為。
   - `var` 宣告則沒有 TDZ，因為它會被「提升 (Hoisting)」，並初始化為 `undefined`。

4. **函式與參數影響**：
   - 如果函式的參數使用了 `let` 或 `const`，參數變數也可能會受到 TDZ 的影響。

---

#### 範例解析

1. **基本範例**

   ```javascript
   console.log(a); // ReferenceError: Cannot access 'a' before initialization
   let a = 10;
   ```

   - 在 `let a = 10;` 宣告之前，變數 `a` 處於 TDZ 中。
   - 因此，在宣告之前存取 `a`，會拋出 `ReferenceError`。

2. **TDZ 的範圍**

   ```javascript
   {
     console.log(b); // ReferenceError
     let b = 20;
   }
   ```

   - 變數 `b` 的 TDZ 從作用域區塊 `{` 開始，到 `let b = 20;` 宣告為止。

3. **不受 TDZ 限制的 `var`**

   ```javascript
   console.log(c); // undefined
   var c = 30;
   ```

   - 使用 `var` 宣告的變數會被提升 (Hoisted) 並初始化為 `undefined`，因此不會拋出錯誤。

4. **函式參數的 TDZ**

   ```javascript
   function example(d = e) {
     let e = 50;
     return d;
   }
   console.log(example()); // ReferenceError: Cannot access 'e' before initialization
   ```

   - 在函式中，`let e = 50;` 的 TDZ 影響函式參數 `d = e` 的初始化，因此會拋出 `ReferenceError`。

5. **`const` 的 TDZ**

   ```javascript
   console.log(f); // ReferenceError
   const f = 40;
   ```

   - `const` 宣告的變數同樣有 TDZ 行為，且在宣告後必須立即賦值。

---

#### 為什麼會有 TDZ？

TDZ 的設計是為了讓程式碼更安全，避免開發者在變數尚未初始化前就進行存取。這與 `var` 的「提升」行為相比，能有效減少因使用未初始化變數而導致的錯誤。

---

#### TDZ 的應用與注意事項

1. **良好變數管理**：

   - 確保變數在使用之前先宣告並初始化。

2. **避免混淆的變數提升**：

   - 儘量使用 `let` 和 `const`，避免使用 `var`，以降低不必要的行為混亂。

3. **函式中避免 TDZ 問題**：

   - 小心函式參數的初始化順序，避免 TDZ 的錯誤。

4. **遵循區塊作用域 (Block Scope)**：
   - 熟悉 `let` 和 `const` 的區塊作用域範圍，避免在作用域外存取變數。

---

#### 總結

- **Temporal Dead Zone (TDZ, 暫時性死區)** 是 `let` 和 `const` 的特性。
- 在變數宣告之前，變數處於 TDZ 中，任何存取都會拋出 `ReferenceError`。
- TDZ 的目的是提升程式碼的安全性，鼓勵更嚴謹的變數使用方式。

**[🔝 Back to Top](#table-of-contents)**

### 10. Closure 閉包

#### 什麼是閉包？

閉包（Closure）是 JavaScript 中的一個重要概念。閉包指的是**當一個函式能夠記住並存取它在定義時的作用域（Scope）時，即使這個函式在其定義作用域之外被執行**，它仍然能夠存取該作用域內的變數。

---

#### 閉包的特性

1. **函式內部的函式**  
   閉包通常是在函式內部定義另一個函式，內層函式可以存取外層函式的變數，即使外層函式已經執行完畢。

2. **記住作用域**  
   閉包會記住函式在定義時的作用域，而不是函式在執行時的作用域。

3. **私有變數**  
   閉包可以用來模擬「私有變數」，因為閉包內的變數無法從外部直接存取。

---

#### 使用閉包的範例

以下是一些常見的閉包使用場景：

##### 範例 1: 基本閉包

```javascript
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log(`Outer Variable: ${outerVariable}`);
    console.log(`Inner Variable: ${innerVariable}`);
  };
}

const newFunction = outerFunction("外層變數");
newFunction("內層變數");
// Output:
// Outer Variable: 外層變數
// Inner Variable: 內層變數
```

---

##### 範例 2: 模擬私有變數

閉包可以用來模擬私有變數，讓外部無法直接存取內部的數據。

```javascript
function createCounter() {
  let count = 0; // 私有變數
  return {
    increment: function () {
      count++;
      console.log(count);
    },
    decrement: function () {
      count--;
      console.log(count);
    },
    getCount: function () {
      return count;
    },
  };
}

const counter = createCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.decrement(); // 1
console.log(counter.getCount()); // 1
```

---

##### 範例 3: 每個閉包擁有自己的作用域

每次執行外層函式時，閉包都會擁有一個新的作用域。

```javascript
function createAdder(x) {
  return function (y) {
    return x + y;
  };
}

const add5 = createAdder(5);
const add10 = createAdder(10);

console.log(add5(2)); // 7
console.log(add10(2)); // 12
```

---

##### 範例 4: 閉包中的迴圈問題

在使用 `var` 宣告變數時，可能會遇到閉包與迴圈的作用域問題。

```javascript
// 錯誤範例
function badExample() {
  var functions = [];
  for (var i = 0; i < 3; i++) {
    functions.push(function () {
      console.log(i);
    });
  }
  return functions;
}

const funcs = badExample();
funcs[0](); // 3
funcs[1](); // 3
funcs[2](); // 3
```

##### 解法：使用 `let` 或立即執行函式表達式（IIFE）

```javascript
// 解法 1: 使用 let
function fixedExample() {
  var functions = [];
  for (let i = 0; i < 3; i++) {
    functions.push(function () {
      console.log(i);
    });
  }
  return functions;
}

const fixedFuncs = fixedExample();
fixedFuncs[0](); // 0
fixedFuncs[1](); // 1
fixedFuncs[2](); // 2

// 解法 2: 使用 IIFE
function fixedExampleWithIIFE() {
  var functions = [];
  for (var i = 0; i < 3; i++) {
    (function (j) {
      functions.push(function () {
        console.log(j);
      });
    })(i);
  }
  return functions;
}

const iifeFuncs = fixedExampleWithIIFE();
iifeFuncs[0](); // 0
iifeFuncs[1](); // 1
iifeFuncs[2](); // 2
```

---

#### 閉包的應用場景

1. **資料封裝與模擬私有變數**  
   閉包可以隱藏內部的變數，讓外部無法直接修改，類似於物件導向程式設計中的「封裝」。

2. **事件處理與回呼函式**  
   在事件監聽或回呼函式中，閉包可以幫助記住某些特定的上下文資訊。

3. **函式工廠（Function Factory）**  
   使用閉包可以動態生成具有不同行為的函式，例如範例 3 的 `createAdder`。

4. **柯里化（Currying）**  
   柯里化是一種將多參數函式轉換為一系列單參數函式的技術，閉包在其中扮演關鍵角色。

---

#### 注意事項與最佳實踐

1. **記憶體洩漏（Memory Leak）**  
   閉包可能會導致記憶體洩漏，因為閉包會持續引用外層函式的作用域，即使外層函式已經執行完畢。避免不必要的閉包，並適時釋放不再需要的引用。

2. **過度使用閉包**  
   雖然閉包功能強大，但過度使用可能會降低程式碼的可讀性與維護性。要根據實際需求決定是否使用閉包。

---

#### 小結

- 閉包是 JavaScript 中強大的工具，能讓函式記住其定義時的作用域。
- 閉包常用於實現私有變數、事件處理以及函式工廠等場景。
- 在使用閉包時，需注意記憶體管理與可讀性問題。

**[🔝 Back to Top](#table-of-contents)**

### 11. Garbage Collection 垃圾回收機制

### 11. Garbage Collection 垃圾回收機制

JavaScript 的垃圾回收機制 (Garbage Collection, GC) 是一個自動化的記憶體管理過程，負責回收不再使用的記憶體資源，避免記憶體洩漏 (Memory Leak)。以下是 Garbage Collection 的運作方式與相關概念整理：

---

#### 1. 垃圾回收的基本原理

JavaScript 使用「可達性」(Reachability) 作為判斷物件是否應被回收的依據：

- **可達性 (Reachability)**：指從根 (Root) 出發能夠訪問到的值。
  - 根 (Root)：在 JavaScript 中，根通常指的是全域物件 (Global Object，例如 `window`)、當前執行的執行緒 (Execution Context) 的變數，以及函式的執行堆疊 (Call Stack)。
  - 若某物件無法從根訪問，則被認為是不可達的，會被視為垃圾。

---

#### 2. 常見的垃圾回收演算法

JavaScript 的垃圾回收機制是由 JavaScript 引擎 (例如 V8) 實現的。以下是兩種常見的垃圾回收演算法：

1. **標記-清除 (Mark-and-Sweep)**：

   - 這是最常用的垃圾回收演算法。
   - 運作流程：
     1. 垃圾回收器從根開始，標記所有可達的物件。
     2. 清除那些未被標記的物件，釋放其佔用的記憶體。
   - 特點：
     - 簡單且高效。
     - 不會整理記憶體碎片 (Memory Fragmentation)。

2. **分代回收 (Generational Garbage Collection)**：
   - 將記憶體分為「新生代」(Young Generation) 和「老生代」(Old Generation)。
     - **新生代 (Young Generation)**：存放短時間內使用的物件，通常是剛創建的物件。
     - **老生代 (Old Generation)**：存放存在時間較長的物件。
   - 運作流程：
     1. 垃圾回收器會頻繁地掃描新生代，快速回收短命的物件。
     2. 如果物件在新生代中存活足夠久，會被移動到老生代。
   - 特點：
     - 提高垃圾回收效率，因為大多數物件是短命的。

---

#### 3. 記憶體管理的注意事項

1. **避免記憶體洩漏 (Memory Leak)**：

   - 記憶體洩漏是指不再需要的記憶體沒有被釋放，導致記憶體使用量持續增加。
   - 常見的記憶體洩漏原因：
     1. **未清理的全域變數**：例如，將變數賦值給 `window`。
     2. **閉包 (Closure)**：閉包可能意外地保留不必要的變數。
     3. **未清理的事件監聽器 (Event Listeners)**：未移除的事件監聽器會導致對 DOM 元素的引用保留。
     4. **被遺忘的計時器或回呼 (setInterval, setTimeout)**：未清理的計時器會持續佔用記憶體。

2. **合理使用弱引用 (Weak References)**：

   - JavaScript 提供了 `WeakMap` 和 `WeakSet`，它們的鍵或值是弱引用 (Weak Reference)，不會阻止垃圾回收。
   - 適合用於儲存臨時資料或可被釋放的物件。

3. **手動釋放資源**：
   - 雖然 JavaScript 是自動記憶體管理，但仍需注意手動釋放資源，例如：
     - 移除不再需要的事件監聽器。
     - 清理計時器或回呼。

---

#### 4. 記憶體分析工具

JavaScript 提供一些工具幫助開發者分析記憶體使用情況，檢測記憶體洩漏問題：

1. **瀏覽器開發者工具 (DevTools)**：
   - 如 Chrome DevTools 提供的 `Memory` 面板，可以用來分析記憶體堆快照 (Heap Snapshot)、追蹤記憶體分配。
2. **Performance Profiling**：
   - 使用性能分析工具檢測記憶體使用峰值，找出不必要的佔用資源。

---

#### 5. 實踐建議

1. **避免不必要的全域變數**：

   - 使用 `let` 或 `const` 宣告區域變數，盡量避免使用全域變數。

2. **適時清理事件監聽器**：

   - 使用 `removeEventListener` 清理不再需要的事件監聽器。

3. **小心閉包的使用**：

   - 在使用閉包時，確保不會意外捕獲不必要的變數，避免無意的記憶體佔用。

4. **使用工具定期檢測**：
   - 定期使用開發工具檢查記憶體使用情況，特別是在大型應用中。

---

#### 總結

JavaScript 的垃圾回收機制是自動化的，但開發者仍需注意合理管理記憶體，避免記憶體洩漏問題。了解垃圾回收的工作原理和常見問題，能有效提高應用效能和穩定性。

**[🔝 Back to Top](#table-of-contents)**

### 12. Memory Management 記憶體管理

在 JavaScript 中，記憶體管理主要是指如何分配記憶體以及如何釋放不再使用的記憶體。以下是相關的重點整理：

---

#### **記憶體管理的基本概念**

1. **記憶體分配 (Memory Allocation)**

   - 當我們宣告變數、建立物件或函式時，JavaScript 會自動分配記憶體。
   - 範例：
     ```javascript
     let num = 42; // 為數字分配記憶體
     let obj = { name: "Alice" }; // 為物件分配記憶體
     ```

2. **記憶體使用 (Memory Usage)**

   - JavaScript 的執行環境（例如瀏覽器中的 V8 引擎）會管理記憶體，包括分配及追蹤使用狀態。

3. **記憶體釋放 (Memory Release)**
   - 當某些記憶體不再被使用時，JavaScript 會嘗試釋放這些記憶體。這通常由垃圾回收機制（Garbage Collection）完成。

---

#### **垃圾回收 (Garbage Collection)**

1. **什麼是垃圾回收 (Garbage Collection)**

   - JavaScript 是一種高階語言，具有自動垃圾回收機制，無需手動管理記憶體。
   - 垃圾回收的主要目的是釋放不再使用的記憶體以避免記憶體洩漏（Memory Leak）。

2. **標記-清除演算法 (Mark-and-Sweep Algorithm)**

   - 最常見的垃圾回收方式。
   - 工作流程：
     1. **標記 (Mark)**：垃圾回收器會標記所有存活（reachable）的物件。
     2. **清除 (Sweep)**：未被標記的物件會被視為不再使用，記憶體將被釋放。

3. **可達性 (Reachability)**
   - 一個物件是否存活的判斷標準是「可達性」。
   - 可達的物件包括：
     - 全域物件（Global Object），例如 `window`。
     - 函式的局部變數和參數。
     - 被其他存活物件引用的物件。

---

#### **常見的記憶體問題**

1. **記憶體洩漏 (Memory Leak)**

   - 當物件不再需要使用，但未被垃圾回收器釋放，導致記憶體不斷累積。
   - 常見原因：
     - **全域變數 (Global Variables)**：未正確清理的全域變數可能會永久存在。
     - **閉包 (Closure)**：閉包可能導致某些變數無法被垃圾回收。
     - **未清理的 DOM 引用**：移除 DOM 元素時未清理相關事件監聽器或引用。

2. **如何避免記憶體洩漏**：
   - 使用 `let` 或 `const` 宣告區域變數，避免使用 `var`。
   - 在不需要使用的時候，主動解除對物件的引用。
   - 移除不再需要的事件監聽器。
   - 避免不必要的全域變數。

---

#### **記憶體最佳化 (Memory Optimization)**

1. **減少記憶體使用**

   - 避免建立過大的資料結構。
   - 使用更高效的資料型別，例如 `Map` 或 `Set`。

2. **清理不必要的物件引用**

   - 範例：在函式結束時，讓不必要的物件被垃圾回收。
     ```javascript
     function processData() {
       let largeData = new Array(1000000).fill("data");
       // 處理資料
       largeData = null; // 主動清理引用
     }
     ```

3. **使用工具監控記憶體**
   - 使用瀏覽器開發工具（例如 Chrome DevTools）中的「記憶體分析」功能來檢查記憶體使用情況。

---

#### **示例：追蹤記憶體洩漏**

以下是一個可能導致記憶體洩漏的範例，以及如何修復：

```javascript
// 記憶體洩漏範例
let element = document.getElementById("button");
element.addEventListener("click", () => {
  console.log("Button clicked!");
});
// 如果 element 被移除，但事件監聽器未清理，會導致洩漏

// 修復方式
element.removeEventListener("click", handler);
element = null; // 移除引用，允許垃圾回收
```

---

#### **結論**

記憶體管理在 JavaScript 中雖然由執行環境自動處理，但了解其工作原理以及潛在問題，可以幫助我們撰寫更高效、穩定的程式。避免記憶體洩漏並進行適當的最佳化，能提升應用程式的效能並減少不必要的資源消耗。

---

**[🔝 Back to Top](#table-of-contents)**

### 13. Call Stack 呼叫堆疊

#### 什麼是 Call Stack？

在 JavaScript 中，**Call Stack（呼叫堆疊）** 是一種資料結構，用來記錄程式執行時函式呼叫的順序。它是一個 **LIFO（Last In, First Out，後進先出）** 的結構，最新的函式呼叫會被放在堆疊的頂端，執行完成後再移除。

#### Call Stack 的工作流程

1. 當一個函式被呼叫時，會被加入到 **Call Stack** 的頂端。
2. 函式執行完成後會從 **Call Stack** 中移除。
3. JavaScript 以同步方式（synchronous execution）逐一執行 **Call Stack** 中的函式。

---

#### 例子：Call Stack 的基本運作

以下是一個簡單範例，展示 **Call Stack** 的工作流程：

```javascript
function foo() {
  console.log("執行 foo");
  bar(); // 呼叫 bar 函式
}

function bar() {
  console.log("執行 bar");
}

foo(); // 呼叫 foo 函式
```

##### Call Stack 操作步驟：

1. `foo()` 被呼叫，加入 Call Stack。
2. `foo` 開始執行，並在內部呼叫了 `bar()`，將 `bar()` 加入 Call Stack。
3. `bar()` 執行完成後，從 Call Stack 移除。
4. `foo()` 執行完成後，從 Call Stack 移除。

執行順序如下：

```
Call Stack:
1. foo()
2. bar() (加入)
3. bar() (移除)
4. foo() (移除)
```

---

#### JavaScript 為何使用 Call Stack？

JavaScript 是單執行緒（single-threaded）的程式語言，不能同時執行多個程式碼片段。因此，**Call Stack** 用來管理程式碼的執行順序，確保每一個函式依序執行。這是 JavaScript 非同步行為（如事件監聽和 Promise）背後的基礎。

---

#### 堆疊溢出（Stack Overflow）

如果函式呼叫過多且未正確完成，**Call Stack** 可能會溢出（Stack Overflow），導致程式崩潰。

##### 範例：無窮遞迴（Infinite Recursion）

```javascript
function recursive() {
  recursive(); // 無窮遞迴，永遠不會停止
}

recursive(); // 呼叫 recursive 函式
```

執行上述程式碼時，`recursive()` 不斷呼叫自己，造成 **Call Stack** 無法清空，最後導致堆疊溢出。

---

#### 非同步行為與 Event Loop（事件循環）

雖然 JavaScript 是同步的，但它透過 **Event Loop（事件循環）** 和 **Callback Queue（回呼佇列）** 來處理非同步行為。非同步操作（如 `setTimeout` 或 `Promise`）不會直接加入 **Call Stack**，而是被排到 **Callback Queue**，等到 **Call Stack** 清空後才執行。

##### 範例：非同步操作的 Call Stack 流程

```javascript
console.log("1");

setTimeout(() => {
  console.log("2");
}, 1000);

console.log("3");
```

執行順序：

1. `console.log("1")` 加入 Call Stack，執行後移除。
2. `setTimeout` 加入 Call Stack，定時器啟動，回呼函式加入 **Callback Queue**，`setTimeout` 從 Call Stack 移除。
3. `console.log("3")` 加入 Call Stack，執行後移除。
4. 當 **Call Stack** 清空後，事件循環檢查 **Callback Queue**，將回呼函式加入 **Call Stack**，執行 `console.log("2")`。

執行結果：

```
1
3
2
```

---

#### 總結

- **Call Stack（呼叫堆疊）** 是 JavaScript 的核心機制，用來管理函式呼叫的執行順序。
- 它是同步的，採用 **LIFO（後進先出）** 原則。
- 過多的函式呼叫可能導致 **Stack Overflow（堆疊溢出）**。
- 非同步行為透過 **Event Loop（事件循環）** 和 **Callback Queue（回呼佇列）** 來進行管理。

了解 **Call Stack** 是理解 JavaScript 執行模型的基礎，尤其在處理非同步程式碼時非常重要。

**[🔝 Back to Top](#table-of-contents)**

### 14. Event Loop 事件迴圈

在 JavaScript 中，事件迴圈（Event Loop）是負責執行程式碼、收集和處理事件以及執行子任務（sub-tasks）的機制。JavaScript 是單執行緒的語言，這意味著同一時間只能執行一個任務。然而，透過事件迴圈，JavaScript 可以執行非同步操作（如 I/O 操作、定時器等）。

以下是事件迴圈的完整筆記：

#### 單執行緒（Single-threaded）

JavaScript 是單執行緒的，這意味著它一次只能執行一個任務。這樣的設計使得 JavaScript 在執行同步任務時非常高效，但在遇到需要等待的任務（如網路請求）時可能會阻塞。

#### 堆疊（Stack）

執行緒有一個堆疊（call stack），用來追蹤程式執行的位置。當函式被呼叫時，它會被推入堆疊中；函式執行完畢後，則會從堆疊中移除。

#### 任務隊列（Task Queue）

任務隊列（task queue）是存放待處理的非同步任務的地方。這些任務可能包括事件處理器（event handlers）、定時器（timers），或其他非同步操作。

#### 微任務（Microtasks）

微任務（microtasks）是指那些需要在目前執行的程式（script）的執行完畢後立即執行的任務，例如 Promise 的回調（callback）。微任務會在事件迴圈的每次迴轉（tick）結束後執行。

#### 宏任務（Macrotasks）

宏任務（macrotasks）是指那些需要在事件迴圈的下一次迴轉執行的任務，例如 setTimeout、setInterval、I/O 操作等。

#### 事件迴圈的運作流程

1. **執行堆疊中的任務**：事件迴圈會先執行堆疊中的同步任務。
2. **處理微任務**：執行完同步任務後，事件迴圈會處理所有的微任務。
3. **更新渲染**：如果微任務執行完畢，瀏覽器可能會更新渲染。
4. **執行宏任務**：事件迴圈會從任務隊列中取出宏任務並執行。
5. **重複過程**：事件迴圈會不斷重複上述過程。

#### 實例說明

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

執行順序：

1. `console.log('Start');`
2. `console.log('End');`
3. `console.log('Promise');`（微任務）
4. `console.log('Timeout');`（宏任務）

#### 結論

事件迴圈是 JavaScript 處理非同步行為的核心機制。理解事件迴圈的運作對於編寫高效且不會阻塞的 JavaScript 程式非常重要。透過事件迴圈，JavaScript 可以在單執行緒的限制下，優雅地處理大量的非同步操作。

**[🔝 Back to Top](#table-of-contents)**

### 15. Callback Queue 回調隊列

在 JavaScript 的非同步操作中，了解 Callback Queue（回調隊列）是非常重要的。它是事件循環（Event Loop）的一部分，負責處理非同步程式的執行順序。本筆記將完整整理關於 Callback Queue 的核心概念、運作方式及相關細節。

---

#### 什麼是 Callback Queue（回調隊列）？

Callback Queue 是一個專門用來儲存 **非同步回調函式（asynchronous callback functions）** 的隊列。當某些非同步操作完成後，對應的回調函式會被推入此隊列中，等待主執行緒（Main Thread）空閒時執行。

---

#### Callback Queue 的運作方式

Callback Queue 與事件循環（Event Loop）密切相關，其運作方式如下：

1. **非同步操作被觸發**  
   當程式執行非同步操作（如 `setTimeout`、HTTP 請求或 DOM 事件）時，回調函式不會立即執行，而是被委派給 Web APIs（瀏覽器或 Node.js 提供的底層功能）。

2. **回調函式進入 Callback Queue**  
   一旦非同步操作完成，例如計時器到期或伺服器回應完成，回調函式會被推入 Callback Queue。

3. **事件循環檢查**  
   事件循環會持續檢查 Call Stack（呼叫堆疊）是否為空。如果 Call Stack 為空，事件循環會從 Callback Queue 中取出第一個回調函式，並推入 Call Stack 中執行。

4. **回調函式執行**  
   回調函式在 Call Stack 中執行，處理完畢後，Call Stack 再次清空，事件循環繼續檢查 Callback Queue。

---

#### 與 Macro Task 和 Micro Task 的關係

JavaScript 的非同步隊列分為兩種類型：**Macro Task（巨任務）** 和 **Micro Task（微任務）**。它們的執行順序不同，並且會影響 Callback Queue 的行為。

- **Macro Task（巨任務）**  
  包括 `setTimeout`、`setInterval` 和 I/O（如 HTTP 請求）。當 Macro Task 完成後，回調函式會進入 Callback Queue。

- **Micro Task（微任務）**  
  包括 `Promise.then` 和 `MutationObserver`。Micro Task 的優先級高於 Macro Task，會直接進入 Micro Task Queue，並在每次事件循環完成後立即執行。

執行順序：

1. 先執行所有 Micro Task。
2. 再執行 Macro Task。

---

#### 範例：Callback Queue 的行為

以下範例展示 Callback Queue 的運作方式：

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout callback");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise callback");
});

console.log("End");
```

執行結果：

```
Start
End
Promise callback
Timeout callback
```

原因：

1. `Promise.then` 是 Micro Task，優先於 `setTimeout`（Macro Task）。
2. `setTimeout` 的回調函式進入 Callback Queue，需等待 Call Stack 清空後才執行。

---

#### 關鍵點整理

1. Callback Queue 儲存非同步操作完成後的回調函式。
2. 事件循環負責檢查 Call Stack 是否空閒，並從 Callback Queue 中取出回調函式執行。
3. Micro Task 的優先級高於 Macro Task。
4. 使用非同步程式設計時，需考慮回調函式的執行順序，以避免混淆。

---

**[🔝 Back to Top](#table-of-contents)**

### 16. Microtask Queue 微任務隊列

#### **什麼是 Microtask Queue？**

Microtask Queue（微任務隊列）是 JavaScript 執行環境中一種用於處理任務的隊列，它是事件循環（Event Loop）的一部分。微任務的執行優先級高於宏任務（Macrotask）。

---

#### **Microtask 的來源**

Microtask 是由以下操作或 API 產生的：

1. **Promise** 的 `.then()`、`.catch()` 和 `.finally()` 回調。
2. **MutationObserver**（變動監聽器）。
3. **queueMicrotask()** 方法。

---

#### **Microtask Queue 的特性**

1. **執行優先級高**  
   在事件循環中，Microtask Queue 會在每次執行完同步代碼後、處理下一個 Macrotask（如 setTimeout）的之前執行。換句話說，微任務的執行優先於宏任務。

2. **一個批次執行完所有微任務**  
   當事件循環進入 Microtask Queue 時，會清空隊列中的所有微任務，確保它們一次性執行完畢。

3. **與同步代碼緊密結合**  
   微任務的執行是在同步代碼結束後立即開始的，因此它們能夠快速響應。

---

#### **Microtask 與 Macrotask 的比較**

| 特性           | Microtask (微任務)             | Macrotask (宏任務)           |
| -------------- | ------------------------------ | ---------------------------- |
| **執行優先級** | 高                             | 低                           |
| **常見來源**   | Promise、queueMicrotask        | setTimeout、setInterval      |
| **執行時機**   | 同步代碼之後，下一個宏任務之前 | 每次事件循環的下一輪執行開始 |
| **執行範圍**   | 一次性清空所有微任務           | 每次執行一個宏任務           |

---

#### **事件循環（Event Loop）與 Microtask**

事件循環的執行順序如下：

1. 執行 **同步代碼**。
2. 執行 **Microtask Queue** 中的所有任務。
3. 執行 **Macrotask Queue** 中的第一個任務。
4. 重複步驟 2 和步驟 3。

---

#### **常見操作示例**

以下是一些與 Microtask Queue 相關的實際代碼示例：

1. **Promise 與 Microtask**

   ```javascript
   console.log("同步代碼開始");

   Promise.resolve().then(() => {
     console.log("微任務 1");
   });

   console.log("同步代碼結束");

   // 輸出順序：
   // 同步代碼開始
   // 同步代碼結束
   // 微任務 1
   ```

2. **queueMicrotask**

   ```javascript
   console.log("同步代碼開始");

   queueMicrotask(() => {
     console.log("微任務 1");
   });

   console.log("同步代碼結束");

   // 輸出順序：
   // 同步代碼開始
   // 同步代碼結束
   // 微任務 1
   ```

3. **Microtask 與 Macrotask 的比較**

   ```javascript
   console.log("同步代碼開始");

   setTimeout(() => {
     console.log("宏任務 1");
   }, 0);

   Promise.resolve().then(() => {
     console.log("微任務 1");
   });

   console.log("同步代碼結束");

   // 輸出順序：
   // 同步代碼開始
   // 同步代碼結束
   // 微任務 1
   // 宏任務 1
   ```

---

#### **queueMicrotask() 的使用**

`queueMicrotask()` 是一個專門用於向 Microtask Queue 添加任務的方法。它是一個全域方法，語法簡單，且執行效率高。

```javascript
queueMicrotask(() => {
  console.log("這是一個微任務");
});
```

**使用場景：**

- 當需要優先執行某些任務時，可以使用 `queueMicrotask()`。
- 比使用 `setTimeout` 更快速，且不會延遲。

---

#### **注意事項**

1. **避免無限遞歸**  
   如果在一個微任務中不斷添加新的微任務，可能會導致無限遞歸，阻塞事件循環。

   ```javascript
   function infiniteMicrotask() {
     queueMicrotask(infiniteMicrotask);
   }
   infiniteMicrotask(); // 將導致無限遞歸，程式崩潰
   ```

2. **與同步代碼的關係**  
   微任務的執行總是在同步代碼之後，因此若有大量的同步代碼，微任務可能會被延遲執行。

---

#### **常見問題**

1. **為什麼 Microtask 的執行優先於 Macrotask？**  
   這是因為 JavaScript 設計的目的是希望能快速響應細粒度的任務（如 Promise 的回調），而 Macrotask 通常用於處理較大型的操作（如計時器或 I/O 操作）。

2. **Microtask 與同步代碼的執行順序是什麼？**  
   同步代碼總是優先執行，其次是 Microtask，最後才是 Macrotask。

---

#### **總結**

- Microtask Queue 是 JavaScript 中一個高優先級的任務隊列。
- 主要來源包括 Promise、MutationObserver 和 queueMicrotask。
- Microtask 的執行時機是在同步代碼之後、下一個宏任務之前。
- 理解 Microtask 與 Macrotask 的執行順序，對於撰寫高效的非同步 JavaScript 程式至關重要。

**[🔝 Back to Top](#table-of-contents)**

### 17. Promises Promise 機制

## 什麼是 Promise？

`Promise` 是 JavaScript 中用來處理非同步操作（Asynchronous Operation）的物件。它可以幫助我們更有效率地管理非同步流程，避免傳統回呼函式（Callback Function）所造成的「回呼地獄」（Callback Hell）。

---

## Promise 的三種狀態（States）

1. **待定狀態（Pending）**  
   初始狀態，表示非同步操作尚未完成，也尚未有結果。

2. **已解決狀態（Fulfilled / Resolved）**  
   表示非同步操作已成功完成，並且返回了一個值（Result）。

3. **已拒絕狀態（Rejected）**  
   表示非同步操作發生錯誤或失敗，並返回一個原因（Reason）。

---

## Promise 的基本語法

```javascript
const promise = new Promise((resolve, reject) => {
  // 非同步操作
  if (成功) {
    resolve(結果); // 將 Promise 狀態改為 "已解決"
  } else {
    reject(原因); // 將 Promise 狀態改為 "已拒絕"
  }
});
```

---

## 使用 `.then()` 和 `.catch()`

1. `.then()`：用來處理已解決（Fulfilled）的結果。
2. `.catch()`：用來捕捉已拒絕（Rejected）的錯誤。

範例：

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve("成功的結果");
  } else {
    reject("發生錯誤");
  }
});

promise
  .then((result) => {
    console.log("成功:", result);
  })
  .catch((error) => {
    console.error("錯誤:", error);
  });
```

---

## 使用 `.finally()`

`.finally()` 方法無論 Promise 是成功還是失敗，最後都會執行某段程式碼，適合用於清理資源。

範例：

```javascript
promise
  .then((result) => {
    console.log("成功:", result);
  })
  .catch((error) => {
    console.error("錯誤:", error);
  })
  .finally(() => {
    console.log("完成操作");
  });
```

---

## Promise 的鏈式調用（Chaining）

多個 `.then()` 可以串聯起來形成鏈式調用，讓程式碼更具可讀性。

範例：

```javascript
new Promise((resolve, reject) => {
  resolve(1);
})
  .then((result) => {
    console.log(result); // 1
    return result + 1;
  })
  .then((result) => {
    console.log(result); // 2
    return result + 1;
  })
  .then((result) => {
    console.log(result); // 3
  })
  .catch((error) => {
    console.error("錯誤:", error);
  });
```

---

## Promise 的靜態方法（Static Methods）

JavaScript 提供了一些靜態方法來輔助操作多個 Promise。

### 1. `Promise.all()`

當所有的 Promise 都完成（Fulfilled）後，返回一個包含所有結果的陣列。如果有任一 Promise 被拒絕（Rejected），則整個結果被拒絕。

範例：

```javascript
const promise1 = Promise.resolve(1);
const promise2 = Promise.resolve(2);
const promise3 = Promise.resolve(3);

Promise.all([promise1, promise2, promise3])
  .then((results) => {
    console.log(results); // [1, 2, 3]
  })
  .catch((error) => {
    console.error("錯誤:", error);
  });
```

### 2. `Promise.race()`

返回第一個完成的 Promise（不論是 Fulfilled 還是 Rejected）。

範例：

```javascript
const promise1 = new Promise((resolve) => setTimeout(resolve, 100, "慢"));
const promise2 = new Promise((resolve) => setTimeout(resolve, 50, "快"));

Promise.race([promise1, promise2]).then((result) => {
  console.log(result); // "快"
});
```

### 3. `Promise.allSettled()`

等待所有 Promise 都完成（不論是 Fulfilled 還是 Rejected），然後返回每個 Promise 的結果物件。

範例：

```javascript
const promise1 = Promise.resolve(1);
const promise2 = Promise.reject("錯誤");
const promise3 = Promise.resolve(3);

Promise.allSettled([promise1, promise2, promise3]).then((results) => {
  console.log(results);
  // [
  //   { status: "fulfilled", value: 1 },
  //   { status: "rejected", reason: "錯誤" },
  //   { status: "fulfilled", value: 3 }
  // ]
});
```

### 4. `Promise.any()`

返回第一個成功（Fulfilled）的 Promise，如果所有 Promise 都被拒絕，則返回一個 AggregateError。

範例：

```javascript
const promise1 = Promise.reject("錯誤1");
const promise2 = Promise.reject("錯誤2");
const promise3 = Promise.resolve("成功");

Promise.any([promise1, promise2, promise3])
  .then((result) => {
    console.log(result); // "成功"
  })
  .catch((error) => {
    console.error(error); // 如果全部失敗，返回 AggregateError
  });
```

---

## 與 Async / Await 的結合

`Promise` 通常與 `async/await` 一起使用，使非同步程式碼更具同步化的可讀性。

範例：

```javascript
async function fetchData() {
  try {
    const result = await new Promise((resolve, reject) => {
      setTimeout(() => resolve("資料載入完成"), 1000);
    });
    console.log(result);
  } catch (error) {
    console.error("錯誤:", error);
  }
}

fetchData();
```

---

## 常見的錯誤處理

1. 忘記返回 `Promise` 物件，導致鏈式調用中斷。
2. 忘記使用 `.catch()` 或 `try/catch` 捕捉錯誤。
3. 在 `Promise` 中混用同步和非同步邏輯，導致難以追蹤錯誤。

範例：

```javascript
new Promise((resolve, reject) => {
  throw new Error("未捕捉的錯誤");
}).catch((error) => {
  console.error("捕捉到錯誤:", error);
});
```

---

## 總結

`Promise` 是 JavaScript 中處理非同步操作的核心工具，透過 `.then()`、`.catch()` 和 `.finally()` 來管理流程，並能與 `async/await` 高效結合。此外，透過靜態方法如 `Promise.all()`、`Promise.race()` 等，能輕鬆處理多個非同步操作的情境。學會 `Promise` 的使用，能大幅提升程式碼的可讀性與維護性。

**[🔝 Back to Top](#table-of-contents)**

### 18. Async/Await

#### 概述

`async/await` 是 JavaScript 中用來處理非同步程式碼 (asynchronous code) 的語法糖。它建立在 `Promises` 之上，使得非同步程式碼的撰寫更加簡潔和易讀。

#### 基本用法

1. **Async 函式**：在函式前加上 `async` 關鍵字，該函式就會自動回傳一個 `Promise`。這個函式可以包含 `await` 表達式。

   ```javascript
   async function myFunction() {
     return "Hello, World!";
   }

   myFunction().then((value) => console.log(value)); // 輸出: Hello, World!
   ```

2. **Await 表達式**：`await` 用於等待一個 `Promise` 完成。它只能在 `async` 函式內使用，並會暫停函式的執行直到 `Promise` 解決 (resolved)。

   ```javascript
   async function fetchData() {
     let response = await fetch("https://api.example.com/data");
     let data = await response.json();
     return data;
   }

   fetchData().then((data) => console.log(data));
   ```

#### 錯誤處理

在 `async/await` 中，可以使用 `try...catch` 語句來捕捉 `Promise` 中的錯誤。

```javascript
async function getData() {
  try {
    let response = await fetch("https://api.example.com/data");
    let data = await response.json();
    return data;
  } catch (error) {
    console.error("Error:", error);
  }
}

getData();
```

#### 與 Promise 的比較

- **可讀性**：`async/await` 提供了更接近同步程式碼的寫法，使程式碼更具可讀性和可維護性。
- **錯誤處理**：透過 `try...catch`，錯誤處理更直觀。

#### 注意事項

1. **阻塞 (Blocking)**：`await` 將暫停函式的執行，直到 `Promise` 完成。因此在一個 `async` 函式中，使用 `await` 時需小心避免阻塞其他操作。
2. **順序執行**：若需要並行執行多個非同步操作，應避免直接依次使用 `await`，而是可以考慮使用 `Promise.all()`。

   ```javascript
   async function loadData() {
     let [data1, data2] = await Promise.all([
       fetch("https://api.example.com/data1"),
       fetch("https://api.example.com/data2"),
     ]);

     // 處理 data1 和 data2
   }
   ```

#### 結論

`async/await` 為 JavaScript 的非同步程式碼提供了一種更簡潔、直觀的處理方式。理解其運作方式和適當使用場景，有助於提升程式碼的整體品質和效率。

**[🔝 Back to Top](#table-of-contents)**

### 19. JavaScript 引擎 如 V8 的架構

JavaScript 引擎是負責執行 JavaScript 程式碼的軟體，V8 是其中一個最流行的引擎，主要由 Google 開發，並用於 Chrome 瀏覽器和 Node.js。以下是 V8 引擎的基本架構：

#### 1. 語法分析器 (Parser)

- **語法分析器（Parser）**：將 JavaScript 原始碼轉換為抽象語法樹（Abstract Syntax Tree，AST）。
- **詞法分析（Lexical Analysis）**：將程式碼分解成更小的單位（tokens）。
- **語法分析（Syntax Analysis）**：檢查程式碼的結構，生成 AST。

#### 2. 解釋器 (Interpreter)

- **Ignition**：V8 的解釋器，負責將 AST 轉換為字節碼（Bytecode），便於執行。

#### 3. 編譯器 (Compiler)

- **即時編譯（Just-In-Time Compilation, JIT）**：在執行期間將字節碼進一步編譯為機器碼（Machine Code）。
- **TurboFan**：V8 的優化編譯器，負責將字節碼優化為高效的機器碼。
- **基線編譯（Baseline Compilation）**：一開始將字節碼快速轉換為機器碼。

#### 4. 垃圾回收 (Garbage Collection)

- **垃圾回收器（Garbage Collector）**：自動管理內存，回收不再使用的內存空間。
- **分代回收（Generational Garbage Collection）**：區分新生代（Young Generation）和老生代（Old Generation）物件，進行不同策略的回收。

#### 5. 執行環境 (Execution Environment)

- **執行堆疊（Call Stack）**：追踪函數呼叫和執行順序。
- **堆（Heap）**：用於分配動態記憶體的區域。
- **事件循環（Event Loop）**：負責處理非同步操作，確保非阻塞執行。

#### 6. 優化技術 (Optimization Techniques)

- **內聯快取（Inline Caching）**：優化函數呼叫的查找。
- **隱式類（Hidden Classes）**：優化物件屬性的存取。
- **逃逸分析（Escape Analysis）**：確定變量是否可以在堆上分配。

這些組件和技術共同作用，使得 JavaScript 引擎能夠高效地執行 JavaScript 程式碼。理解這些細節有助於開發者編寫更高效的 JavaScript 程式。

**[🔝 Back to Top](#table-of-contents)**
