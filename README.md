# interesting-code
Record some niubility code.

## Equality(==)

```js
if([]) console.log(123)
//123
if(![]) console.log(123)
//undefined
console.log([] == ![])
//true
```
In [developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)
>The equality operator converts the operands if they are **not of the same type**, then applies strict comparison. If both operands are objects, then JavaScript compares internal references which are equal when operands refer to the same object in memory.

No more info we could get from it.
### First ([] == false)
In [Ecma International, 11.9.3 The Abstract Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3) :
>If Type(y) is Boolean, return the result of the comparison x == ToNumber(y)

Obviously, The above-mentioned `([]==![])` is the same as point 7, cause `![]` is type of boolean. But what is `ToNumber` mean ? In [Ecma International, 9.3 ToNumber](http://www.ecma-international.org/ecma-262/5.1/#sec-9.3) , when the argument type is boolean:
> The result is 1 if the argument is true. The result is +0 if the argument is false.

So, in other words, that mean `([]==![])`(also is `([] == false)`) will be converted to `([] == +0)`. 

### Second ([] == +0)
Review the [Ecma International, 11.9.3 The Abstract Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3) :

>If Type(x) is Object and Type(y) is either String or Number,return the result of the comparison ToPrimitive(x) == y.

Absolutely, `([] == +0)` fit it. Here is the description of `ToPrimitive`

>Return a default value for the Object. The default value of an object is retrieved by calling the `[[DefaultValue]]` internal method of the object, passing the optional hint PreferredType. The behaviour of the `[[DefaultValue]]` internal method is defined by this specification for all native ECMAScript objects in 8.12.8.

Then we go and see what is the `[[DefaultValue]]` :
> When the [[DefaultValue]] internal method of O is called with hint String, the following steps are taken:
> 1. Let toString be the result of calling the [[Get]] internal method of object O with argument "toString".
> 2. If IsCallable(toString) is true then,
>    1. Let str be the result of calling the [[Call]] internal method of toString, with O as the this value and an empty argument list.
>    2. If str is a primitive value, return str.

So, in other words, that mean `([] == +0)` will be converted as below
```js
//1.
if([].toString)
    //2.1
    let str = [].toString()
    //2.2
    if(['undefined','null','boolean','number','string'].indexOf(typeof str) > -1)
        return str
``` 
As you see, the `([]==+0)` is converted to `(''==+0)`. 
### Finally ('' == +0)
Review the [Ecma International, 11.9.3 The Abstract Equality Comparison Algorithm](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3) point 5:
>If Type(x) is String and Type(y) is Number,return the result of the comparison ToNumber(x) == y.

In [Ecma International, 9.3 ToNumber](http://www.ecma-international.org/ecma-262/5.1/#sec-9.3) , when the argument type is string:
> The result is 1 if the argument is true. The result is +0 if the argument is false.

So long... In one word, `''` will return `0`(as below)
```js
Number('')
//0
```
That's all the reason why the comparison of `(0==0)` will return true.
## Array initialization

Init an array with { number : 1 },{ number : 2}.....{ number : 10}
```js 
    // in normal way
    var arr = [{ number : 1 },
               { number : 2 },
               { number : 3 },
               { number : 4 },
               { number : 5 },
               { number : 6 },
               { number : 7 },
               { number : 8 },
               { number : 9 },
               { number : 10}]
    
    // in smart way
    
    var brr = [];
    for(var i=0;i < 10;i++){
        brr.push({number:i+1})
    }
```
Well, if you want to zhuangbility, you may like this one.
```js
    var arr = [...Array(10)].map((v,i)=>({number:i+1}))
```

## Object copying

```js
    var obj = { 
        number : 1, 
        type : 'text', 
        subs : [1,2,3,4,5]
        //so on...
    } 
    
    // width jq
    var newObj = $.extend({},obj);
    
    // widthout jq
    //We need to go through all the node of obj (use for..in.. )
    
```

In zhuangbility way

```js
    var newObj = JSON.parse(JSON.stringify(obj))
```

## Asynchronous requests

```js
	// write in js without jq make me sick
	var xmlhttp;
	if (window.XMLHttpRequest) {
		// code for IE7+, Firefox, Chrome, Opera, Safari
		xmlhttp=new XMLHttpRequest();
	} else {
		// code for IE6, IE5
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange = function() {
		if (xmlhttp.readyState==4 && xmlhttp.status==200) {
			document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
		}
	}
	xmlhttp.open("GET","/ajax/demo_get.asp",true);
	xmlhttp.send();
    
	// width jq 
	// in much better way by using defered
	var deferred = jQuery.Deferred();
	deferred.then(function() {
	  console.log("first callback");
	  throw new Error("error in callback");
	})
	.then(function() {
	  console.log("second callback");
	}, function(err) {
	  console.log("rejection callback", err instanceof Error);
	});
	deferred.resolve();
    
```

In zhuangbility way

```js
	// fetch is still an experimental technology, so you should use babel first
	fetch("/submit", {
		method: "POST",
		headers: {
			"Content-Type": "application/x-www-form-urlencoded"
		},
		body: "firstName=Nikhil&favColor=blue&password=easytoguess"
	})
	.then(function(res) {
		// on response
	}, function(err) {
		// on error
	});

```

In more zhuangbility way. Use `then(fn).catch(fn)` is equal to `then(fn).then(null, fn)`, but `catch` make your code more clear than before.

```js
	// fetch is still an experimental technology, so you should use babel first
	fetch("/submit", {
		method: "POST",
		headers: {
			"Content-Type": "application/x-www-form-urlencoded"
		},
		body: "firstName=Nikhil&favColor=blue&password=easytoguess"
	})
	.then(function(res) {
		// on response
	})
	.catch(function(err) {
		// on error
	})
```
### Type checking

```js
	var someFn = function(){}
	//type checking
	if(typeof someFn === 'function'){
		//do something here
	}

```

In zhuangbility way

```js
	var someFn = function(){}
	var isFunction = {
		'function': true
	}
	if(isFunction[typeof someFn]){
		//do something here
	}
```