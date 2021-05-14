
this
所以this的指向的是目前呼叫函式或方法的擁有者(owner)物件，也就是說它與函式如何被呼叫或調用有關，雖然是同一函式的呼叫，因為不同的物件呼叫，也有可能是不同的this值。

所有的函式在呼叫時，其實都有一個擁有者物件來進行呼叫。所以你可以說，其實所有函式都是物件中的"方法"。所有的函式執行都是以Object.method()方式呼叫。

對this值來說，它根本不關心函式是在哪裡定義或是怎麼定義的，它只關心是誰呼叫了它。

call(呼叫): 以個別提供的this值與傳入參數值來呼叫函式。
bind(綁定): 建立一個新的函式，這個新函式在呼叫時，會以提供的this值與一連串的傳入參數值來進行呼叫。
apply(應用): 與call方法功能一樣，只是除了this值傳入外，另一個傳入參數值使用陣列。


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



function bubble(list){
  let m = list.length
  for(let i = 0;i )
}

