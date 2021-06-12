
this
所以this的指向的是目前呼叫函式或方法的擁有者(owner)物件，也就是說它與函式如何被呼叫或調用有關，雖然是同一函式的呼叫，因為不同的物件呼叫，也有可能是不同的this值。

所有的函式在呼叫時，其實都有一個擁有者物件來進行呼叫。所以你可以說，其實所有函式都是物件中的"方法"。所有的函式執行都是以Object.method()方式呼叫。

對this值來說，它根本不關心函式是在哪裡定義或是怎麼定義的，它只關心是誰呼叫了它。

call(呼叫): 以個別提供的this值與傳入參數值來呼叫函式。
bind(綁定): 建立一個新的函式，這個新函式在呼叫時，會以提供的this值與一連串的傳入參數值來進行呼叫。
apply(應用): 與call方法功能一樣，只是除了this值傳入外，另一個傳入參數值使用陣列。


this 的值跟作用域跟程式碼的位置在哪裡完全無關，只跟「你如何呼叫」有關


scope chain 
outer environment
``` javascript
function b() {
    console.log(myVar);
}


function a() {
    var myVar = 2;
    console.log(myVar);
    
	b();
}

var myVar = 1;
a();
```





closure
let 與 var

``` javascript
function a() {

    function b() {
        console.log(myVar);
    }
    	b();

    var myVar = 2;
    console.log(myVar);
    
	b();

    myVar = 3;
    b();
}

var myVar = 1;
a();
```

how javaScript runs

https://medium.com/jspoint/how-javascript-works-in-browser-and-node-ab7d0d09ac2f


Lexical Environments
https://github.com/lizhongzhen11/lizz-blog/issues/49


eventloop
https://medium.com/itsems-frontend/javascript-event-loop-event-queue-call-stack-74a02fed5625

scope 
https://www.cnblogs.com/leoo2sk/archive/2010/12/19/ecmascript-scope.html
https://github.com/nightn/front-end-plan/blob/master/js/js-scope.md
-----------

undefined
i++ ++i






https://github.com/lizhongzhen11/lizz-blog/issues/78#this

... ... 赋值表达式




-------------------------
# JavaScript Note

> 深度理解javascript運作原理




(1) JS是單線程語言

(2) JS的Event Loop是JS的執行機制。深入了解JS的執行,就等於深入了解JS里的event loop

javascript語言是單線程機制。所謂單線程就是按次序執行，執行完一個任務再執行下一個。
【同步】:按照一定的順序去執行，執行完一個才能執行下一個,會阻塞代碼執行
【異步】:不會阻塞代碼執行


## Syntax Parsers、Execution Contenxts、Lexical Environments

### Syntax Parser: Abstract Syntax Tree(AST)


Event Loop

https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf



exection context
https://ernieyang09.github.io/posts/javascript-context/
https://www.javascripttutorial.net/javascript-execution-context/
https://ernieyang09.github.io/posts/javascript-context/



https://www.jianshu.com/p/f862130fcdae



Us JavaScript programmers like to use words like, "event-loop", "non-blocking", "callback", "asynchronous", "single-threaded" and "concurrency".

JavaScript語言的一大特點就是單線程，也就是說，同一個時間只能做一件事。那麽，為什麽JavaScript不能有多個線程呢？這樣能提高效率啊。

JavaScript的單線程，與它的用途有關。作為瀏覽器腳本語言，JavaScript的主要用途是與用戶互動，以及操作DOM。這決定了它只能是單線程，否則會帶來很覆雜的同步問題。比如，假定JavaScript同時有兩個線程，一個線程在某個DOM節點上添加內容，另一個線程刪除了這個節點，這時瀏覽器應該以哪個線程為準？

所以，為了避免覆雜性，從一誕生，JavaScript就是單線程，這已經成了這門語言的核心特征，將來也不會改變。

為了利用多核CPU的計算能力，HTML5提出Web Worker標準，允許JavaScript腳本創建多個線程，但是子線程完全受主線程控制，且不得操作DOM。所以，這個新標準並沒有改變JavaScript單線程的本質。





JavaScript程式執行的確都在單一個執行緒(Single Thread)中的

javascript Syntax Parsers



How JavaScript works: an overview of the engine, the runtime, and the call stack

JavaScript運作機制 ： Engine， Runtime 和 Call Stack的概述








---

### Table of Contents

| No. | Questions |
|---- | ---------
|1  | [JavaScript prototypes](#JavaScript-prototypes) |
|2  | [What is prototype chain](#what-is-a-prototype-chain)|
|3  | [What is the difference between Call, Apply and Bind](#what-is-the-difference-between-call-apply-and-bind)|
|4  | [What is JSON and its common operations](#what-is-json-and-its-common-operations)|
|5  | [What is the purpose of the array slice method](#what-is-the-purpose-of-the-array-slice-method)|



1. ### JavaScript prototypes

   原型基礎物件導向

   1. **Object constructor:**

      The simplest way to create an empty object is using the Object constructor. Currently this approach is not recommended.

      ```javascript
      function Player() { }

      console.log(Player)
      console.log(Player.prototype)
      console.log(Player.prototype.constructor)
      console.log(Player.prototype.constructor === Player) //true
      ```

   2. **Object's create method:**

      The create method of Object creates a new object by passing the prototype object as a parameter

      ```javascript
      var object = Object.create(null);
      ```

   3. **Object literal syntax:**

      The object literal syntax is equivalent to create method when it passes null as parameter

      ```javascript
      var object = {};
      ```

   4. **Function constructor:**

      Create any function and apply the new operator to create object instances,

      ```javascript
      function Person(name){
         var object = {};
         object.name=name;
         object.age=21;
         return object;
      }
      var object = new Person("Sudheer");
      ```

   5. **Function constructor with prototype:**

      This is similar to function constructor but it uses prototype for their properties and methods,

      ```javascript
      function Person(){}
      Person.prototype.name = "Sudheer";
      var object = new Person();
      ```

      This is equivalent to an instance created with an object create method with a function prototype and then call that function with an instance and parameters as arguments.

      ```javascript
      function func {};

      new func(x, y, z);
      ```

      **(OR)**

      ```javascript
      // Create a new instance using function prototype.
      var newInstance = Object.create(func.prototype)

      // Call the function
      var result = func.call(newInstance, x, y, z),

      // If the result is a non-null object then use it otherwise just use the new instance.
      console.log(result && typeof result === 'object' ? result : newInstance);
      ```

   6. **ES6 Class syntax:**

      ES6 introduces class feature to create the objects

      ```javascript
      class Person {
         constructor(name) {
            this.name = name;
         }
      }

      var object = new Person("Sudheer");
      ```

   7. **Singleton pattern:**

      A Singleton is an object which can only be instantiated one time. Repeated calls to its constructor return the same instance and this way one can ensure that they don't accidentally create multiple instances.

      ```javascript
      var object = new function(){
         this.name = "Sudheer";
      }
      ```

      **[⬆ Back to Top](#table-of-contents)**

2. ### What is a prototype chain

    **Prototype chaining** is used to build new types of objects based on existing ones. It is similar to inheritance in a class based language. 
    
    The prototype on object instance is available through **Object.getPrototypeOf(object)** or **__proto__** property whereas prototype on constructors function is available through **Object.prototype**.

    ![Screenshot](images/prototype_chain.png)

    **[⬆ Back to Top](#table-of-contents)**

3. ### What is the difference between Call, Apply and Bind

    The difference between Call, Apply and Bind can be explained with below examples,

    **Call:** The call() method invokes a function with a given `this` value and arguments provided one by one

    ```javascript
    var employee1 = {firstName: 'John', lastName: 'Rodson'};
    var employee2 = {firstName: 'Jimmy', lastName: 'Baily'};

    function invite(greeting1, greeting2) {
        console.log(greeting1 + ' ' + this.firstName + ' ' + this.lastName+ ', '+ greeting2);
    }

    invite.call(employee1, 'Hello', 'How are you?'); // Hello John Rodson, How are you?
    invite.call(employee2, 'Hello', 'How are you?'); // Hello Jimmy Baily, How are you?
    ```

    **Apply:** Invokes the function with a given `this` value and allows you to pass in arguments as an array

    ```javascript
    var employee1 = {firstName: 'John', lastName: 'Rodson'};
    var employee2 = {firstName: 'Jimmy', lastName: 'Baily'};

    function invite(greeting1, greeting2) {
        console.log(greeting1 + ' ' + this.firstName + ' ' + this.lastName+ ', '+ greeting2);
    }

    invite.apply(employee1, ['Hello', 'How are you?']); // Hello John Rodson, How are you?
    invite.apply(employee2, ['Hello', 'How are you?']); // Hello Jimmy Baily, How are you?
    ```

    **bind:** returns a new function, allowing you to pass any number of arguments

    ```javascript
    var employee1 = {firstName: 'John', lastName: 'Rodson'};
    var employee2 = {firstName: 'Jimmy', lastName: 'Baily'};

    function invite(greeting1, greeting2) {
        console.log(greeting1 + ' ' + this.firstName + ' ' + this.lastName+ ', '+ greeting2);
    }

    var inviteEmployee1 = invite.bind(employee1);
    var inviteEmployee2 = invite.bind(employee2);
    inviteEmployee1('Hello', 'How are you?'); // Hello John Rodson, How are you?
    inviteEmployee2('Hello', 'How are you?'); // Hello Jimmy Baily, How are you?
    ```

    Call and apply are pretty interchangeable. Both execute the current function immediately. You need to decide whether it’s easier to send in an array or a comma separated list of arguments. You can remember by treating Call is for **comma** (separated list) and Apply is for **Array**. 
    
    Whereas Bind creates a new function that will have `this` set to the first parameter passed to bind().

    **[⬆ Back to Top](#table-of-contents)**

4. ### What is JSON and its common operations

    **JSON** is a text-based data format following JavaScript object syntax, which was popularized by `Douglas Crockford`. It is useful when you want to transmit data across a network and it is basically just a text file with an extension of .json, and a MIME type of application/json
    
    **Parsing:** Converting a string to a native object

    ```javascript
    JSON.parse(text)
    ```

    **Stringification:** converting a native object to a string so it can be transmitted across the network

    ```javascript
    JSON.stringify(object)
    ```

    **[⬆ Back to Top](#table-of-contents)**

5. ### What is the purpose of the array slice method

    The **slice()** method returns the selected elements in an array as a new array object. It selects the elements starting at the given start argument, and ends at the given optional end argument without including the last element. If you omit the second argument then it selects till the end.
    
    Some of the examples of this method are,

    ```javascript
    let arrayIntegers = [1, 2, 3, 4, 5];
    let arrayIntegers1 = arrayIntegers.slice(0,2); // returns [1,2]
    let arrayIntegers2 = arrayIntegers.slice(2,3); // returns [3]
    let arrayIntegers3 = arrayIntegers.slice(4); //returns [5]
    ```

    **Note:** Slice method won't mutate the original array but it returns the subset as a new array.

    **[⬆ Back to Top](#table-of-contents)**




* 從源代碼到語法樹—解析器從源代碼生成抽象語法樹（AST）
* 語法樹到字節碼— V8的解釋器Ignition從語法樹生成字節碼（此字節碼步驟在2017年之前不存在；此處描述了2017年之前的V8）
* 字節碼到機器代碼— V8的編譯器TurboFan從字節碼生成圖形，用高度優化的機器代碼替換字節碼的各個部分


* Source to syntax tree — The parser generates an abstract syntax tree (AST) from source
* Syntax tree to bytecode — V8’s interpreter, Ignition, generates bytecode from the syntax tree (this bytecode step was not present before 2017; the pre-2017 V8 is described here)
* Bytecode to machine code — V8’s compiler, TurboFan, generates a graph from bytecode, replacing sections of bytecode with highly optimized machine code


## Syntax Parsers、Execution Contenxts、Lexical Environments

### Syntax Parser: Abstract Syntax Tree(AST)




Syntax Parser就像是一個翻譯，負責將我們的Javascript Code 翻譯給電腦讀，進而執行程式。

所以當我們今天的Javascript Code執行時，會先經過Syntax Parser一字一句讀取我們的程式，並判斷這個語法是否有效，將有效的語法翻譯成電腦可以明白的方式。

### Lexical Environment
Lexical的原意為，與字詞或語法相關的。Lexical Environment在Javascript代表的是，今天我們Code寫在哪個 位置 ，是非常重要的。

``` javascript
function init() {
  var num = 123;
}
```

上面我們看到有一個變數，在函式裡面，這變數在字詞上來說就是坐落於、存在於這個函式，這隻函式`{ ... }` 就是一個Lexical Environment。

就如我們上面講的，我們寫的Code是會經過Syntax Parser翻譯的，而翻譯的過程它是對於這些字詞存在的位置很在意的，寫的位置是會影響存放於電腦記憶體中的位置，以及如何去與其他變數、函式、元素互動的。

所以當我們提到Lexical Environment中，就要注意到code是被寫在哪個段落位置以及被什麼涵蓋住。

Execution Context
一隻程式函式這麼多，到底要怎麼知道目前是哪段Code正在執行阿，搞得我好亂R，這時候我們就歡迎偉大的Execution Context，拯救我們的世界。

Execution Context就像是一張包裝紙，幫助管理哪段Code正在被執行。如同我們上面提到的，一隻程式會有許多個Lexical Environment，那哪一個先執行便是透過Execution Contexts管理。

而最基本的Execution Context就是 Global Execution Context，每隻Javascript程式一開始一定會有的傢夥，而這傢夥在一開始便替我們做兩件事，創造 Global Object & This 。

Global Object - 在Code裡面不管哪個元素不管在哪個段落都可以Access到的物件。
this - 之後會有篇幅說明





i++ = Use the value of i in statement then increase it by 1
++i = Increase the value of i by 1 then use in statement.


++x (pre-increment) means "increment the variable; the value of the expression is the final value"
x++ (post-increment) means "remember the original value, then increment the variable; the value of the expression is the original value"


a = 5;
b = 5;
c = ++a;
d = b++;
a is 6, b is 6, c is 6 and d is 5.


++i 先給i自加1，再計算表達式的值。
i++先計算表達式的值，再給i自加1。

可以對比一下下面這2個表達式：
var i = 1; (++i)+0; // 2
var i = 1; (i++)+0; // 1



execution-context 執行順序

https://pjchender.blogspot.com/2015/12/execution-context.html







併發數（concurrent request）



-----------------------------
翻譯文

https://blog.sessionstack.com/how-javascript-works-parsing-abstract-syntax-trees-asts-5-tips-on-how-to-minimize-parse-time-abfcf7e8a0c8

Javascript是如何工作的： Engine， Runtime 和 Call Stack的概述
https://www.jianshu.com/p/458930ff4778


event loop
https://cek.io/blog/2015/12/03/event-loop/

https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/eventloop.html

https://segmentfault.com/a/1190000012806637


1.ssssyoki《这一次，彻底弄懂 JavaScript 执行机制》

https://juejin.im/post/59e85eebf265da430d571f89#heading-9

2.阮一峰《JavaScript 运行机制详解：再谈Event Loop》

http://www.ruanyifeng.com/blog/2014/10/event-loop.html

3.ziwei3749 《深入理解JS引擎的执行机制》

https://segmentfault.com/a/1190000012806637

4.Jake《Tasks, microtasks, queues and schedules》

有動畫
https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/?utm_source=html5weekly


链接：https://www.jianshu.com/p/4de0e7ce30a7

https://juejin.cn/post/6844903512845860872#heading-9